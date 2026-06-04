# Séance 16 - Transcription brute

- Durée détectée : 44:51
- Langue : fr

## Segments

### 00:00 - 00:06
C'est parti, on attaque du coup maintenant la séance 16, on va enrichir avec DataGouv.

### 00:06 - 00:13
Donc on va essayer de récupérer le CRN, le dirigeant, le CA et la fiabilité.

### 00:13 - 00:21
Donc en gros la liste qu'on a générée lors de la séance précédente,

### 00:21 - 00:28
donc au niveau du XLSX, l'élite qu'on a scrapté avec Google Maps,

### 00:28 - 00:37
et bien on va essayer d'enrichir tous ces leads-là grâce à l'API gratuite de l'État, donc Datagoof.

### 00:37 - 00:40
On va voir ça ensemble, donc c'est pas bien complexe encore une fois.

### 00:40 - 00:45
On va partir d'un système de prompt. Pourquoi ? Parce que là on n'aura même pas besoin d'écrire l'API.

### 00:45 - 00:53
Là on aura juste besoin de lui donner des indications sur comment bien utiliser cet API qui est public,

### 00:53 - 00:58
elle mène pour tout le monde donc nous notre objectif ça va être

### 00:58 - 01:04
d'enrichir ces 10 milles là donc ces 10 milles ça va prendre pas mal de temps

### 01:04 - 01:07
donc comme vous avez pu le voir pour Scrapte avec Google Maps on a pris un

### 01:07 - 01:13
peu près ouais une bonne heure là je pense que ça sera un peu moins mais

### 01:13 - 01:19
ça va enrichir toute cette liste donc c'est le top du top donc c'est

### 01:19 - 01:27
parti. On va voir un petit peu de théories. Donc DataGouv, encore une fois, il ne va pas

### 01:27 - 01:32
trouver des contacts, il va fiabiliser les entreprises qu'on a déjà scraptées.

### 01:32 - 01:37
Donc ce que DataGouv va nous apporter, c'est le siren, le siret, la raison sociale, le

### 01:37 - 01:40
nom commerciale, la PE, le dirigeant, l'effectif, les finances publiques et

### 01:40 - 01:42
les statuts actifs. Ça, ça va être important.

### 01:42 - 01:48
Encore une fois, notre objectif, c'est quand on enrichit le lead, c'est qu'à la fin,

### 01:48 - 01:53
le message qu'on va envoyer pour la prospection sera extrêmement personnalisé. Donc c'est pour ça

### 01:53 - 01:58
qu'on fait cette étape là. On cherche de l'enrichissement constant. Et ensuite l'étape

### 01:58 - 02:04
prochaine, ça va être la partie API. On va utiliser une clé API payante pour récupérer

### 02:04 - 02:09
l'adresse email pour qu'on puisse envoyer ce message de prospection. Donc la séance 16,

### 02:09 - 02:14
elle est du coup la couche de vérité. On va récupérer le fichier juste ici,

### 02:14 - 02:22
donc leads massif, on va récupérer l'api-adresse, donc l'api-adresse data-goo, on va faire un

### 02:22 - 02:26
recherche d'entreprise, on va faire un scoring match, on va faire un to-veil file et on va faire

### 02:26 - 02:34
ensuite une mise à jour de d'autres fichiers xlsx. Donc deux api suffisent pour faire un

### 02:34 - 02:40
promis enrichissement solide, donc nettoyer et géocoder et trouver l'entité légale. Donc

### 02:40 - 02:45
c'est ces deux API là qu'on va utiliser donc le prompt repart du fichier

### 02:45 - 02:50
serper comme on l'a vu donc colonne serper, colonne compagnie name, adresse

### 02:50 - 02:56
city, website domain, catégorie segment, email phone donc encore une fois on n'a pas

### 02:56 - 03:00
beaucoup d'emails et phones donc on va surtout se concentrer sur ces quatre

### 03:00 - 03:06
colonnes là donc c'est grâce à ça qu'on va pouvoir faire du matching et

### 03:06 - 03:13
trouver du coup les informations manquantes et enrichir un maximum notre fichier en question.

### 03:13 - 03:18
Donc les colonnes qu'on va ajouter c'est la partie identité, la partie activité, la partie taille,

### 03:18 - 03:25
la partie matching. Donc en plus de toutes les colonnes qu'on a là, on aura du coup ces

