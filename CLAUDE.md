# clarity-releases — conventions

**Dépôt PUBLIC.** Tout ce qui est écrit ici — fichiers, messages de commit, notes de version, journaux d’Actions —
est lisible par tous. Jamais : un chemin ou un document interne, une donnée personnelle, un secret. Ne nomme aucun
autre dépôt que celui-ci.

## Ce qui vit ici

- La page de téléchargement (`index.html`, `README.md`), servie par GitHub Pages (branche `main`, racine).
- Les installateurs, publiés en versions (Releases `v<version>`). L’appli installée s’y met à jour. Le contenu de la
  page et les versions sont déposés par la publication de l’appli : ne pas les éditer à la main sans raison.

## Minutes et stockage Actions

Dépôt public : minutes des exécuteurs standard gratuites, hors du quota des dépôts privés. Un seul workflow :
`.github/workflows/publier.yml` (« Publier une version »), **lancé à la main seulement** :

- il lit le code source (dépôt privé) avec les secrets `MIREI_SOURCE_REPO` et `MIREI_SOURCE_TOKEN` (jeton à grain fin,
  lecture seule du contenu de ce seul dépôt) ; le nom de ce dépôt-là n'est écrit nulle part ici ;
- contrôles Linux, puis vérification Windows de l'installateur (installation silencieuse, app installée), puis —
  case `publier` cochée — page de téléchargement et version `v<version>` ; case décochée = vérification seule d'une
  branche, rien n'est publié ; case `ancienne` = mise à jour depuis la 1.3.0, après publication ;
- une version déjà publiée et complète n'est jamais republiée : la version du code doit augmenter.

Règle commune pour tout changement :

1. Déclencheurs : `workflow_dispatch` (ou `push` sur `main`, `schedule` si un besoin le justifie). Jamais
   `pull_request` ni branche de travail.
2. `timeout-minutes` sur chaque job ; `concurrency` avec `cancel-in-progress: true`, sauf pour un workflow qui publie
   (`false`).
3. Coût estimé écrit en tête du fichier.
4. Pas d’`actions/upload-artifact` : un installateur va directement dans une Release. Si un artefact était vraiment
   nécessaire : `retention-days: 1` et petits fichiers seulement. Pas de gros cache.
5. Journaux publics : aucune étape n'affiche de secret, de donnée personnelle ni de chemin interne.
6. Si le dépôt repassait privé : le quota (minutes et 500 Mo de stockage) devient partagé, et ce workflow ne doit plus
   tourner tel quel (Windows ×2) ; la rétention des artefacts se règle à 1 jour (Settings › Actions › General).

## Git

Auteur `Claude <noreply@anthropic.com>` ; pas de lien de session dans les commits ; `Co-Authored-By` gardé.
