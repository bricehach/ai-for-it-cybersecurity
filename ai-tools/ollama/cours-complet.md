# Comprendre et utiliser Ollama

> Cours complet — modèles locaux, cloud, confidentialité et assistants IA

Modèles locaux, cloud, confidentialité et assistants IA

Parcours progressif pour Windows 11 Pro, Linux Debian, Obsidian, Notion, GitHub et ChatGPT

> Objectif  Comprendre avant de modifier ta méthode de travail : savoir ce qu’Ollama fait, ce qu’il ne fait pas, comment le tester sans migrer tes données, puis décider si un assistant local mérite une place dans ton environnement.

Le premier terrain d’essai est le Lenovo ThinkBook 15 G2 ITL (modèle 20VE) sous Windows 11 Pro : Intel Core i5-1135G7 à 4 cœurs, 24 Go de RAM et Intel Iris Xe Graphics intégrée. La capture du Gestionnaire des tâches montre une mémoire graphique partagée, et non de la VRAM dédiée. Les performances dépendront du modèle choisi, de la mémoire disponible au moment du test et de l’accélération réellement utilisée. Les usages Kali et Parrot viennent ensuite, comme étapes de comparaison et de laboratoire.

Les noms de menus et commandes sont donnés en anglais lorsqu’ils sont susceptibles d’apparaître ainsi dans l’interface. Les exigences d’Ollama et les modèles disponibles changent : les procédures renvoient aux documentations officielles.

| Niveau | Tu vas apprendre à… |
| --- | --- |
| Découverte | Expliquer modèles, runtime, interface, local et cloud. |
| Pratique | Installer Ollama sous Windows, lancer un modèle et évaluer les réponses. |
| Intermédiaire | Relier un modèle aux notes, tester une API et comparer des flux de travail. |
| Avancé et enseignement | Sécuriser un assistant, concevoir un pilote et enseigner les notions avec méthode. |

# Comment suivre le cours

Lis d’abord les parties 1 à 5 sans rien installer. Ensuite, fais le test sur le PC Windows seulement, avec des données fictives. Les étapes d’intégration sont volontairement séparées : une intégration peut lire ou modifier tes données; il faut comprendre ses droits avant de la connecter.

Le projet fil rouge est « Atelier IA et cyberdéfense », un espace fictif de notes et de formation. Il sert à illustrer Obsidian, Notion, GitHub et une recherche documentaire locale. N’utilise pas de dossiers personnels, dossiers de travail réel ou données de cours sensibles pendant les premiers essais.

## Plan de progression conseillé

| Étape | Durée indicative | Résultat attendu |
| --- | --- | --- |
| 1. Comprendre les composants | 1 à 2 h | Dessiner le fonctionnement et expliquer les limites. |
| 2. Tester Ollama sur Windows | 1 à 2 h | Lancer un petit modèle et noter la vitesse/qualité. |
| 3. Évaluer confidentialité et matériel | 1 h | Décider quels usages sont acceptables sur le Lenovo. |
| 4. Tester une interface et un dossier fictif | 2 à 4 h | Faire une recherche locale contrôlée. |
| 5. Relier Obsidian, Notion, GitHub et ChatGPT | plusieurs séances | Comprendre les rôles avant toute refonte. |
| 6. Préparer une mini-leçon | 1 à 2 h | Enseigner les bases et une démonstration sûre. |

> Règle de décision  Tu ne changes pas toute ta méthode de travail parce qu’un outil semble prometteur. Tu compares un petit pilote à ton fonctionnement actuel, puis tu décides à partir de tâches concrètes.

# 1. Qu’est-ce qu’Ollama ?

Ollama est un logiciel qui facilite le téléchargement, le lancement et l’accès à des modèles d’intelligence artificielle. Il s’agit d’un runtime et d’un serveur de modèles, pas d’un seul modèle et pas, à lui seul, d’un assistant personnel complet. Il sait lancer des modèles locaux et peut aussi donner accès à des modèles cloud selon l’offre et la configuration.

Un modèle est un ensemble de paramètres appris pendant l’entraînement. Lorsqu’on lui envoie un texte, il calcule une suite probable de tokens (morceaux de mots) selon son apprentissage et les instructions fournies. Il ne « comprend » pas comme un humain et peut produire une réponse fausse avec assurance.

| Élément | Fonction | Exemple concret |
| --- | --- | --- |
| Modèle (model) | Produit les réponses à partir des paramètres appris. | Gemma, Qwen, Llama ou un autre modèle disponible. |
| Ollama | Télécharge, charge et sert le modèle. | Commande `ollama run …` ou serveur API local. |
| Interface | Permet de converser ou de travailler. | Application Ollama, terminal, éditeur ou interface tierce. |
| Données/context | Informations jointes à la demande. | Question, document sélectionné, notes retrouvées. |
| Outils/connecteurs | Fonctions appelées par l’application si autorisées. | Recherche, API, gestion de fichiers, script. |

