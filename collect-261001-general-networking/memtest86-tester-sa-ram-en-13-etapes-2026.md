Un plantage aléatoire, un écran bleu qui revient sans raison, un rendu vidéo qui plante toujours au même pourcentage : la mémoire vive est souvent la première suspecte, et la dernière testée. Entre la pénurie qui a fait grimper les prix de la RAM de 171 % ces derniers mois (voir notre analyse de la pénurie de RAM) et la généralisation des profils XMP/EXPO poussés au-delà des specs JEDEC d’origine, un test de stabilité mémoire n’a jamais été aussi utile. MemTest86, l’outil développé par PassMark, permet de vérifier en profondeur si vos barrettes tiennent la charge avant qu’une erreur silencieuse ne corrompe un fichier ou ne fasse planter un rendu en cours.

Ce tutoriel détaille, en 13 étapes, comment créer la clé USB de démarrage, configurer le BIOS, lancer le test, lire les résultats et isoler un module défectueux, avec MemTest86 version 11.7 (Build 1000), publiée par PassMark le 4 mai 2026 et toujours désignée, à la rentrée 2026, comme la dernière version UEFI-only et comme l’« industry standard » du diagnostic mémoire sur le site officiel de l’éditeur. Cette mouture succède à la 11.6 (Build 1000) mise en ligne le 6 janvier 2026, qui avait introduit l’enregistrement local des journaux PXE. Prérequis, codes de dépannage, cas pratique complet et astuces avancées sont couverts jusqu’au bout.

## Pourquoi tester la mémoire vive de son PC en 2026

Une barrette de RAM défaillante ne se manifeste pas toujours par un écran bleu explicite. Elle peut corrompre silencieusement un fichier en cours d’écriture, faire échouer une compilation sans message clair, ou introduire un artefact isolé dans un rendu 3D que l’on attribue à tort au GPU. Les codes d’arrêt Windows comme MEMORY_MANAGEMENT, IRQL_NOT_LESS_OR_EQUAL ou PAGE_FAULT_IN_NONPAGED_AREA pointent fréquemment vers la mémoire, tout comme des installations de mises à jour qui échouent systématiquement ou des jeux qui plantent toujours au chargement d’une nouvelle zone.

Le contexte 2026 rend ce diagnostic plus pertinent qu’avant. La pénurie de mémoire a poussé certains fabricants à valider des puces venues de nouveaux fournisseurs, comme la RAM chinoise CXMT que MSI a certifiée à 8 200 MT/s sur plateforme AM5. Ces nouvelles références n’ont pas quinze ans de retours d’expérience derrière elles, ce qui justifie un test de validation avant de faire confiance à un nouveau kit. Dans le même temps, la plupart des cartes mères activent désormais un profil XMP ou EXPO par défaut au premier démarrage, poussant la mémoire au-delà de sa fréquence JEDEC certifiée sans que l’utilisateur n’ait rien demandé.

Trois situations justifient concrètement de lancer MemTest86 : un nouveau montage avant d’installer le système d’exploitation, une instabilité apparue après une mise à niveau de RAM ou de BIOS, et une demande de garantie où le fabricant exigera souvent un rapport d’erreurs daté avant de traiter le retour. Dans les trois cas, quelques dizaines de minutes de préparation évitent des heures de diagnostic à l’aveugle sur des symptômes qui ressemblent à un problème logiciel.

Le DDR5 ajoute une couche de complexité que le DDR4 ne connaissait pas. Chaque module intègre désormais son propre circuit de gestion d’alimentation (PMIC), directement sur la barrette plutôt que sur la carte mère, ce qui déplace une partie des points de défaillance possibles vers le module lui-même. Combiné à des fréquences bien plus élevées et à des marges électriques plus fines, cela explique pourquoi une instabilité qui n’existait pas en DDR4 peut apparaître sur un kit DDR5 pourtant listé comme compatible avec la carte mère. Un test MemTest86 reste le moyen le plus direct de vérifier si cette marge réduite pose réellement problème sur votre configuration précise.

## Qu’est-ce que MemTest86 et comment fonctionne l’outil

MemTest86 est un utilitaire de diagnostic qui démarre indépendamment de Windows, macOS ou Linux, directement depuis une clé USB, avant même que le système d’exploitation ne se charge. Ce fonctionnement hors OS est essentiel : il élimine toute interférence du système, du cache disque ou d’un pilote tiers, et permet de tester la totalité de l’espace d’adressage mémoire, y compris les zones normalement réservées par Windows.

