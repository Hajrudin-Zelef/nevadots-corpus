---
id: collect-261121-veille-ia/google-ecosystem/nouveautes-de-chrome-152-blog-chrome-for-developers
title: "Nouveautés de Chrome 152 | Blog | Chrome for Developers"
domain: google-ecosystem
role: reference
task: article
actors: ["Google"]
dates: []
keywords: ["chrome", "performances", "listes", "autorisation", "connexion", "cette", "version", "consultez"]
source: https://developer.chrome.com/blog/new-in-chrome-152/
source_anchor: ""
source_lines: [1, 47]
section: "Google_Ecosystem"
---
Chrome 152 est en cours de déploiement. Cet article présente certaines des principales fonctionnalités de cette version. Consultez les notes de version complètes de Chrome 152.

Points essentiels de cette version :

- `CSSPseudoElement` prise en charge des pseudo-éléments supplémentaires étend l'interface à`::backdrop` ,`::scroll-marker` et`::view-transition` .
- L'API CPU Performance permet aux applications Web de déterminer les performances du processeur de l'appareil d'un utilisateur.
- Les listes d'autorisation de connexion offrent un contrôle explicite au niveau du réseau sur les points de terminaison externes à l'aide d'un en-tête de réponse HTTP.

## Prise en charge de `CSSPseudoElement` pour `::backdrop`, `::scroll-marker` et `::view-transition`

La prise en charge de `CSSPseudoElement` en JavaScript, précédemment définie pour `::after`, `::before` et `::marker`, s'étend à plusieurs nouveaux pseudo-éléments dans Chrome 152 :

- `::backdrop` : vous permet de gérer les interactions sur les arrière-plans des boîtes de dialogue modales. Par exemple, vous pouvez fermer une boîte de dialogue lorsqu'un utilisateur clique sur l'arrière-plan sans interférer avec les clics dans le contenu de la boîte de dialogue, ce qui élimine le besoin de calculs d'intersection complexes.
- `::scroll-marker` : permet de gérer les interactions sur les marqueurs de défilement, par exemple en collectant des statistiques de clics.
- `::view-transition` : ouvre la voie aux transitions de vue sensibles à la géométrie et à l'interception d'une transition de vue active en plein vol à l'aide des coordonnées des éléments en cours d'animation.

Pour en savoir plus sur l'interaction avec les pseudo-éléments en JavaScript, consultez la documentation CSSPseudoElement sur MDN.

## API CPU Performance

Chrome 152 introduit l'API CPU Performance, qui permet aux applications Web de déterminer les performances du processeur de l'appareil d'un utilisateur.

Les applications Web peuvent utiliser ces informations de niveau pour adapter l'expérience utilisateur aux capacités de l'appareil, par exemple en ajustant la fidélité graphique, en ajustant les calculs lourds ou en personnalisant les tâches en arrière-plan. Cette API peut également être utilisée en combinaison avec l'API Compute Pressure, qui fournit des données en temps réel sur la pression et l'utilisation du processeur.

Les utilisateurs peuvent remplacer le niveau de performances signalé depuis **Paramètres > Performances > Vitesse > Remplacer le niveau de performances du processeur** dans le navigateur Chrome. Les administrateurs peuvent également contrôler ce comportement à l'aide de la `CpuPerformanceTierOverride` règle d'entreprise.

## Listes d'autorisation de connexion

Chrome 152 est fourni avec les listes d'autorisation de connexion, un mécanisme de sécurité qui offre un contrôle explicite sur les points de terminaison externes en limitant les connexions réseau initiées par un document ou un service worker.

Les listes d'autorisation de connexion offrent une méthode directe pour résoudre ces risques en faisant du navigateur le gardien de toutes les connexions réseau provenant de votre page. En incluant l'en-tête de réponse HTTP `Connection-Allowlist`, un site spécifie les formats d'URL exacts autorisés pour toutes les communications réseau initiées par son contexte.

Pour en savoir plus sur les options de configuration et de création de rapports, consultez l'essai Origin Trial des listes d'autorisation de connexion.

## Documentation complémentaire

Cette section ne couvre que quelques points essentiels. Consultez les liens suivants pour découvrir les autres modifications apportées à Chrome 152 :

- Notes de version de Chrome 152
- Mises à jour de ChromeStatus.com pour Chrome 152.
- Calendrier des versions de Chrome.

## S'abonner

Pour ne rien manquer, abonnez-vous à la chaîne YouTube Chrome Developers. Vous recevrez une notification par e-mail chaque fois que nous publierons une nouvelle vidéo. Vous pouvez également nous suivre sur X ou LinkedIn pour découvrir de nouveaux articles et billets de blog.

Dès que Chrome 153 sera disponible, nous vous présenterons les nouveautés de Chrome.