> Distinction centrale  Installer Ollama ne veut pas dire installer ChatGPT. Ollama fournit un moyen d’exécuter certains modèles; chaque modèle garde ses propres capacités, limites et conditions de licence.

# 2. Les notions indispensables

## LLM, paramètres et taille

Un grand modèle de langage (Large Language Model, LLM) est entraîné à traiter et générer du langage. Le nombre de paramètres donne une indication de taille, mais ne suffit pas à juger qualité, vitesse ou adéquation. Un modèle plus petit peut convenir à une tâche simple et répondre plus vite; un modèle plus grand demande davantage de mémoire et peut rester lent sur un PC donné.

## Quantification

La quantification réduit la précision numérique des paramètres pour diminuer la taille mémoire et accélérer l’exécution. Une quantification plus compacte peut réduire la qualité sur certaines tâches. Les noms comme Q4 ou Q8 désignent des familles de représentation, mais le détail dépend du fichier et du modèle. Compare sur tes tâches réelles, pas sur le nom seul.

## Contexte et tokens

La fenêtre de contexte est la quantité d’information prise en compte dans une requête et l’historique transmis. Un contexte plus grand consomme plus de mémoire. Une session de chat ne transforme pas automatiquement tous tes documents en mémoire permanente : il faut une base documentaire, une recherche ou un outil explicitement connecté.

## Inférence locale

L’inférence est le calcul réalisé au moment où le modèle répond. Dans le scénario local, les poids du modèle sont chargés sur ton PC et le calcul s’exécute sur CPU et/ou GPU. Un modèle local n’a pas automatiquement accès au web, à tes fichiers, à ton calendrier ou à tes comptes.

# 3. Local et cloud : deux modes différents

| Aspect | Modèle local | Modèle cloud |
| --- | --- | --- |
| Où le calcul se fait | Sur le PC ou serveur que tu contrôles. | Sur l’infrastructure du fournisseur. |
| Téléchargement | Poids du modèle à télécharger et stocker. | Pas toujours de téléchargement de poids. |
| Internet pendant l’inférence | Pas nécessaire pour un modèle local déjà présent, si aucune fonction externe n’est appelée. | Nécessaire pour envoyer la demande et recevoir la réponse. |
| Données | Peuvent rester dans le périmètre local si l’application et ses extensions le respectent. | Prompts/réponses sont traités par le service distant selon sa politique. |
| Performance | Dépend directement de ton matériel et du modèle choisi. | Dépend du service, de l’offre et de la connexion. |
| Mise à jour des connaissances | Le modèle n’est pas automatiquement à jour; il faut apporter de nouvelles sources. | Peut être relié à des outils web, selon le produit. |
| Coût | Électricité, stockage et temps machine; logiciels parfois gratuits. | Abonnement, crédits ou coût à l’usage selon le service. |

Ollama distingue les modèles locaux des modèles cloud. La documentation indique que les requêtes cloud sont traitées par le service distant, tandis qu’il est possible de désactiver les fonctions cloud pour n’utiliser que les modèles locaux. Vérifie toutefois le chemin de la requête et les fonctions activées à chaque intégration.

> Confidentialité sans illusion  « Local » réduit l’exposition à un fournisseur distant pour l’inférence, mais ne garantit pas une confidentialité absolue. Il reste les sauvegardes, journaux, extensions, permissions, comptes Windows, logiciels malveillants, télémétrie éventuelle et erreurs de configuration réseau.

# 4. Confidentialité : faire le bon découpage

## Les questions à poser

- Où sont stockés les documents, l’historique et les journaux ?
- Quelles données sont envoyées à un service distant, et dans quel but ?
- L’extension ou l’agent peut-il lire tout le disque ou seulement un dossier de test ?
- Le serveur local écoute-t-il seulement sur l’ordinateur, ou sur le réseau ?
- Les données envoyées contiennent-elles des informations personnelles, professionnelles ou des secrets ?
## API locale : protection nécessaire

L’API locale d’Ollama est accessible par défaut sur `http://localhost:11434` et ne demande pas d’authentification. C’est pratique pour les applications locales, mais cela signifie qu’il faut éviter de l’exposer sans contrôle sur le réseau. Ne modifie pas l’adresse d’écoute vers toutes les interfaces réseau simplement pour rendre un essai « plus pratique ».