### 03:25 - 03:32
quatre colonnes supplémentaires. Donc le danger au niveau du matching c'est un restaurant peut

### 03:32 - 03:39
exploiter, en l'occurrence un restaurant, mais vous pouvez avoir la même problématique que

### 03:39 - 03:44
moi, peut être exploité par une société dont le juste juridique est différent. Mais une même

### 03:44 - 03:51
adresse peut aussi contenir plusieurs sociétés. Le Pondoc doit croiser nos enseignes, adresses

### 03:51 - 03:57
distantes, codes APE et statuaires actifs. Donc c'est pour ça qu'on va faire tout simplement

### 03:57 - 04:07
un score par leads croisés. Donc la base, encore une fois, de DataGouv doit dire quoi vérifier.

### 04:07 - 04:13
Donc hide confidence, median confidence, address and need check, low confidence, mode found.

### 04:13 - 04:21
Donc au niveau de la définition, hide confidence c'est nom ou enceinte cohérente plus adresse

### 04:21 - 04:27
cohérente plus activité cohérente. Donc là on peut passer à PIFI. Ensuite medium

### 04:27 - 04:33
confidence, non partiellement cohérent ou société d'exploitation probable.

### 04:33 - 04:39
On peut passer à PIFI mais garder une note. Donc adresse only check, donc adresse bonne mais

### 04:39 - 04:42
non différent ou ambiguë, vérification manuelle. Ça sera à nous d'aller checker.

### 04:42 - 04:47
On pourra le faire en MCP ou autrement. Donc low confidence, résultat faible ou trop loin,

### 04:47 - 04:53
ne pas contacter à vos corrections. Notre fund, aucun match fiable, recherchez

### 04:53 - 04:57
manuellement ou exclure. Donc nous, quand on va passer sur l'enrichissement

### 04:57 - 05:01
final, il faut qu'on ait soit du médium soit du height, comme ça on est sûr que

### 05:01 - 05:07
le leads il est intéressant. Donc les résultats attendus, encore une fois, c'est

### 05:07 - 05:11
le CIREL, la PE et le CA quand les finances sont disponibles. Ça c'est pas

### 05:11 - 05:17
encore disponible constamment, sinon il faudra utiliser PAYPERS et PAYPERS c'est

### 05:17 - 05:21
payants c'est extrêmement cher. Là on cherche vraiment à créer une base de leads

### 05:21 - 05:26
solides en full free. Donc même Happyfile avec un compte gratuit de 5

### 05:26 - 05:32
dollars on pourra enrichir les leads qu'on a passé en high confidence et

### 05:32 - 05:35
medium confidence. Donc la préparation avant de coller le

### 05:35 - 05:43
prompt, encore une fois on passe sur notre dossier, mon business, mon

### 05:43 - 05:50
business, Prospect, clique gauche, nouveau terminal au dossier. Si vous n'avez pas

### 05:50 - 05:56
fermé la session d'avant, vous avez scrapté les leads via Google Maps.

### 05:56 - 06:05
Moi, en l'occurrence, j'ai toujours mon terminal ouvert à cloud code où j'ai

### 06:05 - 06:09
scrapté les leads avec Google Maps. Donc je peux continuer dessus. Il est bien

### 06:09 - 06:14
connecté à 06 Prospect, j'ai bien dedans mon dossier leads avec toutes

### 06:14 - 06:19
les informations. Donc maintenant ce que je dois faire, c'est donner le dossier du

### 06:19 - 06:30
coup à Cloud Code, copier le prompt, le coller et ensuite tout simplement taper

### 06:30 - 06:38
entrée. Là il a toutes les informations pour travailler en autonomie. Donc on

### 06:38 - 06:43
va voir un petit peu en détail la partie data-gouv-leads.

### 06:43 - 06:49
Là on peut voir mon article, c'est top, il est bien positionné, donc c'était un lit de maniètes.

### 06:49 - 06:56
Donc data.google.fr, donc là on va ici.

### 06:56 - 07:01
Et là on a un jeu de données IAPI de 74000, donc informations.

### 07:01 - 07:05
On a la base sérène qu'on va utiliser avec INSEE.

### 07:05 - 07:10
On a le répertoire national des élus, ça on va pas l'utiliser encore une fois, c'est pas ce qui nous intéresse.

### 07:10 - 07:19
Mais on a des informations super intéressantes, qui sont totalement gratuites et en plus de ça, c'est l'état qui nous les mentionne.