L’outil a été créé par Chris Brady à la fin des années 1990, avant que la société australienne PassMark Software ne reprenne son développement commercial pour en faire la version moderne, entièrement réécrite pour le démarrage UEFI natif. MemTest86 exécute une série d’algorithmes de lecture, écriture et comparaison sur chaque adresse mémoire disponible : motifs d’inversion progressive, déplacements de blocs, séquences pseudo-aléatoires conçues pour révéler des erreurs intermittentes que de simples tests répétitifs laisseraient passer. Chaque passe complète parcourt la totalité de la RAM installée avec plusieurs de ces algorithmes enchaînés.

Ce qui distingue MemTest86 d’un simple contrôle rapide, c’est la profondeur du test. Une erreur mémoire ne se déclenche pas forcément à la première passe : certaines n’apparaissent qu’après plusieurs heures, quand la température des puces a grimpé ou quand un motif de bits particulier tombe sur une cellule marginale. C’est pourquoi la recommandation communautaire universelle consiste à laisser tourner l’outil sur plusieurs passes complètes plutôt que de s’arrêter au premier résultat propre.

## Les grandes familles de tests exécutés par MemTest86

Comprendre à grands traits ce que fait chaque famille de tests aide à mieux lire les résultats plus tard. MemTest86 enchaîne plusieurs approches complémentaires plutôt qu’un test unique, car chaque type d’erreur mémoire a sa propre signature.

- **Test d’adressage.** Vérifie que chaque puce répond bien à l’adresse mémoire qui lui correspond, sans chevauchement avec une adresse voisine, un défaut qui trahirait un problème de câblage interne au module.
- **Inversions progressives (moving inversions).** Écrit un motif, puis son inverse, sur des blocs de mémoire croissants pour détecter les interférences entre cellules adjacentes.
- **Déplacement de blocs (block move).** Copie de larges blocs de données d’une zone à l’autre pour solliciter la bande passante mémoire dans des conditions proches d’un usage réel intensif.
- **Séquences pseudo-aléatoires.** Génère des motifs de bits imprévisibles, conçus spécifiquement pour révéler des erreurs intermittentes que des motifs répétitifs et prévisibles laisseraient passer.
- **Test de rémanence (bit fade).** Écrit un motif, attend un certain temps sans y toucher, puis relit la zone pour vérifier qu’aucune cellule n’a perdu son état, un signe de dégradation physique de la puce.

Ces familles de tests s’enchaînent automatiquement dans l’ordre par défaut. Inutile de mémoriser chaque nom : ce qui compte, à l’usage, c’est de savoir qu’un résultat propre implique que la mémoire a résisté à toutes ces approches, pas seulement à la plus simple d’entre elles.

## Prérequis : versions, matériel et logiciels nécessaires