| Situation | Risque à comprendre | Mesure de départ |
| --- | --- | --- |
| Un modèle lit des notes personnelles | Une extension peut transmettre ou enregistrer des extraits. | Copie fictive, permissions minimales, test sans données privées. |
| Serveur Ollama exposé sur LAN/Internet | Un tiers peut interroger le service ou consommer le matériel. | Rester sur localhost; pare-feu; aucune exposition publique. |
| Modèle cloud activé | Le prompt est envoyé au fournisseur cloud. | Étiqueter le mode dans les exercices; retirer les données sensibles. |
| Connecteur API Notion/GitHub | Un token peut autoriser lecture/écriture. | Accès limité, secret hors notes et dépôt, révocation après test. |
| Agent qui lance des outils | Prompt malveillant ou erreur peut provoquer une action. | Dossier sandbox, permissions de lecture seule, validation humaine. |

> Règle pour les premiers essais  N’envoie pas de mots de passe, tokens, données de clients, dossiers médicaux, preuves d’incident, journaux réels ou documents professionnels confidentiels. Remplace-les par des exemples fictifs.

# 5. Évaluer ton Lenovo avant de choisir un modèle

La configuration relevée est un Lenovo ThinkBook 15 G2 ITL (20VE), processeur Intel Core i5-1135G7 à 4 cœurs, 24 Go de RAM et graphique intégré Intel Iris Xe. Au moment de la capture, environ 15,8 Go de RAM étaient utilisés sur 23,8 Go visibles par Windows. Cette disponibilité varie selon les applications ouvertes. Commence par de petits modèles et observe la mémoire, la vitesse et la température avant d’augmenter leur taille. La mémoire graphique affichée comme partagée n’est pas une réserve dédiée de VRAM.

## Informations à collecter sous Windows 11 Pro

```text
Win + R → msinfo32
Paramètres → Système → Informations système
Gestionnaire des tâches → Performances → Processeur / Mémoire / GPU
PowerShell : Get-CimInstance Win32_ComputerSystem | Select-Object Manufacturer,Model,TotalPhysicalMemory
Get-CimInstance Win32_Processor | Select-Object Name
Get-CimInstance Win32_VideoController | Select-Object Name,AdapterRAM,DriverVersion
```
`AdapterRAM` peut être inexact pour certaines cartes; confirme la VRAM avec le Gestionnaire des tâches, le panneau du pilote ou la fiche Lenovo correspondant au numéro de machine. Le GPU intégré partage souvent la mémoire système; la quantité totale de RAM ne correspond donc pas à la mémoire disponible pour le modèle.

## Ce qui influence l’expérience

- RAM disponible : important pour charger les poids et garder Windows réactif.
- VRAM : peut accélérer l’inférence si Ollama prend en charge la carte et le pilote.
- Processeur et refroidissement : déterminent une partie de la vitesse CPU et la durée de charge soutenue.
- Stockage libre : les modèles peuvent occuper plusieurs gigaoctets chacun; des modèles cloud n’occupent pas la même place.
- Taille du contexte : l’augmenter augmente la mémoire utilisée.
> Décision prudente  Commence par un modèle compact, observe les performances, puis augmente progressivement. La meilleure taille dépend de la configuration réelle et du cas d’usage, pas seulement de la génération du Lenovo.

# 6. Installation et première vérification sur Windows

La voie la plus simple pour un premier essai est l’installateur officiel Windows. Il installe Ollama dans l’environnement utilisateur et inclut l’application et le serveur. Les poids des modèles s’ajoutent à l’espace disque requis; leur emplacement peut être configuré avec la variable `OLLAMA_MODELS`.

1. Télécharger l’installateur depuis le site officiel Ollama; éviter les miroirs inconnus.

2. Installer sans modifier les réglages avancés; noter l’espace disque disponible.

3. Ouvrir l’application Ollama et vérifier que le serveur local démarre.

4. Choisir un petit modèle adapté aux ressources relevées, depuis les modèles disponibles dans Ollama.

5. Faire un premier essai avec une question fictive simple.

6. Comparer le temps de réponse, la consommation mémoire et la qualité sur une tâche précise.

```text
ollama --version
ollama list
ollama ps
ollama run NOM_DU_MODELE
```
Quitter une session interactive peut se faire avec la commande d’interface prévue par la version installée; consulte `ollama help` et l’aide officielle si le raccourci diffère. Pour retirer un modèle téléchargé, vérifie d’abord son nom avec `ollama list`, puis utilise la commande documentée `ollama rm NOM_DU_MODELE`.

> Ne pas confondre  Le téléchargement d’un modèle peut prendre du temps et de l’espace. Vérifie le nom, la taille et la source du modèle avant de le récupérer.

# 7. Le vocabulaire de la ligne de commande