### 07:19 - 07:22
Donc c'est vraiment très utile.

### 07:22 - 07:28
Donc demande de valeur foncière, base de sirene des entreprises, c'est ça qu'on veut utiliser.

### 07:28 - 07:33
Donc l'API elle est publique, on n'est pas obligé d'en créer une.

### 07:33 - 07:40
Donc là on peut avoir la description, l'institut national des statistiques économiques,

### 07:40 - 07:44
collecte, produit, analyse et diffuse des informations sur l'économie et les

### 07:44 - 07:49
sociétés françaises. C'est en fait, je ne sais pas pourquoi

### 07:49 - 07:54
il nous donne toutes ces informations, en tout cas c'est une mine d'or, c'est une

### 07:54 - 07:57
mine d'or parce que vous n'êtes pas obligé de payer des services comme

### 07:57 - 08:02
Apporio pour enrichir vos leads. Là on se base vraiment sur quelque chose

### 08:02 - 08:08
qui est à jour constamment et qui est efficace quoi. Une demande à

### 08:08 - 08:16
pays comme là on est en train de faire et puis on fait du matching, on croise les données et ensuite

### 08:16 - 08:22
on a les informations manquantes donc si vous voulez voir un peu plus d'informations vous allez

### 08:22 - 08:29
là directement sur ce lien je vous le mettrais dans le support technique donc data.group.fr,

### 08:29 - 08:35
data set, vous pouvez voir que là on a plusieurs jeux de données comme on avait tout à l'heure

### 08:35 - 08:41
Donc là on a la base sirène, on peut faire vraiment tout et n'importe quoi.

### 08:41 - 08:48
Il y a trop de choses à faire, on peut cartographier des indicateurs sur l'écrit

### 08:48 - 08:55
Médélis, on peut le récupérer par API. On peut vraiment faire un éliminément de choses,

### 08:55 - 09:01
surtout si on connecte directement un système comme CloCode, les possibilités

### 09:01 - 09:06
sont assez énormes, on peut vraiment s'amuser sur les données, on peut comprendre pas mal de choses

### 09:06 - 09:13
sur ce qui se passe en France. Avant c'était assez complexe parce qu'on devait, je sais plus

### 09:13 - 09:19
qu'on récupérait ça, parce que maintenant ça a changé, avant c'était pas le même site,

### 09:19 - 09:28
il n'était pas brandé pareil, donné le valeur foncière, on peut voir que là depuis juillet

### 09:28 - 09:40
2022, il y a 1,8 million d'utilisation, de licence ouverte et franchement ça

### 09:40 - 09:44
accessible partout, pour tout le monde, c'est ça qui est top, il n'y a pas de

### 09:44 - 09:53
créer un compte ou quoi que ce soit, voilà ça va prendre 25 à 40 minutes

### 09:53 - 10:01
ah voilà c'est ça comme on peut voir on a les fichiers juste ici qu'on peut

### 10:01 - 10:05
récupérer, on peut le télécharger. Je ne vais pas le faire parce que c'est des

### 10:05 - 10:12
fichiers qui sont assez énormes. On peut faire la réutilisation API.

### 10:12 - 10:28
Regardez, il y a des gens qui font des sas. Corentin d'avio en full. IA, ils sont

### 10:28 - 10:30
disponibles directement sur le site de l'Etat.

### 10:30 - 10:35
On va aller voir la réutilisation. Ça, je peux vous assurer que c'est un site fait par

### 10:35 - 10:38
Claude. Ce n'est pas le kenmodel mais sûrement

### 10:38 - 10:59
4.5 peut-être, ouais c'est pas mal. On va voir d'autres utilisations, lui aussi

### 10:59 - 11:06
il s'est fait avec Delia, carrément je pourrais me faire un petit site, ça

### 11:06 - 11:13
paie pas mal pour le SEO aussi pour renforcer votre expertise, Loic Ployard

### 11:13 - 11:21
Loic Ployard, réutilisation, il arrive directement, c'est pas mal ça

### 11:21 - 11:24
pourquoi pas avoir ça peut être intéressant de faire un petit site

### 11:24 - 11:34
rapide avec la PIDATA Gouvre et ensuite de le positionner du coup sur cette

### 11:34 - 11:37
partie là au niveau de la réutilisation ça va bien vous placer

### 11:37 - 11:44
vous pouvez vraiment tout faire je vous invite à checker un peu toutes les

### 11:44 - 11:48
données qu'on peut récupérer parce qu'il y a vraiment de tout

