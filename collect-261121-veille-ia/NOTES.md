# NOTES — corpus `collect-261121-veille-ia`

## Génération

- source : collecte locale `Collect211126.tar.gz` (4 823 fiches) + `manquants/` (1 221 fiches régénérées depuis `articles.json`).
- aucun accès réseau : texte issu de `articles.json` / de l'archive.
- frontmatter généré : titre et URL depuis le manifeste de collecte, domaine = dossier thématique, `role=reference`, `task=article`.
- découpage à 1500 lignes maximum (`source_lines` conserve l'intervalle).

## Contenu

Veille IA & tech du 20–21 novembre 2026 : modèles, labos, matériel, régulation, langages et outils.

## Exclusions

- 7 fichiers binaires, 92 doublons exacts — voir `INDEX.md`.