| Commande | Rôle pédagogique |
| --- | --- |
| `ollama run NOM` | Lance le modèle en conversation; peut entraîner son téléchargement initial. |
| `ollama pull NOM` | Télécharge le modèle sans démarrer la conversation. |
| `ollama list` | Affiche les modèles présents localement. |
| `ollama ps` | Montre les modèles actuellement chargés et des détails d’exécution. |
| `ollama show NOM` | Affiche des informations sur un modèle. |
| `ollama rm NOM` | Supprime un modèle local choisi. |
| `ollama serve` | Démarre le serveur Ollama en ligne de commande si nécessaire. |
| `ollama help` | Affiche l’aide et les commandes de la version installée. |

Les commandes exactes et options peuvent évoluer. Le but est de comprendre les familles d’actions : télécharger, lancer, inspecter, supprimer, créer et exposer une API. Fais les suppressions dans un environnement de test et vérifie le nom avant de confirmer.

# 8. Choisir et évaluer un modèle

Les bibliothèques de modèles évoluent vite. Le cours ne fige pas une recommandation de modèle qui pourrait être dépassée. Choisis selon la tâche, la taille téléchargée, la licence, les langues, le contexte, le besoin en vision ou en outils et les performances sur ton PC.

| Critère | Question de contrôle |
| --- | --- |
| Tâche | Ai-je besoin de rédaction, résumé, raisonnement, code, vision ou extraction ? |
| Langue | Répond-il correctement en français et conserve-t-il les accents ? |
| Taille | La taille reste-t-elle compatible avec le stockage et la mémoire ? |
| Licence | Les conditions autorisent-elles mon usage personnel, éducatif ou professionnel ? |
| Origine | Le modèle vient-il d’une source crédible, avec une fiche compréhensible ? |
| Qualité | A-t-il bien répondu à des questions dont je connais les réponses ? |
| Contexte | Le modèle et l’application supportent-ils la longueur nécessaire ? |

## Mini protocole de test

1. Écris cinq questions représentatives de ton travail, en données fictives.

2. Note la réponse idéale ou les critères de qualité avant de tester.

3. Pose les mêmes questions à plusieurs modèles avec les mêmes consignes.

4. Note exactitude, utilité, clarté, temps et erreurs.

5. Garde un modèle seulement s’il apporte une valeur mesurable.

# 9. Bien parler à un modèle local

Le prompt (instruction) doit préciser tâche, contexte utile, format attendu et limites. Les modèles locaux peuvent répondre différemment à une même consigne; cette variabilité fait partie de l’évaluation, pas d’une garantie que le prompt est bon.

```text
Rôle : tu es un assistant pédagogique en informatique.
Tâche : explique le concept ci-dessous à un débutant puis donne l’explication technique.
Contraintes : sépare faits, hypothèses et points à vérifier; n’invente pas de sources.
Format : définition, exemple, limites, question de contrôle.
Sujet : [contenu fictif]
```
## Routine de vérification

- Demander au modèle ce qu’il sait et ce qui lui manque.
- Faire vérifier les commandes dans la documentation officielle avant exécution.
- Demander une réponse avec sections et critères observables plutôt qu’un texte vague.
- Conserver la source et la date pour les connaissances évolutives.
- Ne pas traiter la fluidité du texte comme une preuve de justesse.
# 10. Application, interface et API

Ollama fournit une application et un service local, mais une API permet aussi à d’autres logiciels de l’utiliser. Le point d’accès local documenté est `http://localhost:11434/api`; Ollama propose également une compatibilité avec une partie des formats API OpenAI, qui aide certaines applications existantes à se connecter. Compatibilité partielle ne signifie pas fonctionnalités identiques.

```text
POST http://localhost:11434/api/chat
{
  "model": "NOM_DU_MODELE",
  "messages": [
    {"role": "user", "content": "Explique le principe d’une sauvegarde incrémentale."}
  ],
  "stream": false
}
```
Une interface tierce peut ajouter gestion des conversations, utilisateurs, documents ou fonctions de recherche. Elle constitue un composant distinct : son stockage, ses mises à jour, ses permissions et sa politique de confidentialité doivent être examinés séparément. Pour apprendre, commence avec l’application officielle et ne rajoute une interface qu’après avoir compris le besoin.

# 11. Modèles personnalisés avec Modelfile

Un Modelfile décrit la base d’un modèle personnalisé et certains paramètres/instructions. Il ne réentraîne pas nécessairement le modèle : il crée une configuration réutilisable pour le lancement. On peut définir un modèle de base, une consigne de système et des paramètres adaptés.

```text
FROM NOM_DU_MODELE
SYSTEM "Tu aides à structurer des notes de formation en français. Distingue les faits des hypothèses."
PARAMETER temperature 0.2
ollama create assistant-cours -f Modelfile
ollama run assistant-cours
```
Teste les instructions sur des cas variés et garde un fichier de configuration versionnable. Évite d’y mettre des données personnelles, tokens, instructions secrètes ou paramètres que tu ne comprends pas. Les noms de champs et paramètres doivent être vérifiés dans le Modelfile Reference officiel.