### 11:48 - 11:53
vous pouvez faire pas mal de choses c'est assez intéressant maintenant c'est

### 11:53 - 11:56
l'un des sites que j'utilise le plus pour mes clients pour aller scraper du

### 11:56 - 12:00
leads ou pour les enrichir. En fait il y a toutes les informations de la

### 12:00 - 12:04
France. C'est vraiment une plus value. J'avais fait un post d'ailleurs une

### 12:04 - 12:10
codeine dessus qui avait explosé où je donnais une méthode. Un post qu'on est en train

### 12:10 - 12:18
de voir là actuellement. D'ailleurs le post où on a fait

### 12:18 - 12:23
des lignes maniètes là ensemble il a assez bien fonctionné. J'ai changé un petit

### 12:23 - 12:30
peu le texte mais on a fait 457 commentaires en quatre heures. On a 22000 impressions c'est pas

### 12:30 - 12:38
mal j'ai déjà reçu un appel et deux calls sur mon calendrier et c'était du coup celui-là là.

### 12:38 - 12:45
J'ai fait 3404 posts et j'expliquais justement comment récupérer des entreprises etc. Vous

### 12:45 - 12:49
pouvez faire exactement la même chose sauf que là nous on est en train de parler d'enrichissement.

### 12:49 - 12:55
Donc ici vous voulez récupérer des leads et ensuite les enrichir, vous pouvez très

### 12:55 - 12:58
bien passer en premier sur data gov sauf que vous chargez le prompt et vous envoyez

### 12:58 - 12:59
une autre demande.

### 12:59 - 13:13
Hop, donc là il est en train de travailler, c'est top, là il a fait, ok, 17%, 205

### 13:13 - 13:20
croguer, état 7 minutes, après ça va faire une boucle, il est à 50%, ok,

### 13:20 - 13:26
on va le laisser travailler mais vous avez compris l'objectif quoi on peut

### 13:26 - 13:28
faire des recherches par sirène après c'est ce qu'on va faire

### 13:28 - 13:33
il y en a un dans le prompt, je peux faire la réutilisation, c'est ce qu'on a vu

### 13:33 - 13:37
les gens ce qui sont faits c'est quoi ça ?

### 13:37 - 13:48
je vais les voir ok c'est pas mal bon ça aussi c'est du full codex ou

### 13:48 - 13:52
credit gpt c'est rien je pense c'est du full codex le design c'est pas du

### 13:52 - 13:56
code code et voilà les gens ils mettent un petit peu leur

### 13:56 - 14:02
création, c'est intéressant honnêtement. On peut voir pas mal de... c'est pas mal

### 14:02 - 14:08
fait hein, pas mal fait. Donc pour l'essieu, je vous conseille quand même de faire un

### 14:08 - 14:17
petit projet comme ça, ça peut être pas mal du tout pour placer autrement en

### 14:17 - 14:23
freelance ou d'agence ou d'expertise, ça peut être cool. Je sais pas si ils acceptent

### 14:23 - 14:27
tout le monde par contre ça reste à voir. Je pense que si vous êtes à tête des

### 14:27 - 14:32
projets qui ne sont pas bien complexes à faire, on va apprendre ces faisables. On a

### 14:32 - 14:38
la partie organisation, jeux de données. Je ne sais plus où on me voit, là c'est tout ce

### 14:38 - 14:45
qu'on peut faire avec les API. Il y en a énormément. Vraiment, amusez-vous.

### 14:45 - 14:56
Testez un peu les jeux de données disponibles.

### 14:56 - 15:05
vous pouvez vraiment tout faire. Donc on va voir où il en est, notre code code, votre session,

### 15:05 - 15:20
là il est en train de se faire avec deux monitors qui sont en train de progresser en même temps,

### 15:20 - 15:28
plus vous allez lui laisser de temps avec votre code pour enrichir du coup vos leads,

### 15:28 - 15:33
plus il va faire quelque chose de qualitatif. Donc à la fin de ce système, on est en train

### 15:33 - 15:38
de voir là ou de reprendre que je lui ai envoyé, je pourrais lui demander d'aller chercher d'autres

### 15:38 - 15:43
jeux de données pour enrichir un maximum et plus d'informations. C'est le temps que vous allez

### 15:43 - 15:49
consacrer à votre base de lit, ce qui va vous permettre d'en avoir une de plus en plus.

