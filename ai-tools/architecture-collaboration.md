# Architecture de collaboration entre les outils

## Objectif

Faire travailler ensemble plusieurs outils sans supposer qu’ils partagent automatiquement une mémoire ou qu’ils ont accès aux mêmes fichiers. Chaque transfert doit être choisi et contrôlé.

## Rôle de chaque outil

| Outil | Rôle dans le flux de travail | Point de vigilance |
|---|---|---|
| **ChatGPT** | Cadrer une question, expliquer un concept, structurer une démarche et préparer une analyse. | Vérifier les faits, les sources et la fraîcheur des informations. |
| **Ollama** | Exécuter des modèles compatibles sur une machine locale et expérimenter avec des données de test. | Le modèle ne lit pas automatiquement les fichiers; l’application ou le connecteur détermine les données qui lui sont transmises. |
| **Obsidian** | Conserver des notes durables dans un coffre de fichiers Markdown et relier les connaissances. | Une extension peut accéder aux notes ou transmettre des extraits; limiter l’essai à un coffre fictif. |
| **Notion** | Organiser des pages, bases et informations de projet dans un espace collaboratif hébergé. | Une connexion à Ollama n’est pas automatique. Export, API ou connecteur ont chacun des permissions et des flux de données à vérifier. |
| **GitHub** | Versionner et partager les cours, procédures, scripts et exemples que l’on a décidé de publier. | Un dépôt public n’est pas un coffre-fort : aucun secret, contenu personnel ou donnée réelle sensible ne doit y être ajouté. |

## Flux de travail conseillé

1. **Définir la tâche** : résumé, recherche documentaire, rédaction, comparaison ou suivi de projet.
2. **Qualifier les données** : fictives, publiques, personnelles, professionnelles ou confidentielles.
3. **Choisir l’outil** selon le besoin et le niveau de confidentialité acceptable.
4. **Transmettre le minimum nécessaire** : un fichier ou un extrait sélectionné, pas un accès global au poste ou au compte.
5. **Vérifier le résultat** à partir des notes ou des sources de référence.
6. **Conserver la connaissance validée** dans Obsidian ou Notion selon son usage.
7. **Publier dans GitHub seulement la partie partageable**, après suppression des secrets et données sensibles.

## Exemple : documenter une expérience Ollama

- ChatGPT aide à cadrer le test et à préparer une grille d’évaluation.
- Ollama exécute un modèle sur la machine de test.
- Un coffre Obsidian fictif ou un espace Notion de démonstration contient quelques notes non sensibles.
- L’utilisateur vérifie les réponses et les passages cités.
- La procédure reproductible, les prompts fictifs et les résultats expurgés sont versionnés dans GitHub.

## Ce qui n’est pas automatique

- Installer Ollama ne lui donne pas accès à Obsidian, Notion ou à l’ensemble du disque.
- Un fichier dans GitHub ne devient pas la mémoire commune de ChatGPT, Ollama, Obsidian et Notion.
- Une API, une extension ou un serveur MCP ajoute une connexion explicite, avec ses propres permissions, journaux et risques.
- Une intégration capable de lire des données n’a pas nécessairement besoin du droit de les modifier.

## Règles de sécurité de départ

- Utiliser des données fictives pour apprendre et démontrer.
- Garder un serveur local à l’ordinateur tant qu’un accès réseau n’est pas nécessaire et compris.
- Protéger les tokens hors du dépôt et accorder à chaque intégration le minimum de permissions.
- Examiner les extensions et connecteurs avant de leur confier des données.
- Prévoir une révocation des accès et une suppression des données de test.
- Demander une validation humaine avant toute écriture, publication ou action automatisée.

Cette page décrit une architecture de travail à évaluer. Elle ne signifie pas que les outils sont déjà connectés entre eux.