Avant de commencer, réunissez les éléments suivants. La liste inclut les versions vérifiées au moment de la rédaction (MemTest86 11.7 Build 1000, référencé par Uptodown sous le paquet Windows 1.5.1004.0 depuis le 6 janvier 2026, après les 11.5 Build 1000 du 18 septembre 2025, 11.3 du 7 mai 2025 puis 11.2 du 5 février 2025), à utiliser telles quelles ou en version ultérieure. Le développement de l’écosystème ne s’arrête pas là : côté fork open source, la branche de développement de Memtest86+ affichait encore un commit récent (référence #7726853) daté de septembre 2026 sur la page dev de Memtest.org, signe que les outils de diagnostic mémoire continuent d’évoluer au-delà de cette version de référence.

| Outil | Version testée | Usage dans ce tutoriel | 
|---|---|---|
| MemTest86 (PassMark) | 11.7 (Build 1000) ou ultérieure | Test de RAM principal, édition Free | 
| Image USB (PassMark) | Fournie avec le pack MemTest86 | Création de la clé USB officielle | 
| Rufus (alternative) | 4.14 ou ultérieure | Création manuelle de la clé USB bootable | 
| Clé USB | 1 Go minimum | Support de démarrage, entièrement effacé | 
| BIOS/UEFI | Mode UEFI activé | Requis par MemTest86 v11.7 (le mode Legacy impose l’ancienne édition V4) | 
| Windows | 10 ou 11 | Système utilisé pour préparer la clé USB | 

Un point mérite d’être clarifié avant de télécharger quoi que ce soit : MemTest86 v11.7 ne démarre qu’en UEFI. Sur une machine ancienne qui ne propose pas ce mode, PassMark met à disposition une édition V4 distincte, basée sur l’ancien BIOS historique. Vérifiez ce point dans le BIOS de votre carte mère avant de perdre du temps avec la mauvaise image.

Sauvegardez également le contenu de la clé USB choisie : la création de la clé de démarrage efface entièrement son contenu existant, sans confirmation supplémentaire une fois le processus lancé. Comptez environ 45 à 60 minutes pour dérouler l’ensemble des 13 étapes de ce tutoriel jusqu’à un premier verdict, sachant qu’une validation complète et fiable demande de laisser tourner le test plus longtemps, souvent toute une nuit.

## Étapes 1 à 4 : préparer et créer votre clé USB MemTest86

**Étape 1 : identifier les symptômes qui justifient le test.** Avant de perdre du temps, notez les circonstances exactes des plantages : toujours sous charge, toujours au repos, uniquement en jeu, ou dès le démarrage. Un crash reproductible dans un seul logiciel oriente plutôt vers un pilote ou une application. Des plantages erratiques, sans lien avec une tâche précise, ou des fichiers qui se corrompent silencieusement, pointent davantage vers la mémoire ou le stockage. Si le doute persiste entre les deux, gardez en tête un contrôle de santé SSD avec CrystalDiskInfo comme second diagnostic possible.

**Étape 2 : télécharger la bonne édition de MemTest86.** Rendez-vous sur la page de téléchargement officielle de MemTest86 et choisissez l’édition Free pour un usage personnel : sur MajorGeeks, ce téléchargement freemium ne pèse que 12 Mo et reste répertorié compatible de Windows 7 à Windows 11, fiche mise à jour le 3 mai 2026 pour la 11.7 Build 1000. Elle couvre l’essentiel : test multi-thread, détection UEFI, rapport d’erreurs à l’écran. Les éditions Pro (58 $US, environ 54 €) et Site (5 557 $US) ajoutent l’export de rapports, l’automatisation par script et le démarrage réseau PXE pour tester plusieurs machines à la fois, des fonctions surtout utiles en entreprise ou en atelier de réparation.

**Étape 3 : vérifier l’intégrité du fichier téléchargé.** Un fichier corrompu pendant le téléchargement peut produire une clé USB qui ne démarre pas ou, pire, qui donne de faux résultats. Sous Windows, comparez le hash SHA-256 du fichier téléchargé avec celui publié sur le site de PassMark :

`certutil -hashfile memtest86-usb-installer.exe SHA256`
Si la chaîne affichée ne correspond pas exactement à celle du site officiel, retéléchargez le fichier avant d’aller plus loin.

**Étape 4 : créer la clé USB bootable.** La méthode la plus simple consiste à lancer l’utilitaire Image USB fourni dans le pack MemTest86 : sélectionnez votre clé, cochez la case de création automatique, validez. En quelques minutes, la clé est prête et reconnue comme périphérique de démarrage UEFI. Si vous préférez une méthode manuelle ou si l’utilitaire fourni ne détecte pas votre clé, Rufus (version 4.14 ou ultérieure) reste l’alternative de référence : sélectionnez l’image ISO de MemTest86, laissez le schéma de partition sur GPT pour UEFI, et lancez la création.

Pour les utilisateurs à l’aise en ligne de commande, une clé bootable peut aussi se préparer manuellement avec l’outil intégré à Windows. Cette méthode demande de la rigueur : une erreur sur le numéro de disque efface le mauvais support.

```
diskpart
list disk
select disk 2
clean
create partition primary
active
format fs=fat32 quick
assign
exit
```
Remplacez le numéro « 2 » par celui de votre clé USB, identifié grâce à sa taille dans la sortie de `list disk`. Une fois le formatage terminé, copiez le contenu de l’image MemTest86 à la racine de la clé.

## Étapes 5 à 7 : configurer le BIOS/UEFI pour démarrer sur la clé USB

**Étape 5 : accéder au BIOS.** Redémarrez le PC et appuyez sur la touche d’accès au BIOS dès l’écran de démarrage constructeur. Cette touche varie selon le fabricant de la carte mère ou du portable.

| Fabricant | Touche BIOS/UEFI | Touche menu de boot rapide | 
|---|---|---|
| ASUS | Suppr (Del) ou F2 | F8 | 
| MSI | Suppr (Del) | F11 | 
| Gigabyte | Suppr (Del) | F12 | 
| ASRock | F2 ou Suppr (Del) | F11 | 
| Dell | F2 | F12 | 
| HP | Échap (Esc) puis F10 | F9 | 
| Lenovo | F1 ou F2 | F12 | 
| Acer | F2 ou Suppr (Del) | F12 | 

**Étape 6 : vérifier le Secure Boot.** MemTest86 v11.7 est signé numériquement et démarre normalement même avec le Secure Boot actif. Sur certaines configurations strictes ou d’anciennes révisions de BIOS, il arrive que le démarrage soit malgré tout bloqué. Si la clé USB n’apparaît pas dans la liste des périphériques de démarrage, désactivez temporairement le Secure Boot dans l’onglet Sécurité du BIOS, testez, puis réactivez-le une fois le diagnostic terminé.

**Étape 7 : modifier l’ordre de démarrage.** Deux options s’offrent à vous : passer par le menu de boot rapide (touche dédiée listée dans le tableau ci-dessus) pour un démarrage ponctuel sans toucher à la configuration permanente, ou entrer dans l’onglet Boot du BIOS pour placer la clé USB en première position de la liste. La première méthode est recommandée pour ce tutoriel : elle évite d’oublier de remettre le disque système en priorité après le test.

## Étapes 8 et 9 : lancer et configurer MemTest86

**Étape 8 : le menu de configuration au démarrage.** Une fois la clé USB sélectionnée, MemTest86 affiche un écran de configuration avant de lancer le moindre test. C’est ici que vous choisissez la disposition du clavier et, si besoin, la langue de l’interface (le français fait partie des langues proposées). Prenez le temps de vérifier que la quantité de RAM détectée correspond bien à ce qui est physiquement installé : un écart signale déjà un problème, avant même le premier test.

**Étape 9 : choisir le nombre de passes et les tests à exécuter.** Par défaut, MemTest86 enchaîne l’ensemble de ses algorithmes de test sur un nombre de passes défini. Pour un premier diagnostic, laissez la configuration par défaut et lancez le test avec la touche F10. Pour une validation plus poussée après avoir trouvé des erreurs, le menu de configuration permet de désélectionner certains tests et de vous concentrer sur ceux qui ont déclenché une alerte, afin de confirmer le problème plus rapidement lors des passes suivantes.

Voici, à titre d’illustration, la structure typique de cet écran de configuration :

```
MemTest86 v11.7 - Configuration
------------------------------------------------
Langue du clavier      : Français
RAM détectée            : 32768 Mo (32 Go)
Tests sélectionnés      : Tous (par défaut)
Nombre de passes        : Continu jusqu'à interruption
CPU / Threads utilisés  : Tous les cœurs disponibles
[F1] Aide   [F2] Config avancée   [F10] Démarrer le test
```
## Étape 10 : lire et interpréter les résultats du test

Pendant le test, l’écran principal affiche en permanence le pourcentage de progression de la passe en cours, le nombre total de passes effectuées, le temps écoulé et, surtout, un compteur d’erreurs. Tant que ce compteur reste à zéro, la RAM se comporte correctement sur les motifs testés jusque-là. Voici un exemple simplifié de ce à quoi ressemble un rapport propre après plusieurs passes :

```
Passe : 4 / Continu       Temps écoulé : 03:12:47
Test en cours : #8 [Random Number Sequence]
Progression : 100 %
Erreurs détectées : 0
Statut : AUCUNE ERREUR - RAM stable sur 4 passes complètes
```
Quand une erreur survient, MemTest86 l’affiche en temps réel dans la partie basse de l’écran, avec le numéro du test concerné, l’adresse mémoire fautive et l’écart entre la valeur attendue et la valeur lue. Un exemple simplifié de ce type de rapport :

```
Passe : 1 / Continu       Temps écoulé : 00:47:12
Test en cours : #5 [Block Move]
Erreurs détectées : 3
------------------------------------------------
Erreur 1 : Adresse 0x3A2F1000 - Attendu FFFFFFFF, Lu FFFFFFFB
Erreur 2 : Adresse 0x3A2F1004 - Attendu FFFFFFFF, Lu FFFFFFFB
Erreur 3 : Adresse 0x7C0E8A20 - Attendu 00000000, Lu 00000004
```
Une erreur isolée sur une seule adresse, trouvée une seule fois sur plusieurs passes, mérite d’être surveillée mais ne condamne pas forcément le module. Des erreurs répétées sur la même plage d’adresses, ou qui se multiplient au fil des passes, indiquent en revanche un problème matériel réel qui justifie de passer aux étapes suivantes.

## Étapes 11 à 13 : isoler, corriger et revérifier une RAM défectueuse

**Étape 11 : isoler le module fautif par dichotomie.** Sur une configuration à plusieurs barrettes, retirez tous les modules sauf un et relancez le test dans le même slot. Répétez l’opération barrette par barrette. Si une seule provoque des erreurs quel que soit le slot utilisé, le module est probablement en cause. Si les erreurs apparaissent uniquement dans un slot précis, quel que soit le module testé, le problème vient plutôt du socket ou de la carte mère elle-même, pas de la RAM.

**Étape 12 : appliquer une correction.** Plusieurs pistes, par ordre de simplicité : réenclencher physiquement les barrettes après avoir nettoyé les contacts dorés avec une gomme douce, désactiver le profil XMP/EXPO pour revenir à la fréquence JEDEC certifiée et voir si les erreurs disparaissent, ou mettre à jour le BIOS si le constructeur a publié un correctif de microcode mémoire (voir notre tutoriel de mise à jour BIOS sur plateforme AM5 pour la procédure complète). Si aucune de ces pistes ne résout le problème, la barrette est probablement défectueuse et il faut engager une procédure de retour auprès du fabricant, comme le service support de Crucial pour cette marque.

**Étape 13 : revérifier après chaque changement.** Après toute intervention, quelle qu’elle soit, relancez MemTest86 pour au moins deux passes complètes avant de conclure. Une seule passe propre après un changement de configuration ne suffit pas à garantir la stabilité : c’est justement le genre de faux positif que ce tutoriel cherche à éviter.

## Identifier votre configuration RAM en ligne de commande

Avant de démonter quoi que ce soit, il est utile de savoir précisément quels modules sont installés, dans quels slots, et à quelle vitesse ils tournent réellement. Sous Windows, PowerShell donne cette information sans ouvrir le boîtier :

`Get-CimInstance Win32_PhysicalMemory | Select-Object BankLabel, Manufacturer, PartNumber, Capacity, Speed, ConfiguredClockSpeed`
Cette commande liste chaque barrette avec son emplacement, son fabricant, sa référence exacte, sa capacité et sa vitesse configurée. La référence (PartNumber) est justement ce que le support technique du fabricant demandera en cas de RMA. Sur un système plus ancien ou en invite de commande classique, l’équivalent existe aussi :

`wmic memorychip get BankLabel, Manufacturer, PartNumber, Capacity, Speed`
Notez les résultats avant de commencer les tests d’isolement de l’étape 11 : cela évite de confondre les slots une fois les barrettes retirées et remises dans un ordre différent.

## Retrouver les erreurs mémoire déjà enregistrées par Windows

Avant même de créer une clé USB, Windows a peut-être déjà enregistré des indices utiles. Chaque plantage lié à la mémoire laisse une trace dans l’Observateur d’événements, sous forme de code d’arrêt (bug check) associé au fichier minidump généré lors du crash. Ouvrez l’Observateur d’événements, puis Journaux Windows > Système, et filtrez sur la source « BugCheck » pour retrouver l’historique de ces incidents.

Les codes suivants pointent le plus souvent vers un problème mémoire plutôt que vers un pilote ou une application isolée :

- **0x0000001A (MEMORY_MANAGEMENT).** Le gestionnaire de mémoire Windows a détecté une incohérence grave dans la gestion des pages mémoire.
- **0x0000000A (IRQL_NOT_LESS_OR_EQUAL).** Un pilote a tenté d’accéder à une zone mémoire à un niveau de priorité incorrect, parfois révélateur d’une adresse physique corrompue.
- **0x00000050 (PAGE_FAULT_IN_NONPAGED_AREA).** Le système a tenté de lire une donnée absente d’une zone mémoire qui ne devrait jamais être vidée sur le disque.
- **0x0000007F (UNEXPECTED_KERNEL_MODE_TRAP).** Erreur matérielle de bas niveau, parfois liée au CPU mais fréquemment corrélée à une instabilité mémoire sous charge.

Retrouver plusieurs occurrences de ces codes sur une période courte constitue un signal fort pour lancer MemTest86 sans attendre. À l’inverse, l’absence totale de ces codes, combinée à des plantages qui touchent toujours le même logiciel, oriente plutôt vers une cause logicielle et rend un test mémoire moins prioritaire.

## MemTest86 vs MemTest86+ vs Windows Memory Diagnostic : quel outil choisir

MemTest86 n’est pas le seul outil disponible pour tester la mémoire vive. Deux alternatives reviennent régulièrement dans les recommandations, chacune avec un usage différent.

### MemTest86 (PassMark) : la référence UEFI moderne

C’est l’outil couvert dans ce tutoriel. Développement actif — la version 11.7 (Build 1000), publiée en mai 2026, reste à ce jour la dernière release UEFI-only répertoriée sur le site de PassMark —, interface UEFI native, support multi-thread pour accélérer les passes sur les configurations à nombreux cœurs, et éditions payantes pour l’automatisation en entreprise. C’est le choix par défaut pour un diagnostic fiable sur du matériel récent.

### MemTest86+ : l’alternative open source

Après le passage de MemTest86 sous développement propriétaire, une partie de la communauté a maintenu un fork entièrement open source sous le nom de MemTest86+, aujourd’hui développé par le mainteneur x86fr. La version 8.00, sortie le 24 novembre 2025, avait déjà apporté un installeur USB pour Windows, plusieurs images ISO Linux et des binaires LA64, mais aussi — selon TechPowerUp et Phoronix — un rapport de température DDR5, un mode sombre optionnel, la prise en charge des derniers processeurs Intel et AMD avec une détection accélérée des configurations multi-cœurs, et la fusion en un seul binaire capable de démarrer aussi bien en UEFI qu’en Legacy ; la 8.10 qui a suivi, publiée le 16 mai 2026, ajoute pour sa part le support x2APIC, une meilleure gestion des puces Panther Lake, un correctif des timings LPDDR5 et une mesure de bande passante mémoire améliorée pour le cache et la RAM, selon Memtest86+ et PauseHardware. C’est l’option à privilégier pour les utilisateurs Linux qui préfèrent un outil dont le code reste consultable, ou pour l’intégrer à une distribution de secours personnalisée — cette même 8.10 a également livré des correctifs GRUB et Secure Boot ainsi que des améliorations LA64, et Gentoo a intégré l’ebuild memtest86+-8.10 dès le 18 mai 2026, avec un correctif Secure Boot supplémentaire livré deux jours plus tard. Consultez la page Wikipédia consacrée à l’historique de MemTest86 pour retracer la scission entre les deux projets.

### Windows Memory Diagnostic : l’outil intégré, pour un contrôle rapide

Windows embarque depuis longtemps son propre outil, accessible en tapant `mdsched.exe` dans la recherche ou la boîte Exécuter. Il redémarre le PC et lance un test basique, sans avoir besoin d’une clé USB séparée. Pratique en dépannage express, il reste nettement moins approfondi que MemTest86 : moins d’algorithmes, pas de support multi-thread comparable, et une interface qui donne moins de détails sur les erreurs trouvées.

| Critère | MemTest86 (Free) | MemTest86+ | Windows Memory Diagnostic | 
|---|---|---|---|
| Prix | Gratuit | Gratuit, open source | Gratuit, intégré | 
| Version de référence | 11.7 (Build 1000) | 8.10 | Intégré à Windows 10/11 | 
| Démarrage UEFI natif | Oui | Oui | Oui (via redémarrage Windows) | 
| Support multi-thread | Oui | Oui | Non | 
| Clé USB requise | Oui | Oui | Non | 
| Usage idéal | Diagnostic approfondi | Utilisateurs Linux / open source | Vérification rapide sans préparation | 

En pratique, Windows Memory Diagnostic sert de premier réflexe quand vous n’avez pas de clé USB sous la main, mais MemTest86 reste l’outil à utiliser pour un verdict fiable, notamment avant une demande de garantie où un rapport plus détaillé pèse davantage dans le dossier.

## Cas pratique complet : valider un kit DDR5 32 Go après passage en EXPO

Voici, à titre d’illustration, le déroulé complet d’un cas représentatif de ce que rencontrent beaucoup d’utilisateurs après un montage récent. Un kit DDR5 32 Go (2×16 Go) vient d’être installé sur une carte mère AM5, avec le profil EXPO activé dans le BIOS pour atteindre la fréquence annoncée par le fabricant. Après quelques jours d’utilisation, des plantages apparaissent, mais seulement pendant les sessions de jeu longues, jamais en bureautique.

Premier réflexe : identifier la configuration exacte avec la commande PowerShell de la section précédente, pour confirmer la vitesse configurée face à la vitesse annoncée sur la boîte. Ensuite, création de la clé USB MemTest86 via l’utilitaire Image USB, démarrage sur la clé via le menu de boot rapide de la carte mère, et lancement d’un premier test avec le profil EXPO toujours actif.

Après un peu moins d’une heure, la première passe remonte plusieurs erreurs concentrées sur les mêmes plages d’adresses, un signe caractéristique d’un profil trop agressif plutôt que d’un module réellement mort. Retour au BIOS, désactivation de l’EXPO, retour à la fréquence JEDEC de base : nouveau test, quatre passes complètes cette fois, zéro erreur. Le diagnostic se confirme : les barrettes ne sont pas défectueuses, mais le profil EXPO fourni par défaut est trop tendu pour cette combinaison précise de carte mère et de kit mémoire, un scénario d’autant plus fréquent avec des références encore jeunes comme les kits validés récemment par MSI pour la RAM CXMT sur AM5.

Dernière étape : plutôt que de rester à la fréquence JEDEC de base, ce qui gâche l’intérêt du kit, un ajustement manuel réduit légèrement la fréquence EXPO tout en augmentant de façon mesurée la tension DIMM. Un nouveau cycle de quatre passes confirme la stabilité à ce réglage intermédiaire. Le système tourne depuis sans plantage, avec un gain de fréquence conservé par rapport au JEDEC pur.

## Les erreurs les plus fréquentes à éviter

- **S’arrêter après une seule passe.** Beaucoup d’erreurs mémoire n’apparaissent qu’après plusieurs heures, quand la température a grimpé ou qu’un motif de test particulier tombe sur la bonne cellule. Une passe unique sans erreur ne garantit rien.
- **Ignorer le mode UEFI/Legacy.** Créer une clé avec la mauvaise image et s’étonner qu’elle ne démarre pas fait perdre un temps précieux. Vérifiez le mode de boot de la carte mère avant de télécharger.
- **Ne jamais comparer JEDEC et XMP/EXPO.** Tester uniquement avec le profil activé empêche de savoir si le problème vient d’une barrette défectueuse ou simplement d’un profil trop agressif pour la configuration.
- **Oublier de sauvegarder la clé USB avant de la formater.** La création de la clé bootable efface tout son contenu sans avertissement supplémentaire.
- **Condamner un module après une seule erreur isolée.** Une erreur unique, non reproduite sur les passes suivantes, mérite une surveillance mais pas un retour immédiat en garantie.
- **Laisser un test tourner plusieurs heures sans surveiller la température.** Une pièce mal ventilée peut faire grimper la température des barrettes et fausser l’interprétation d’erreurs liées à la chaleur plutôt qu’à un défaut permanent.

## Dépannage : problèmes courants et leurs solutions

| Problème | Cause probable | Solution | 
|---|---|---|
| La clé USB n’apparaît pas au démarrage | Mode de boot incompatible (Legacy au lieu d’UEFI) | Basculer le BIOS en UEFI ou recréer la clé avec l’image V4 pour Legacy | 
| Écran noir après sélection de la clé | Port USB ou sortie vidéo incompatible avec le mode d’affichage utilisé | Essayer un port USB à l’arrière de la carte mère plutôt qu’un hub en façade | 
| Blocage à 0 % de progression | Image USB corrompue ou téléchargement incomplet | Revérifier le hash SHA-256 du fichier et refaire la clé USB | 
| Redémarrage en boucle avant la fin du test | RAM trop instable pour tenir même le temps du test | Réduire manuellement la fréquence RAM dans le BIOS avant de retester | 
| Erreurs uniquement en profil XMP/EXPO | Profil trop agressif pour ce kit ou ce CPU précis | Revenir au JEDEC, puis augmenter légèrement la tension DIMM par petits paliers | 
| Adresses d’erreur qui changent à chaque passe | Timings secondaires trop serrés ou tension VSOC insuffisante | Assouplir les timings secondaires et tertiaires dans le BIOS | 
| MemTest86 ne détecte pas toute la RAM installée | Barrette mal enclenchée ou combinaison hors liste QVL de la carte mère | Réenclencher chaque module et vérifier la liste de compatibilité du fabricant | 
| Le clavier ne répond pas dans MemTest86 | Pilote USB non chargé pour ce port ou ce hub particulier | Utiliser un port USB natif à l’arrière du boîtier, éviter les hubs et rallonges | 

## Astuces avancées pour les configurations exigeantes

Une fois les bases maîtrisées, quelques techniques supplémentaires permettent d’aller plus loin dans le diagnostic, en particulier sur les configurations de jeu haut de gamme ou les petits serveurs personnels.

- **Dichotomie par slot, pas seulement par barrette.** Si toutes les barrettes échouent dans un même slot mais réussissent ailleurs, le problème vient du socket de la carte mère, pas de la mémoire elle-même.
- **Tester chaque module avant l’assemblage final.** Sur un nouveau montage, valider chaque barrette individuellement avant de tout assembler évite de chercher une aiguille dans une botte de foin après coup.
- **Surveiller les températures en parallèle.** Sous Windows, HWiNFO permet de suivre en temps réel les tensions et températures avant de lancer un test long, pour repérer une dérive thermique qui fausserait le diagnostic.
- **Exploiter le mode PXE pour les parcs de machines.** Les éditions Pro et Site de MemTest86 permettent de démarrer plusieurs postes sur le réseau sans préparer une clé USB par machine, un gain de temps net pour un atelier ou une petite entreprise.
- **Revalider systématiquement après une mise à jour BIOS.** Un changement de microcode mémoire (AGESA côté AMD) peut améliorer ou dégrader la stabilité d’un profil EXPO déjà validé : retestez après chaque mise à jour, y compris suite à une mise à jour BIOS AM5.
- **Ne pas oublier le stockage si le doute persiste.** Si la RAM sort propre de quatre passes complètes mais que les plantages continuent, un contrôle SSD avec CrystalDiskInfo permet d’écarter une autre cause matérielle courante.
- **Documenter chaque test pour un usage professionnel.** Un atelier de réparation ou un service informatique a intérêt à photographier l’écran final de chaque test, avec date et référence du module, pour constituer un historique consultable en cas de retour client.

## Ce que recommande la communauté overclocking

La méthode décrite dans ce tutoriel rejoint ce que les passionnés d’overclocking mémoire répètent depuis des années sur les forums spécialisés. Dans un fil consacré à la stabilité mémoire DDR4/DDR5 sur Overclock.net, un contributeur résume l’approche à privilégier : « My advice, and the way that I test, is to test the RAM outside the OS using Memtest86 by PassMark. » Autrement dit, tester hors du système d’exploitation reste la référence, exactement la logique suivie depuis l’étape 1 de ce guide.

Sur la durée du test, le même échange donne un repère chiffré que partagent de nombreux overclockers expérimentés : « 4 cycles of tests 0 to 9 […] should be enough to tell if the RAM is stable. » Ce seuil de quatre passes complètes correspond précisément à celui utilisé dans le cas pratique de ce tutoriel, et constitue un bon compromis entre un test trop court qui laisse passer des erreurs intermittentes et une validation qui s’éternise inutilement.

## Foire aux questions

**MemTest86 est-il vraiment gratuit ?**

Oui, l’édition Free couvre l’essentiel du diagnostic pour un usage personnel : détection UEFI, test multi-thread, rapport d’erreurs à l’écran. Les éditions Pro et Site ajoutent l’export de rapports et l’automatisation réseau, utiles surtout en entreprise.

**Combien de temps faut-il laisser tourner MemTest86 ?**

Au minimum quatre passes complètes avant de conclure à une stabilité, selon le repère communément admis par la communauté overclocking. La durée exacte dépend de la capacité de RAM installée et de sa vitesse : mieux vaut lancer le test le soir et lire le résultat le lendemain matin plutôt que de se limiter à une passe rapide.

**MemTest86 fonctionne-t-il sur Mac ou seulement sur PC ?**

MemTest86 prend en charge les Mac à processeur Intel ainsi que les machines ARM récentes, en plus de la grande majorité des PC x86 sous UEFI. La procédure de création de clé USB reste globalement la même.

**Quelle est la différence entre MemTest86 et MemTest86+ ?**

MemTest86, développé par PassMark, est un logiciel propriétaire avec des éditions payantes pour l’entreprise. MemTest86+ est un fork resté open source, maintenu par le développeur x86fr, privilégié par les utilisateurs qui veulent un code entièrement consultable.

**Dois-je désactiver le XMP/EXPO avant de tester ?**

Pas nécessairement au premier test, mais si des erreurs apparaissent, retestez toujours au JEDEC pour savoir si la cause est le profil ou le module lui-même. C’est l’étape qui distingue un vrai défaut matériel d’un réglage trop optimiste.

**Que faire si MemTest86 ne démarre pas depuis la clé USB ?**

Vérifiez d’abord le mode UEFI/Legacy du BIOS, puis le Secure Boot, et enfin l’intégrité du fichier téléchargé via son hash SHA-256. Ces trois causes couvrent la grande majorité des échecs de démarrage.

**MemTest86 peut-il endommager ma RAM ou mon PC ?**

Non. L’outil se contente de lire et écrire des motifs de test dans la mémoire, sans modifier de paramètre matériel. Le pire scénario reste un système qui redémarre si la RAM est trop instable pour tenir le temps du test.

**L’outil Windows Memory Diagnostic intégré suffit-il ?**

Pour un premier contrôle rapide sans préparation, oui. Pour un diagnostic qui doit trancher entre un module défectueux et un simple réglage instable, ou pour appuyer une demande de garantie, MemTest86 reste plus complet et plus convaincant.

**Faut-il tester la RAM ECC de la même façon ?**

La RAM ECC corrige déjà les erreurs à un bit à la volée et journalise les corrections effectuées, mais un test MemTest86 reste utile pour détecter les erreurs à plusieurs bits qu’un système ECC standard ne peut pas toujours corriger. La procédure de création de clé et de lancement du test reste identique.

**Un test MemTest86 propre garantit-il zéro problème futur ?**

Non, et c’est une nuance importante. Un module peut se dégrader avec le temps, sous l’effet de la chaleur ou d’une tension excessive prolongée. Un résultat propre aujourd’hui donne une forte confiance sur l’instant présent, pas une garantie à vie. Un nouveau test après un changement matériel ou une longue période d’usage intensif reste une bonne pratique.

Un dernier conseil pour finir : gardez la clé USB MemTest86 dans votre boîte à outils informatique. Entre les upgrades de RAM, les mises à jour de BIOS et les nouveaux montages, l’occasion de la ressortir revient plus souvent qu’on ne le pense, surtout tant que les prix de la mémoire resteront sous tension. D’autres ressources existent aussi pour approfondir le sujet, comme le guide de test RAM publié par PCWorld.

### Related Coverage

Retrouvez tous nos guides matériel dans la catégorie Matériel de Tech Insider.