### 15:49 - 16:56
Je lui demande il reste combien de temps selon toi. Donc vous avez vu à chaque séance, sur

### 16:56 - 17:06
chaque séance plutôt, on utilise un système de prompt et ensuite on construit. Et après on

### 17:06 - 17:14
on avise, comme la première séance qu'on a vu sur Code Code, la stratégie. Vous pouvez vraiment tout

### 17:14 - 17:19
construire. Vous avez les capacités je pense, après la compréhension que vous avez eu au fur et à

### 17:19 - 17:25
mesure des projets, et des améliorations que vous avez dû faire avec par exemple le blog

### 17:25 - 17:33
automatique, la partie c'est internet, faire un petit peu maintenant ce que vous voulez. Le but

### 17:33 - 17:39
c'est vraiment aussi de prendre une expertise de votre propre expérience dans le sens où vous pouvez

### 17:39 - 17:46
vous donner l'objectif de projet et ensuite le construire de votre côté avec des systèmes de

### 17:46 - 17:51
prompte, avec des fichiers MD, avec votre réflexion personnelle, ensuite voir comment

### 17:51 - 17:55
lier à vos réponses, voir comment vous pouvez améliorer. Je pense que c'est vraiment comme ça que

### 17:55 - 18:05
vous pouvez apprendre encore plus et avoir une meilleure expertise. En tout cas,

### 18:05 - 18:09
je n'ai pas d'inquiétude que derrière, si vous avez un projet client, vous allez pouvoir le

### 18:09 - 18:14
résoudre. De toute manière, si vous avez la main de question, vous pouvez directement venir

### 18:14 - 19:08
sur WhatsApp, sur Slack, et sirene trouvée, il n'en a pas trouvé beaucoup. Donc là,

### 19:08 - 19:15
vous avez vu qu'au final il n'a pas trouvé beaucoup de sirènes donc on va lui demander

### 19:15 - 19:20
d'aller faire un jeu de données différent là il a déjà la stratégie grâce aux prompts

### 19:20 - 19:25
qu'on lui a envoyé maintenant on va chercher à réussir encore une fois tout ce fichier là

### 19:25 - 19:33
donc comme je compte vous dire le but si vous voulez un résultat qui sera concret on met

### 19:33 - 19:38
système de prompt, on voit le résultat, on modifie, on améliore. Je vais expliquer

### 19:38 - 19:50
clairement, ok, donc là je vais expliquer clairement que ça ne va pas le

### 19:50 - 19:56
tour et que je cherche à améliorer du coup les leads avec leurs sirènes et

### 19:56 - 19:59
sirènes. Donc là ce qu'il a fait c'est qu'il a réfléchi, il m'a proposé un

### 19:59 - 20:04
plan, télécharger le data set sirène établissement, j'ai localisé donc

### 20:04 - 20:09
il y a 150 000 restaurants à peu près, ensuite chercher du coup le matching

### 20:09 - 20:13
entre les données qu'on a et du fichier zip qu'il a téléchargé.

### 20:13 - 20:46
Ça prend un petit peu de temps. Vous voyez là qu'il est en train d'utiliser du

### 20:46 - 20:54
béton 3 donc en C. C'est le code 98.NOTPHONE. Je relance sur un airbar, rayon serré

### 20:54 - 21:01
plus match, plus tolérant, coquet. Donc là il propose une nouvelle stratégie.

### 21:01 - 22:06
Donc il n'a pas passé par le zip. On va voir après l'enrichissement qu'il a fait.

### 22:06 - 22:10
L'image lancée sur 11000.NOTPHONE 13 est à 10 minutes. Là il tente une

### 22:10 - 22:13
nouvelle stratégie, on va voir après le résultat d'ici dix minutes.

### 22:13 - 22:21
L'objectif c'est vraiment, là vous voyez que je l'ai fait par rapport à la

### 22:21 - 22:25
restauration, vous êtes sûrement dans un autre secteur donc la stratégie sera

### 22:25 - 22:27
différente. Donc vous allez emprunter un nouveau parcours, c'est pour ça que

### 22:27 - 22:31
derrière, ce qui est important c'est d'enrichir votre système de promptes

### 22:31 - 22:35
après la VA que vous avez envoyé. Ok on peut améliorer, on le sait.

### 22:35 - 22:38
Donc comment on peut faire pour améliorer ? Je vais vous demander à code code

### 22:38 - 22:41
d'aller chercher des informations supplémentaires sur le jeu de données.