# 12. Documents, embeddings et RAG

Un modèle ne connaît pas automatiquement les notes stockées sur ton PC. Pour poser des questions sur un dossier de documents, une application peut extraire les textes, les découper en passages, calculer des embeddings et retrouver les passages pertinents au moment d’une question. Cette méthode est appelée RAG (Retrieval-Augmented Generation, génération augmentée par récupération).

```text
Notes source → extraction et découpage → index de recherche → passages pertinents
Question → récupération des passages → prompt augmenté → modèle → réponse avec références à vérifier
```
Un embedding représente un texte par des nombres afin d’en rechercher la proximité de sens. Ollama peut générer des embeddings, mais une application doit les stocker et les utiliser. Un index documentaire devient lui-même un dépôt à protéger : il peut contenir des extraits ou représentations de notes privées.

## Limites d’un chat avec les documents

- La réponse dépend des passages effectivement retrouvés; un passage manquant peut produire une réponse lacunaire.
- Les documents peuvent contenir des erreurs, données périmées ou instructions malveillantes.
- Une citation générée ne prouve pas que le modèle a bien compris la source.
- L’index doit être mis à jour lorsque les notes changent.
- Le filtrage des documents et droits d’accès doit être testé, surtout si plusieurs utilisateurs partagent l’interface.
# 13. Étape d’intégration Obsidian

Obsidian convient naturellement aux tests de recherche locale parce que ses notes sont des fichiers Markdown dans un coffre (vault). Ollama ne lit pas ce coffre sans une extension ou un programme séparé. L’intégration peut utiliser l’API locale, un plugin communautaire ou une application de RAG qui indexe les fichiers.

1. Créer un coffre de test séparé, avec trois notes fictives seulement.

2. Choisir une extension dont le code, l’auteur, les permissions et les mises à jour sont compréhensibles.

3. Vérifier si l’extension envoie les textes vers un cloud ou garde le traitement local.

4. Limiter l’accès à un dossier de test; ne pas pointer vers Documents ou tout le profil utilisateur.

5. Poser des questions dont les réponses se trouvent explicitement dans les notes et vérifier les références.

Une première version peut rester manuelle : copier une note fictive dans la conversation locale d’Ollama et demander un résumé. La recherche automatisée vient après, quand tu maîtrises la différence entre note, index, modèle et extension.

# 14. Étape d’intégration Notion

Notion est principalement un espace cloud de pages et bases. Il ne faut pas supposer qu’un modèle local installé dans Ollama peut déjà lire ton espace Notion. Il faut un export/import manuel, une connexion API, un connecteur ou un autre service configuré, et chacun a des conséquences différentes sur les données.

| Étape | Méthode | Point de contrôle |
| --- | --- | --- |
| 1. Manuel | Copier un exemple fictif entre Notion et Ollama. | Retirer données sensibles; vérifier le résultat avant publication. |
| 2. Export contrôlé | Exporter quelques pages en Markdown/HTML et travailler sur copie locale. | Examiner pièces jointes et limites d’export. |
| 3. API ou connecteur | Autoriser une intégration à certaines pages. | Droits minimaux, token protégé, accès révocable. |
| 4. Automatisation | Synchroniser certaines entrées selon une règle. | Traçabilité, erreurs, doublons, approbation humaine. |

Le connecteur MCP documenté par Notion vise des assistants compatibles; cela ne signifie pas qu’Ollama se connecte nativement à ton espace. La connexion d’un assistant doit être vérifiée dans son interface et dans les réglages du workspace. Un jeton API donne accès selon les permissions accordées : ne le colle jamais dans un prompt ou un dépôt Git.

# 15. Étape d’intégration GitHub

GitHub peut garder le code d’un projet Ollama, les prompts, les Modelfiles, les procédures et les résultats de tests expurgés. Un dépôt ne doit pas devenir un endroit où publier les poids lourds des modèles par défaut, des conversations personnelles ou des secrets.

| Adapté à un dépôt | À exclure ou protéger |
| --- | --- |
| README et guide d’installation | Clés API, mots de passe, tokens Notion/GitHub. |
| Modelfile générique et prompts fictifs | Notes personnelles ou conversations privées. |
| Scripts d’évaluation et résultats anonymisés | Données de cours protégées ou preuves réelles non autorisées. |
| Configuration sans secret | Modèles de plusieurs Go sans stratégie de stockage claire. |

Tu peux versionner l’évolution de la configuration : expliquer pourquoi tu as changé une consigne ou ajouté une étape. C’est une bonne façon d’apprendre et d’enseigner le fonctionnement sans confondre GitHub avec un stockage privé sécurisé.

