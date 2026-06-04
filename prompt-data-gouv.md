# Seance 16 - Prompt enrichissement data.gouv

```text
Tu es un data engineer B2B senior, spécialiste de l'enrichissement de bases de leads françaises avec les données publiques officielles.

MISSION
Je te donne un fichier Excel généré lors de la séance Serper :
@~/Desktop/mon-business/06-leads-gratuits/leads-serper.xlsx

Ta mission est de créer une version enrichie avec data.gouv et les APIs publiques françaises.
Le but n'est pas de trouver des emails ou des numéros de téléphone.
Le but est de fiabiliser l'identité légale de chaque lead :
- SIREN
- SIRET
- raison sociale
- nom commercial officiel
- code APE / NAF
- statut actif ou fermé
- date de création
- tranche d'effectif
- catégorie entreprise
- dirigeants publics quand disponibles
- chiffre d'affaires et résultat net quand disponibles
- adresse officielle
- latitude / longitude
- score de confiance du matching

APIS AUTORISÉES
1. API Adresse :
https://api-adresse.data.gouv.fr/search/

2. API Recherche d'entreprises :
https://recherche-entreprises.api.gouv.fr/search
https://recherche-entreprises.api.gouv.fr/near_point

Tu dois respecter les limites raisonnables d'appel.
Mets un délai court entre les appels.
Cache les réponses JSON pour ne pas répéter les mêmes requêtes.
Ne contourne aucune restriction.

FICHIER D'ENTRÉE
Lis l'onglet principal du fichier :
~/Desktop/mon-business/06-leads-gratuits/leads-serper.xlsx

Colonnes importantes à utiliser si elles existent :
- lead_id
- company_name
- segment
- category
- website
- domain
- email
- phone
- address
- city
- postal_code
- google_rating
- google_review_count
- cid
- source_urls
- source_queries
- source_endpoints
- priority_score
- priority_class
- personalized_angle
- outreach_message

CONTRAINTE IMPORTANTE
Tu dois conserver toutes les colonnes existantes.
Tu ne dois jamais écraser email, phone, website, source_urls ou outreach_message.
Tu ajoutes uniquement des colonnes avec le préfixe dg_.

COLONNES À AJOUTER
Ajoute au minimum :
- dg_siren
- dg_siret
- dg_raison_sociale
- dg_nom_commercial
- dg_code_ape
- dg_activite
- dg_section_activite
- dg_statut
- dg_date_creation
- dg_tranche_effectif
- dg_categorie_entreprise
- dg_dirigeant_nom
- dg_dirigeant_role
- dg_ca_dernier
- dg_ca_annee
- dg_resultat_net_dernier
- dg_resultat_net_annee
- dg_adresse_officielle
- dg_code_postal_officiel
- dg_ville_officielle
- dg_latitude
- dg_longitude
- dg_distance_m
- dg_match_score
- dg_match_status
- dg_match_method
- dg_source_url
- dg_notes

STRUCTURE DE SORTIE
Crée ou complète ce dossier :
~/Desktop/mon-business/07-enrichissement-data-gouv/

Crée :
- leads-data-gouv-enriched.xlsx
- leads-data-gouv-enriched.csv
- rapport-data-gouv.md
- data-gouv-cache/
- data-gouv-cache/adresse/
- data-gouv-cache/search/
- data-gouv-cache/near-point/
- data-gouv-ledger.csv

MÉTHODE PAR LIGNE

PHASE 1 — NORMALISATION
Pour chaque lead :
1. Nettoie company_name :
   - supprime les emojis
   - retire les suffixes marketing inutiles
   - garde aussi le nom original
2. Nettoie address et city.
3. Si postal_code est vide, ne l'invente pas. Il sera récupéré via géocodage.
4. Normalise domain depuis website si besoin.
5. Crée une clé de cache stable par lead_id.

PHASE 2 — GÉOCODAGE ADRESSE
Si address et city existent :
Appelle l'API Adresse avec :
q = address + city
limit = 1

Récupère :
- adresse normalisée
- code postal
- code commune
- latitude
- longitude
- score BAN

Si l'API Adresse ne trouve rien :
- continue quand même avec recherche par nom + ville
- note dg_notes = "adresse non géocodée"

PHASE 3 — RECHERCHE TEXTUELLE ENTREPRISE
Interroge l'API Recherche d'entreprises avec plusieurs stratégies :

Stratégie A :
q = company_name + city
code_postal = code postal disponible
etat_administratif = A

Stratégie B :
q = company_name nettoyé
code_postal = code postal disponible
etat_administratif = A

Stratégie C :
q = company_name + address + city
etat_administratif = A

Pour les restaurants, bars, traiteurs, cafés ou hôtels-restaurants :
ajoute si utile :
section_activite_principale = I
et priorise les codes :
- 56.10A
- 56.10B
- 56.10C
- 56.30Z
- 55.10Z seulement si le lead est hôtel ou hébergement

PHASE 4 — RECHERCHE GÉOGRAPHIQUE
Si le matching textuel est faible mais que tu as latitude/longitude :
Appelle :
/near_point
lat = latitude BAN
long = longitude BAN
radius = 0.08 à 0.20 km selon densité
section_activite_principale = I si le lead est restaurant / bar / traiteur

Attention :
Un résultat à la même adresse ne suffit pas.
Une adresse peut contenir plusieurs sociétés.
Le résultat géographique doit être comparé au nom commercial, aux enseignes, à l'activité et au statut.

PHASE 5 — SCORING DE MATCHING
Calcule dg_match_score sur 100 :
- +35 si le nom commercial, l'enseigne ou la raison sociale ressemble fortement au company_name
- +20 si une partie distinctive du nom est retrouvée
- +20 si l'adresse correspond
- +15 si distance inférieure à 50 mètres
- +10 si code APE cohérent avec le secteur
- +5 si établissement actif
- -25 si la seule preuve est l'adresse
- -30 si le code APE est manifestement incohérent
- -40 si l'entreprise est fermée

Définis dg_match_status :
- HIGH_CONFIDENCE : score >= 85 ET nom/enseigne cohérent ET adresse cohérente
- MEDIUM_CONFIDENCE : score 65 à 84 avec cohérence raisonnable
- ADDRESS_ONLY_CHECK : adresse ou distance cohérente mais nom différent
- LOW_CONFIDENCE : score inférieur à 65 ou forte ambiguïté
- NOT_FOUND : aucun résultat exploitable

PHASE 6 — EXTRACTION DES DONNÉES
Pour le meilleur candidat :
Récupère :
- siren
- siret de l'établissement correspondant
- nom_complet
- nom_raison_sociale
- nom_commercial
- activité principale
- section activité
- état administratif
- date création
- tranche effectif
- catégorie entreprise
- adresse officielle
- latitude / longitude officielle
- dirigeants
- finances si présentes

Dirigeants :
Priorise les rôles :
1. Président
2. Gérant
3. Directeur général
4. Représentant légal

Ne mets pas les commissaires aux comptes comme décideur commercial.
Si plusieurs dirigeants pertinents existent, garde le principal dans dg_dirigeant_nom et note les autres dans dg_notes.

Finances :
Si plusieurs années existent, prends la plus récente.
Renseigne :
- dg_ca_dernier
- dg_ca_annee
- dg_resultat_net_dernier
- dg_resultat_net_annee

PHASE 7 — CLASSEUR EXCEL
Crée leads-data-gouv-enriched.xlsx avec ces onglets :

1. DATA_GOUV_DASHBOARD
KPIs :
- total leads
- enrichis avec SIREN
- enrichis avec SIRET
- HIGH_CONFIDENCE
- MEDIUM_CONFIDENCE
- ADDRESS_ONLY_CHECK
- LOW_CONFIDENCE
- NOT_FOUND
- taux d'enrichissement
- taux prêt pour Apify

Graphiques :
- répartition dg_match_status
- top codes APE
- top villes officielles
- effectifs par tranche

2. ALL_LEADS_ENRICHED
Toutes les lignes d'origine + colonnes dg_.

3. READY_FOR_APIFY
Seulement :
dg_match_status = HIGH_CONFIDENCE ou MEDIUM_CONFIDENCE
et website ou domain non vide.

4. TO_VERIFY
Toutes les lignes :
MEDIUM_CONFIDENCE
ADDRESS_ONLY_CHECK
LOW_CONFIDENCE
avec dg_notes et la raison de doute.

5. NOT_FOUND
Lignes sans match fiable.

6. DIRIGEANTS
Une ligne par dirigeant trouvé :
lead_id, company_name, dg_siren, dg_siret, nom, rôle, type_dirigeant, source.

7. FINANCES
lead_id, company_name, dg_siren, année, CA, résultat net.

8. DATA_GOUV_LEDGER
Toutes les requêtes :
timestamp, lead_id, api, query, status, result_count, cache_file.

9. SCORING_RULES
Explique clairement la méthode de matching.

MISE EN FORME
- filtres sur les tableaux
- première ligne figée
- colonnes lisibles
- couleurs par dg_match_status
- aucune clé ou donnée secrète
- notes claires pour les lignes à vérifier

RAPPORT MARKDOWN
Crée rapport-data-gouv.md avec :
- résumé de la méthode
- nombre de leads traités
- nombre de SIREN trouvés
- nombre de SIRET trouvés
- taux HIGH_CONFIDENCE
- top erreurs de matching
- recommandations pour améliorer le fichier source
- limites de data.gouv
- suite recommandée : enrichissement Apify

CONFORMITÉ
N'utilise que les données publiquement diffusées par les APIs.
Respecte les statuts de diffusion partielle.
Ne collecte aucune donnée sensible.
Ne transforme pas ces données en spam.
Garde la source de chaque donnée.

CONTRÔLE FINAL
Avant de finir :
- vérifie que le XLSX s'ouvre
- vérifie que les colonnes dg_ existent
- vérifie que READY_FOR_APIFY ne contient que des leads avec site ou domaine
- vérifie que TO_VERIFY contient les cas ambigus
- vérifie que dg_match_score est entre 0 et 100
- vérifie qu'aucune colonne d'origine n'a été supprimée

MESSAGE FINAL
Réponds seulement avec :
"Enrichissement data.gouv terminé.
Fichiers :
- leads-data-gouv-enriched.xlsx
- leads-data-gouv-enriched.csv
- rapport-data-gouv.md

Leads traités : X
SIREN trouvés : Y
SIRET trouvés : Z
HIGH_CONFIDENCE : A
MEDIUM_CONFIDENCE : B
À vérifier : C
Non trouvés : D
Prêts pour Apify : E"
```