### 22:41 - 22:57
Donc le jeu de données, on l'a vu ensemble, c'était là, on va attendre un petit peu, on va le laisser travailler.

### 22:57 - 23:05
Donc on va lui demander si la stratégie est bonne ou pas.

### 23:05 - 23:16
Donc on va lui donner une question supplémentaire pour voir si ce qui m'a en place est vraiment utile.

### 23:16 - 23:24
Non pas suffisant, le rematch API n'a remonté que 310%, le vrai gars demande de télécharger un nain et faire un matching au local sur CP Ville.

### 23:24 - 23:41
il va se faire un rematch API, il va repartir sur la stratégie numéro une, donc télécharger

### 23:41 - 23:46
vraiment tous les restaurants en France et ensuite faire le matching qu'on parait, qui

### 23:46 - 24:11
corresponde du coup à ma base de données, qu'on va attendre un petit peu, je donne

### 24:11 - 24:17
Et l'information supplémentaire, ça ne sert à rien de perdre son temps, on lance le bulk sérén.

### 24:17 - 24:48
Donc là il est en train de télécharger la partie, je pense qu'il a téléchargé un local.

### 24:48 - 24:56
Donc là ça va prendre un petit peu de temps parce que c'est quand même un sacré co-fichier.

### 24:56 - 25:02
Donc là il va préparer le script pour matcher.

### 25:02 - 25:07
Et ensuite il va nous refaire le rebuild xlsx.

### 25:07 - 25:25
On va demander... Ok bon bah il m'a donné les informations. Ah oui, on prend pas mal de temps

### 25:25 - 26:05
d'en parler. Il me fait le matching local. Bon je vais demander... J'ai oublié le N.

### 26:05 - 26:12
Ça j'ai le en attente. Casse travailler. On va attendre qu'ils signent les deux tâches.

### 26:12 - 26:17
Donc là on a 4 monitors qui sont en running. Strong rematch, Sunday,

### 26:17 - 26:36
Donc là il y a 4 tâches qui sont en train d'être faites en parallèle, 14 minutes rythme

### 26:36 - 26:48
actuel.

### 26:48 - 26:56
Le but vraiment c'est que vous partez d'une stratégie au départ, ou la tester, si elle

### 26:56 - 27:02
marche, elle convient dès le one shot, tant mieux, sinon on cherche une amélioration.

### 27:02 - 27:05
là c'est ce que j'ai fait sur cette partie là

### 27:06 - 27:10
ma V1 était pas mal, j'avais quand même enrichi 10%

### 27:10 - 27:14
et je pense qu'on peut faire mieux, comme on peut faire mieux on demande

### 27:15 - 27:20
ok il m'a fait une amélioration, il m'a récupéré 50% de lits en plus avec un syringe trouvé

### 27:20 - 27:22
bon, est-ce que je m'arrête là ou je cherche ?

### 27:23 - 27:26
moi perso j'ai envie d'aller chercher du 50-60%

### 27:26 - 27:28
une fois c'est 50-60%

### 27:28 - 27:33
on peut dire ok est-ce que je veux améliorer ou pas moi personnellement ce n'est pas le cas

### 27:33 - 27:44
c'est pareil pour tout là on parle de leads on peut le faire aussi sur du rag on va voir qu'on

### 27:44 - 27:55
va faire la partie chatbot ok l'information de ce qu'a donné le one shot est bien mais il

### 27:55 - 27:59
manque des informations pour qu'on soit plus précis plus technique donc comment on peut faire

### 27:59 - 28:02
on peut rajouter justement des informations dans ce règle là.

### 28:02 - 28:08
Ensuite on fait des tests, est-ce que la réponse me convient maintenant, oui non, non elle me

### 28:08 - 28:12
convient pas, ok ben on enrichit encore, on comprend la problématique et on modifie. Et là vous

### 28:12 - 28:16
avez de la chance, vous avez une IA qui peut vous permettre ça. En plus de ça vous pouvez

### 28:16 - 28:22
faire du MCP pour que directement sur internet de faire des tests, qu'on verra aussi cette

### 28:22 - 28:54
C'est parti là, j'y arrive tout le monde.

### 28:54 - 28:56
Ces mecs qui commentent là, tu...

### 28:56 - 28:58
...notre message pour envoyer direct.

### 28:58 - 29:00
Je sais pas si vous pensez que j'ai une colonie,