# 16. Étape d’intégration ChatGPT

Dans cette conversation, je ne peux pas joindre directement le service Ollama qui tourne sur ton Lenovo : le serveur est local à ton ordinateur. En revanche, je peux t’aider à concevoir les tests, expliquer les commandes, analyser une réponse que tu colles ici et améliorer le processus. Une connexion via API ou un autre connecteur demanderait un composant configuré explicitement et autorisé.

La collaboration initiale la plus claire : ChatGPT t’aide à analyser et structurer; Ollama te sert à apprendre le fonctionnement local et à tester des tâches choisies; tu compares les réponses; tu vérifies les faits; tu sauvegardes les conclusions utiles dans Obsidian/Notion; tu versionnes les configurations publiables sur GitHub.

> Ce qui ne se fait pas tout seul  Aucun de ces outils ne devient automatiquement la mémoire commune de tous les autres. Les accès, transferts, indexations et synchronisations doivent être explicitement construits et testés.

# 17. Automatisations, fonctions et assistants

Certains modèles peuvent produire des appels d’outils (tool calling), mais le modèle ne devrait pas agir seul. L’application décide si un outil existe, l’exécute avec ses permissions et transmet le résultat. Il faut donc évaluer le modèle et le code d’orchestration ensemble.

| Niveau | Capacité | Contrôle recommandé |
| --- | --- | --- |
| Réponse texte | Résumer une note fictive. | Vérification humaine. |
| Recherche documentaire | Retrouver des passages pertinents. | Limiter dossier et afficher les sources. |
| Appel d’outil | Demander une action ou un appel API. | Liste blanche, validation avant écriture. |
| Agent | Planifier plusieurs étapes et appeler des outils. | Sandbox, logs, droits minimaux, arrêt d’urgence. |
| Assistant vocal | Entrée/sortie audio et commandes. | Consentement, contrôle de l’action, protection des enregistrements. |

## Ton « JARVIS » en termes réalistes

Un assistant personnel local peut combiner Ollama, une interface, une recherche dans des notes, reconnaissance vocale, synthèse vocale et outils. Il ne devient utile que quand chaque composant est fiable et que ses droits sont limités. Le premier bon prototype répond à des questions sur un dossier fictif; le contrôle du PC ou l’envoi de messages vient bien plus tard, avec confirmation explicite.

# 18. Utiliser Ollama sous Kali et Parrot

Ollama propose une installation Linux officielle. Pour Kali et Parrot, deux distributions Debian-based, il faut tenir compte des versions de noyau, pilotes GPU et dépendances disponibles. L’installation d’un runtime IA sur une machine de laboratoire peut aussi affecter stockage, mises à jour et stabilité. C’est pourquoi le cours place Windows en première étape et Linux ensuite.

Après validation sur Windows, tu pourras comparer une installation Linux sur la machine Parrot ou une VM isolée. Une VM n’obtient pas automatiquement l’accélération GPU du PC : cela dépend de la virtualisation et du passthrough disponibles. Pour un premier essai, le modèle CPU peut suffire mais être plus lent.

```text
Linux — installation officielle (à consulter au moment du test)
curl -fsSL https://ollama.com/install.sh | sh

Vérification après installation
ollama --version
ollama list
ollama ps

Journaux d’un service systemd
journalctl -u ollama --no-pager
```
> Pratique cybersécurité  Le script d’installation officiel doit être téléchargé depuis le domaine officiel et compris avant son exécution. Pour un environnement d’étude, note la version, le chemin, le service créé et la procédure de désinstallation. Ne l’exécute pas en root par réflexe; suis la procédure officielle adaptée à la version.

# 19. Entretien, stockage et dépannage

| Symptôme | Vérifications de départ |
| --- | --- |
| Réponse très lente | Taille du modèle, CPU/GPU utilisé, mémoire, contexte, température. |
| Mémoire insuffisante | Fermer les applications, réduire modèle/contexte, vérifier VRAM/RAM. |
| Modèle introuvable | `ollama list`, orthographe du nom, serveur démarré, espace disque. |
| Modèle lancé mais non visible | `ollama ps`, serveur local et compte utilisateur. |
| Erreur API | URL locale, JSON, nom modèle, serveur en cours d’exécution. |
| Disque plein | Répertoire des modèles, taille téléchargée, modèles inutilisés, espace libre. |
| GPU non utilisé | Pilote et compatibilité listée par Ollama, logs, système d’exploitation. |

Sous Windows, les modèles peuvent occuper beaucoup d’espace et être déplacés par configuration de la variable `OLLAMA_MODELS`. Sous Linux, le service peut être inspecté avec les journaux systemd. Avant déplacement ou suppression, arrête le service si la documentation le demande et garde une copie si nécessaire.

