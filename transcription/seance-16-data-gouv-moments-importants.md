# Séance 16 - Moments importants

## 00:00 - 02:14 - Objectif de la séance

Je pars de la liste de leads générée avec Serper / Google Maps et je veux l'enrichir avec data.gouv. L'objectif n'est pas de trouver des emails, mais de fiabiliser l'identité légale des entreprises : SIREN, SIRET, raison sociale, nom commercial, code APE, dirigeants, effectifs, finances publiques et statut actif.

## 02:14 - 05:35 - Architecture de l'enrichissement

La séance 16 devient la couche de vérité. Je relie le fichier `leads-massif`, l'API Adresse, l'API Recherche d'entreprises, un scoring de matching, puis un classeur final enrichi. Les statuts importants sont `HIGH_CONFIDENCE`, `MEDIUM_CONFIDENCE`, `ADDRESS_ONLY_CHECK`, `LOW_CONFIDENCE` et `NOT_FOUND`.

## 05:35 - 06:38 - Préparation dans Codex

Je garde le terminal ouvert dans le dossier du projet, je donne le dossier à Codex, je colle le prompt maître, puis je laisse l'agent travailler en autonomie. La qualité dépend de la précision du prompt et des corrections que je donne ensuite.

## 06:38 - 12:59 - Découverte de data.gouv

Je montre que data.gouv donne accès à des jeux de données publics, notamment la base SIRENE de l'INSEE. Je consulte les jeux de données, les APIs et les réutilisations pour comprendre ce qu'on peut exploiter gratuitement.

## 12:59 - 19:59 - Premier résultat insuffisant

Codex travaille, mais le premier enrichissement trouve peu de SIREN. Je ne m'arrête pas au premier résultat : je constate que la stratégie n'est pas suffisante et je demande une amélioration.

## 19:59 - 24:17 - Nouvelle stratégie

Codex propose de télécharger un dataset SIRENE local, de filtrer les établissements HORECA, puis de faire un matching local sur code postal, ville, nom et activité. Cette stratégie est plus lourde, mais beaucoup plus fiable.

## 24:17 - 33:44 - Matching local SIRENE

Le téléchargement et le filtrage prennent du temps. L'agent lance plusieurs monitors en parallèle. Je suis la progression, je corrige les erreurs et je relance quand nécessaire.

## 33:44 - 36:23 - Gain obtenu

Le matching local améliore fortement le résultat. Je passe d'un enrichissement faible à environ 10 000 SIREN trouvés sur 11 839 leads, avec des statuts de confiance exploitables.

## 36:23 - 42:43 - Ouverture du classeur

J'importe le fichier dans Google Sheets pour le lire plus facilement. Je me trompe de fichier au départ, ce qui montre l'importance d'une organisation stricte des dossiers et des noms.

## 42:43 - 44:51 - Vérification finale et suite

Je vérifie les colonnes `dg_*`, les statuts de matching et les liens vers les fiches légales. La base est prête pour la suite : enrichir les contacts avec Apify, récupérer davantage d'emails, puis ranger la base avant prospection.