### 29:00 - 29:02
mais t'en fais quoi, tu leur propose tes services après derrière,

### 29:02 - 29:04
ou je t'en sers juste en mode

### 29:04 - 31:08
touche visibilité.

### 31:08 - 31:13
Donc là on voit par où on en est.

### 31:13 - 31:18
Donc là, hop, punaise.

### 31:18 - 31:20
On a 620 000 établissements actifs.

### 31:20 - 31:22
Auréka filtré

### 31:22 - 31:24
en 3-7 secondes, lancement du matching local.

### 31:24 - 31:26
On glace

### 31:26 - 31:28
au zip qu'on a récupéré.

### 31:28 - 31:36
C'est pas mal du tout, on va faire ensuite, sur ces 620 000 là, un matching.

### 31:36 - 31:41
Donc là, on voit que les 4 moniteurs sont en train de travailler, donc vous pouvez voir ça comme des agents IA

### 31:41 - 31:46
qui font chacun leur tâche, qui sont prédéfinis directement par code code.

### 31:46 - 32:02
Donc là, je vais demander une demande, on va voir si...

### 32:02 - 32:40
Ok, donc là, il a fait une erreur en date, il aurait trouvé lui-même de quelque manière par la suite.

### 32:40 - 32:47
Ok, donc là il a refait un nouveau format, une université du pays qu'il a créé juste

### 32:47 - 32:59
avant pour faire le coût matching entre le Trek Seren 01 et l'autre LSX.

### 32:59 - 33:11
Ok, donc là on a bien ces superbes, superbes, donc ça fonctionne bien le matching.

### 33:11 - 33:29
là on a trouvé la stratégie superbe vous avez vu tout est parti d'une seule stratégie

### 33:29 - 33:39
ensuite on a cherché des améliorations et là du coup on a un retour pour le moment

### 33:39 - 33:44
qui atteint les 83% alors qu'avant on était à je sais plus combien on était à 10% je crois

### 33:44 - 33:51
c'est un peu moi même c'est pour ça qu'il faut à chaque fois itérer itérer itérer itérer

### 33:51 - 33:58
c'est comme ça qu'il faut travailler avec les clients auquel je sais pas faire c'est

### 33:58 - 34:06
pas comment je peux résoudre cette problématique là point 1 première stratégie j'envoie un prompt

### 34:06 - 34:11
à Claude code Claude code me donne une solution ça marche mais il y a mieux à faire donc qu'est ce

### 34:11 - 34:19
que je peux améliorer réflexion prompt itération amélioration que je peux encore améliorer du coup

### 34:19 - 34:25
on l'envoie directement à Cloud Code et c'est comme ça qu'il faut faire sur

### 34:25 - 35:31
dépôt les clients. Il nous mentionne que c'est bientôt terminé, voilà c'est

### 35:31 - 35:37
beau. On a récupéré 3 487, nouveau height,

### 35:37 - 35:47
reconstruction du xlsx. Là il est en train de faire les updates au niveau de ce

### 35:47 - 36:05
documents là. On va attendre un petit peu. Il a trouvé 10 000 sirènes sur l'élite qu'on

### 36:05 - 36:17
avait récupérée sur Greenmaps, à vérifier 6200. Donc là c'est top. Donc on a un peu près,

### 36:17 - 36:23
devant on a plus, près poids, c'est pas mal. On va ouvrir le fichier du coup, on va le relire,

### 36:23 - 36:29
on va voir les informations qu'on a pu récupérer du coup. Donc là encore une fois,

### 36:29 - 36:45
c'est un gros fichier. On va l'importer sur Google Sheets, ça va être plus simple et plus lisible et

### 36:45 - 36:54
on va analyser tout ça pour du coup la suite. Donc pour la prochaine séance, business, prospect,

### 36:54 - 37:17
xlsx, j'espère que ça a un moment bleu vu que c'est dans le cloud direct et pas en local,

### 37:17 - 37:22
pour ça que mon ordinateur chauffe un petit peu. Je vais fermer cette page là,

### 37:22 - 37:48
attendre un petit peu. Voilà, superbe. Là on a le récap, les petits schémas, tout ça.

### 37:48 - 37:55
On a la partie All Leads. C'est là, il est enrichi. Du coup, Domaine,

### 37:55 - 38:03
adresse, poste en région, speed, personnalisation, angle, bonjour, bonjour,

### 38:03 - 38:14
source URL, place, en plus je vois pas où il a mis les informations, donc on va aller voir avec