# 20. Réorganiser ton travail sans tout migrer

Ta question porte sur une possible refonte de méthode. La bonne réponse n’est pas de tout transférer vers un assistant local. Il faut vérifier si Ollama apporte une capacité supplémentaire utile par rapport à ChatGPT et à tes outils actuels, pour un coût d’apprentissage, de matériel et de maintenance acceptable.

## Pilote en quatre semaines

| Semaine | Essai | Décision documentée |
| --- | --- | --- |
| 1 | Ollama seul avec questions fictives. | Est-il assez rapide et utile sur le Lenovo ? |
| 2 | Résumer trois notes fictives issues d’un coffre de test Obsidian. | La qualité est-elle suffisante et la source visible ? |
| 3 | Exporter une page Notion fictive, puis tester API seulement si le besoin existe. | Quelle donnée circule et qui peut l’écrire ? |
| 4 | Versionner README, prompts et script d’évaluation sur dépôt privé. | Qu’est-ce qui mérite d’être conservé ou enseigné ? |

## Indicateurs à noter

- Temps nécessaire pour préparer une réponse utile.
- Part des réponses factuellement correctes selon une vérification définie.
- Nombre de corrections nécessaires.
- Temps de réponse et gêne sur le PC.
- Données utilisées et exposition associée.
- Valeur nouvelle par rapport à ton flux actuel avec ChatGPT.
# 21. Le système de collaboration envisagé

Voici un fonctionnement cible à tester, pas une architecture imposée :

```text
ChatGPT : cadrer le problème, expliquer, structurer et préparer une démarche
Ollama local : expérimenter, comparer et interroger un corpus de test local
Obsidian : conserver les notes durables, décisions, procédures et sources
Notion : suivre les projets, tâches, bases et collaborations cloud
GitHub : versionner les documents et scripts que tu choisis de publier ou partager
```
Le passage de l’information reste contrôlé par toi : tu choisis ce qui est fiable, ce qui doit être archivé, où le placer et si cela peut être partagé. Cette étape de sélection est la partie humaine du « second cerveau » et elle évite d’accumuler automatiquement chaque conversation.

| Action | Responsable recommandé au départ |
| --- | --- |
| Définir le problème et les critères | Toi avec ChatGPT |
| Exécuter un test local | Toi sur Ollama |
| Vérifier les faits et les sources | Toi, avec aide de ChatGPT et sources officielles |
| Décider ce qui devient connaissance durable | Toi |
| Choisir la destination | Obsidian, Notion ou GitHub selon l’usage |
| Automatiser | Seulement après répétition et validation du processus manuel |

# 22. Exercices pratiques

## Exercice A — classer les composants

Associe à chacun de ces éléments son rôle : Ollama, modèle, interface, API, Obsidian. Explique ensuite pourquoi installer Ollama ne suffit pas à donner au modèle accès à un dossier.

## Exercice B — local ou cloud ?

Pour chaque demande, choisis une première approche et justifie-la : (1) résumer une note fictive; (2) analyser un dossier client confidentiel; (3) chercher une information actuelle sur les mises à jour Windows; (4) comparer deux modèles sur une consigne pédagogique.

## Exercice C — audit confidentialité

Tu installes une extension de chat avec accès à ton coffre Obsidian et à l’API locale. Liste cinq questions à vérifier avant d’y ajouter tes vraies notes.

## Exercice D — choisir les composants d’un mini-assistant

Tu veux poser des questions à trois notes de cours fictives. Décris composants nécessaires, données qui transitent, contrôles d’accès et limites de la réponse. Ne donne aucun accès à tout le disque.

## Exercice E — décision de changement

Après une semaine de test, Ollama résume vite les notes, mais produit des erreurs dans les références et ralentit ton Lenovo pendant les gros prompts. Décide si tu abandonnes, limites ou poursuis le pilote. Donne des critères mesurables.

# 23. Corrigés et critères de réussite

## Corrigé A

Ollama est le runtime/serveur; le modèle est le système de génération; l’interface est l’application où l’on pose les questions; l’API est un point d’accès pour les applications; Obsidian est l’outil qui conserve les notes. Le modèle ne lit pas un dossier simplement parce qu’il est installé : il faut une application, une extension ou du code pour lire le fichier et transmettre des extraits.

## Corrigé B

- Note fictive : modèle local adapté, utile pour apprendre et limiter les transferts cloud.
- Dossier client : uniquement un environnement local autorisé par la politique de l’organisation; sinon ne pas l’importer dans un outil personnel.
- Information actuelle : source web officielle, outil connecté ou service de recherche; un modèle local seul peut être périmé.
- Comparaison de modèles : protocole identique et questions fictives; noter qualité, erreurs, vitesse et matériel.
## Corrigé C