### 38:14 - 39:08
email, source de performance, je vais lui demander directement, soit j'ai tous les onglets qui

### 39:08 - 39:20
entiennent les leads, j'ai un petit collène, des jus, ok, donc all leads il m'a dit, qu'est-ce

### 39:20 - 39:29
qu'il voit des deux petites tools alors je vois pas ok attends je vais copier les

### 39:29 - 39:47
premières lignes alors peut-être que j'ai pas pris le bon fichier alors attends

### 39:47 - 39:57
je suis con j'ai pas pris le bon fichier c'est dans celui là que je travaille

### 39:57 - 40:09
en fait je vais essayer de l'ouvrir c'est pour ça qu'il faut bien s'organiser

### 40:09 - 40:16
encore une fois là j'ai fait une connerie donc ouais bah oui c'est sûr

### 40:16 - 40:19
parce que là il y a du python tout ça c'est sûr que c'est lui je vais quand

### 40:19 - 40:30
me voir. C'est sûr. Donc je vais aller voir du coup. Ça va, il faut vraiment bien

### 40:30 - 40:42
serrer. Bah oui, c'est sûr. C'est pas ouvert à mon moment.

### 40:42 - 41:03
On met ça là. Tac. Même lui là. Du coup il doit être dans ça maintenant. Bah oui.

### 41:03 - 41:14
Bah oui, c'est sûr. C'est moi qui ai fait une caméra. C'est pour ça qu'il faut

### 41:14 - 41:17
vraiment, vraiment bien s'organiser. C'est trop important.

### 41:17 - 41:44
non du coup il n'est pas là. Je vais ré-ouvrir un peu, j'essaie juste de trouver le fichier

### 41:44 - 42:31
voilà il n'y a plus de données, il y en avait 5, on va importer les données. Si on ne trouve pas,

### 42:31 - 42:43
ça va être très simple. On va mettre bien toutes les informations ajoutées.

### 42:43 - 42:54
Donc là on a la partie DG Match Statue, là on a la partie annuaire où on peut retrouver

### 42:54 - 42:59
du coup directement le lien avec toutes les informations que je souhaite, donc là c'est

### 42:59 - 43:04
vraiment une phase d'enrichissement qui est au top, on peut voir son immatriculation

### 43:04 - 43:16
RNE, on peut voir son siren, la TVA, son capital social, son adresse, ce n'est pas mon top.

### 43:16 - 43:20
Du coup, est-ce qu'on a récupéré son nom et prénom à lui ?

### 43:20 - 43:25
Ouais, Mathieu, on a récupéré Mathieu, voilà, donc là c'est parfait pour de la

### 43:25 - 43:26
prospection automatique.

### 43:26 - 43:30
On peut récupérer son nom, après on n'a pas le nom de tout le monde encore

### 43:30 - 43:31
une fois parce que c'est complexe.

### 43:31 - 43:41
On va essayer, on a quelques petits noms, mais surtout nos leads, on a au moins la

### 43:41 - 43:43
partie Read Confidence, etc.

### 43:43 - 43:49
Donc là on a réussi à enrichir une partie de nos leads, donc on sait maintenant qui

### 43:49 - 43:53
contacter en Light Confidence et en Medium Confidence, donc c'est cela qui nous

### 43:53 - 43:54
intéresse.

### 43:54 - 44:02
Donc maintenant on va falloir tout simplement encore enrichir pour récupérer toutes

### 44:02 - 44:03
les adresses e-mail.

### 44:03 - 44:13
Comme on a pu le voir, on n'a pas pu enrichir un maximum avec notre premier Scrapting.

### 44:13 - 44:15
On n'a pas pu récupérer toutes les adresses e-mails.

### 44:15 - 44:21
Là, on va utiliser une API payante avec Apify, qui est une bibliothèque d'ascrapteurs

### 44:21 - 44:27
où on peut vraiment tout faire et puis voir comment justement gérer tout ça.

### 44:27 - 44:30
On va avoir une base de données exploitable pour de la prospection.

### 44:30 - 44:38
Et après on ira faire aussi, juste avant la prospection, un rangement complet de

### 44:38 - 44:41
toute notre base pour qu'elle soit bien structurée, qu'on sache où on en est.

### 44:41 - 44:46
C'est parti, on se retrouve tout de suite et on attaque la séance du coup

### 44:46 - 44:51
17. A tout de suite !