Vérifier notamment : données envoyées au cloud; portée d’accès au coffre; emplacement des journaux et index; permissions API; écoute réseau de l’API locale; conservation des conversations; code et réputation du plugin; possibilité de révocation et suppression des données.

## Corrigé D

Composants minimaux : Ollama avec un modèle, application de chat/RAG, trois notes de test, outil de découpage et d’indexation, et références aux passages utilisés. Démarrer avec accès en lecture seule, dossier restreint, localhost, aucun service cloud et journal d’essai. Vérifier les réponses par consultation des passages sources.

## Corrigé E

Poursuivre uniquement pour une tâche où la qualité reste acceptable, en réduisant le contexte ou le modèle et en ajoutant une vérification des références. Mesurer le temps, exactitude, gêne sur le PC et corrections. Si le résultat n’améliore pas le flux de travail, ne pas migrer et conserver la connaissance acquise sur Ollama.

## Réussite globale

- Tu peux expliquer local/cloud et préciser ce qui transite.
- Tu sais démarrer, inspecter et arrêter un modèle et vérifier les ressources utilisées.
- Tu peux identifier les composants nécessaires pour faire rechercher des notes.
- Tu sais expliquer pourquoi un assistant ne doit pas recevoir des accès excessifs.
- Tu peux décider par critères si Ollama mérite une place dans ton système.
# 24. Comment enseigner Ollama

Une première initiation peut durer 90 minutes. Son objectif n’est pas de présenter toutes les options : les apprenants doivent comprendre l’architecture, démarrer un modèle, tester une tâche simple et raisonner sur la confidentialité.

| Séquence | Temps | Activité |
| --- | --- | --- |
| Question de départ | 10 min | Qu’est-ce qui se passe quand une IA répond ? |
| Schéma des composants | 15 min | Modèle, runtime, interface, API, données. |
| Démonstration locale | 20 min | Lancer un petit modèle avec texte fictif. |
| Exercice de vérification | 15 min | Comparer une réponse à une source et repérer une erreur. |
| Confidentialité | 15 min | Comparer local, cloud, extensions et API ouverte. |
| Synthèse | 15 min | Chaque apprenant explique un bénéfice et une limite. |

Pour enseigner à un public cyber, sépare les enjeux : confidentialité des prompts, surface API, gestion des secrets, approvisionnement des modèles, permissions des agents, sauvegardes et traçabilité. Fais les démonstrations dans un environnement isolé et avec données fictives.

# 25. Glossaire anglais-français

| Anglais | Français / sens |
| --- | --- |
| Large Language Model (LLM) | Grand modèle de langage |
| Runtime / inference server | Environnement d’exécution / serveur d’inférence |
| Weights | Poids du modèle, paramètres appris |
| Quantization | Quantification, réduction de précision/taille |
| Token / context window | Token / fenêtre de contexte |
| Prompt / system prompt | Instruction / consigne système |
| Local inference | Inférence locale |
| Cloud model | Modèle hébergé dans le cloud |
| API / endpoint | Interface de programmation / adresse de service |
| Embedding | Représentation vectorielle du texte |
| RAG | Génération augmentée par récupération documentaire |
| Tool calling | Appel d’outil demandé par le modèle |
| Sandbox | Environnement isolé et limité |
| Modelfile | Fichier de configuration de modèle Ollama |
| VRAM | Mémoire vidéo du GPU |
| MCP | Protocole de connexion entre agents IA et outils |

# 26. Références officielles

Les fonctionnalités et versions évoluent. Pour l’installation et les options récentes, privilégie la documentation Ollama. Les pages Obsidian, Notion et GitHub ci-dessous aident à vérifier les capacités d’intégration et les limites des produits associés.

- Ollama — Quickstart
- Ollama — Windows
- Ollama — Linux
- Ollama — GPU support
- Ollama — Cloud and data handling
- Ollama — API introduction
- Ollama — API authentication
- Ollama — OpenAI API compatibility
- Ollama — Embeddings and RAG
- Ollama — Structured outputs
- Ollama — Modelfile reference
- Ollama — Troubleshooting and logs
- Ollama — Obsidian notes example (older technical example)
- Notion — MCP for AI tools
- Notion — API integrations
- Notion — data export and backup
- Obsidian — Community plugins security
- GitHub — Secret scanning documentation
> Conclusion  Ollama mérite d’être appris comme un nouveau composant de ton environnement IA, pas comme un remplacement automatique de ta méthode actuelle. Le premier résultat attendu est une décision informée : pour quelles tâches le local apporte un avantage, avec quelles limites et quelles protections.
