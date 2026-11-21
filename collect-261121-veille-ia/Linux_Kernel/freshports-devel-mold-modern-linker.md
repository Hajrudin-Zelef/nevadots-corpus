---
id: collect-261121-veille-ia/linux-kernel/freshports-devel-mold-modern-linker
title: "FreshPorts -- devel/mold: Modern Linker"
domain: linux-kernel
role: reference
task: article
actors: []
dates: []
keywords: ["ashish", "mold", "devel", "update", "shukla", "remove", "onetbb", "upstream"]
source: https://www.freshports.org/devel/mold/
source_anchor: ""
source_lines: [1, 106]
section: "Linux_Kernel"
---
| Searches without login should now work. You must get asked to click a button. | 
| Some translations wanted please. | 
| New RSS 2.0 and Atom feeds are here. The old /backend/ feeds still work, but will be redirected later, so please update your subscriptions. Details. | 

| Nombre de commits trouvés : 34 # Nombre de commits trouvés : 34 |  | 

| Port details | 
|    | 

| Historique des commits - (peut être incomplet : pour tous les détails, voir les liens vers les dépôts en haut de la page) |  |  | 
| Commit | Crédits | Message | 
|---|---|---|
| 3.0.0 28 oct. 2026, 13 h 49 min 39 s | Hiroki Tagato (tagattie) Auteur : Josh Megnauth | devel/mold: Update to 3.0.0  Port changes: - switch to USES=cargo since mold has been rewritten in Rust   Other irrelevant USES and CMAKE_* have also been dropped - enable on 32-bit architectures (armv7 and i386)   Linking libatomic from gcc solves undefined symbol: __atomic_load   error - remove dependency on oneTBB and GNU grep - remove options since they are not provided by upstream anymore - remove pre-test target   The testing infrastructure is cleaner now so it is not necessary   anymore - update pkg-descr  Upstream changes: https://github.com/rui314/mold/releases/tag/v3.0.0  PR:		300096 Reported by:	Josh Megnauth <jo.sh@tutanota.com> Co-authored-by:	Hiroki Tagato <tagattie@FreeBSD.org> | 
| 2.40.3 25 oct. 2026, 22 h 46 min 50 s | Hiroki Tagato (tagattie) | devel/mold: Take maintainership  MFH:		2026Q4 | 
| 2.40.3 25 oct. 2026, 20 h 42 min 11 s | Ashish SHUKLA (ashish) | devel/mold: Return to pool  MFH:		2026Q4 | 
| 2.40.3 30 juill. 2025, 19 h 19 min 25 s | Ashish SHUKLA (ashish) | devel/mold: Update to 2.40.3 | 
| 2.40.1 9 juin 2025, 11 h 39 min 40 s | Ashish SHUKLA (ashish) | devel/mold: Update to 2.40.1 | 
| 2.40.0 27 mai 2025, 09 h 27 min 24 s | Ashish SHUKLA (ashish) | devel/mold: Update to 2.40.0 | 
| 2.39.1 13 mai 2025, 09 h 07 min 14 s | Ashish SHUKLA (ashish) | devel/mold: Update to 2.39.1 | 
| 2.38.0 27 avr. 2025, 11 h 17 min 04 s | Ashish SHUKLA (ashish) | devel/mold: Update to 2.38.0 | 
| 2.36.0 11 janv. 2025, 18 h 04 min 25 s | Ashish SHUKLA (ashish) | devel/mold: Update to 2.36.0 | 
| 2.35.0 9 déc. 2024, 04 h 42 min 37 s | Ashish SHUKLA (ashish) | devel/mold: Update to 2.35.0  - Remove upstreamed/non-applicable patch - Update pre-test target with updated paths | 
| 2.32.1 16 juill. 2024, 06 h 07 min 31 s | Ashish SHUKLA (ashish) | devel/mold: Update to 2.32.1 | 
| 2.32.0 16 juill. 2024, 06 h 07 min 01 s | Ashish SHUKLA (ashish) Auteur : Daniel Engberg | devel/mold: Update to 2.32.0  - Add test target - Add option for LTO  PR:		279617 Approved by:	maintainer | 
| 2.31.0_2 4 juill. 2024, 11 h 39 min 44 s | Ganael LAPLANCHE (martymac) | devel/onetbb: Update to 2021.13.0  and bump dependent ports' revisions.  Release notes:	https://github.com/oneapi-src/oneTBB/blob/v2021.13.0/RELEASE_NOTES.md | 
| 2.31.0_1 4 mai 2024, 12 h 41 min 39 s | Ashish SHUKLA (ashish) | devel/mold: Correct LICENSE  Reported by:	upstream MFH:		2024Q2 (correct mistake with LICENSE) | 
| 2.31.0 3 mai 2024, 16 h 06 min 51 s | Ashish SHUKLA (ashish) | devel/mold: Update to 2.31.0  - Add a patch to fix building on FreeBSD, submitted upstream   at https://github.com/rui314/mold/pull/1251 | 
| 2.30.0_1 17 avr. 2024, 11 h 28 min 34 s | Ganael LAPLANCHE (martymac) | devel/onetbb: Update to 2021.12.0  and bump dependent ports' revisions.  Release notes: https://github.com/oneapi-src/oneTBB/releases/tag/v2021.12.0 | 
| 2.30.0 16 mars 2024, 12 h 07 min 43 s | Ashish SHUKLA (ashish) | devel/mold: Update to 2.30.0 | 
| 2.4.1 1 mars 2024, 21 h 18 min 31 s | Ashish SHUKLA (ashish) | devel/mold: Update to 2.4.1 | 
| 2.4.0 21 févr. 2024, 11 h 44 min 33 s | Ashish SHUKLA (ashish) | devel/mold: Remove the patch which was missed  In the last commit to this port, I missed removing the patch which was failing to apply, there by causing the build to be broken.  Sorry about this.  Reported by:	pkg-fallout | 
| 2.4.0 16 févr. 2024, 16 h 15 min 40 s | Ashish SHUKLA (ashish) Auteur : Sheng-Yi Hong | devel/mold: Update to 2.4.0  - Remove the BROKEN - Import the fix from upstream - Add ASAN option - Add LIB_DEPENDS on zstd  Differential Revision:	<https://reviews.freebsd.org/D43718> | 
| 1.2.1_6 28 nov. 2023, 11 h 35 min 02 s | Ganael LAPLANCHE (martymac) | devel/onetbb: Update to 2021.11.0  and bump dependent ports' revisions.  Release notes: https://github.com/oneapi-src/oneTBB/releases/tag/v2021.11.0 | 
| 1.2.1_5 21 août 2023, 10 h 09 min 23 s | Ganael LAPLANCHE (martymac) | devel/onetbb: Update to 2021.10.0  and bump dependent ports' revisions.  Release notes: https://github.com/oneapi-src/oneTBB/releases/tag/v2021.10.0 | 
| 1.2.1_4 9 mai 2023, 10 h 25 min 26 s | Ganael LAPLANCHE (martymac) | devel/onetbb: Update to 2021.9.0  and bump dependent ports' revisions.  Release notes: https://github.com/oneapi-src/oneTBB/releases/tag/v2021.9.0 | 
| 1.2.1_3 3 mars 2023, 15 h 01 min 50 s | Ganael LAPLANCHE (martymac) | devel/onetbb: Update to 2021.8.0  Also: - bump dependent ports' revisions - fix build when WITH_DEBUG is set  Release notes: https://github.com/oneapi-src/oneTBB/releases/tag/v2021.8.0 | 
| 1.2.1_2 22 janv. 2023, 18 h 51 min 40 s | Ashish SHUKLA (ashish) | devel/mold: Mark as BROKEN | 
| 1.2.1_2 3 nov. 2022, 11 h 32 min 41 s | Ganael LAPLANCHE (martymac) | devel/onetbb: Update to 2021.7.0  and bump dependent ports' revisions.  Release notes: https://github.com/oneapi-src/oneTBB/blob/v2021.7.0/RELEASE_NOTES.md | 
| 1.2.1_1 28 sept. 2022, 10 h 28 min 09 s | Ganael LAPLANCHE (martymac) | devel/onetbb: Update to 2021.6.0  and bump dependent ports' revisions.  Release notes: https://github.com/oneapi-src/oneTBB/blob/v2021.6.0/RELEASE_NOTES.md | 
| 7 sept. 2022, 21 h 58 min 51 s | Stefan Eßer (se) | Remove WWW entries moved into port Makefiles  Commit b7f05445c00f has added WWW entries to port Makefiles based on WWW: lines in pkg-descr files.  This commit removes the WWW: lines of moved-over URLs from these pkg-descr files.  Approved by:		portmgr (tcberner) | 
| 1.2.1 7 sept. 2022, 21 h 10 min 59 s | Stefan Eßer (se) | Add WWW entries to port Makefiles  It has been common practice to have one or more URLs at the end of the ports' pkg-descr files, one per line and prefixed with "WWW:". These URLs should point at a project website or other relevant resources.  Access to these URLs required processing of the pkg-descr files, and they have often become stale over time. If more than one such URL was present in a pkg-descr file, only the first one was tarnsfered into the port INDEX, but for many ports only the last line did contain the port specific URL to further information.  There have been several proposals to make a project URL available as a macro in the ports' Makefiles, over time.  | 
| 1.2.1 2 mai 2022, 03 h 12 min 05 s | Ashish SHUKLA (ashish) | devel/mold: Update to 1.2.1  - Update patch to avoid depending on python | 
| 1.2 16 avr. 2022, 01 h 43 min 06 s | Ashish SHUKLA (ashish) | devel/mold: Update to 1.2 | 
| 1.1.1 11 mars 2022, 19 h 52 min 20 s | Ashish SHUKLA (ashish) | devel/mold: Update to 1.1.1 | 
| 1.1 21 févr. 2022, 23 h 48 min 02 s | Ashish SHUKLA (ashish) | devel/mold: Update to 1.1 | 
| 1.0.3 8 févr. 2022, 19 h 19 min 34 s | Ashish SHUKLA (ashish) | devel/mold: New port: A Modern Linker  mold is a faster drop-in replacement for existing Unix linkers. It is several times faster than LLVM lld linker, the second-fastest open-source linker which I originally created a few years ago. mold is created for increasing developer productivity by reducing build time especially in rapid debug-edit-rebuild cycles.  WWW: https://github.com/rui314/mold | 

| Connexion | 
|---|
| Connexion utilisateur Créer un compte | 

| Ce site | 
|---|
| Qu'est-ce que FreshPorts ? À propos des auteurs Problèmes FAQ Quelle est sa taille ? Politique de sécurité Confidentialité Blog Contact | 

| Dernières vulnérabilités | 
|---|
| 44 vulnerabilities affecting 728 ports have been reported in the past 14 days | 
| <sup>*</sup> - modifiées, pas nouvelles Toutes les vulnérabilités Dernier traitement : 2026-11-21 10:07:55 UTC | 

| angie | Nov 21 | 
| angie-module-cache-purge | Nov 21 | 
| clamav | Nov 20 | 
| clamav-lts | Nov 20 | 
| py-mkdocs-material | Nov 20 | 
| chromium | Nov 19 | 
| dbus | Nov 19 | 
| linux-chrome | Nov 19 | 
| ungoogled-chromium | Nov 19 | 
| gitlab | Nov 18 | 
| libfontenc | Nov 18 | 
| libXfont2 | Nov 18 | 
| mariadb1011-server | Nov 18 | 
| mariadb114-server | Nov 18 | 
| mariadb118-server | Nov 18 | 

| Ports | 
|---|
| Accueil Catégories Ports supprimés Échecs des tests de cohérence Flux de nouvelles | 

| Statistiques | 
|---|
| Graphiques NOUVEAUX graphiques (Javascript) Calculated hourly: | 

| Port count | 35640 | 
| Broken | 62 | 
| Deprecated | 374 | 
| Ignore | 181 | 
| Forbidden | 0 | 
| Restricted | 1 | 
| No CDROM | 1 | 
| Vulnerable | 54 | 
| Expired | 12 | 
| Set to expire | 292 | 
| Interactive | 0 | 
| new 24 hours | 15 | 
| new 48 hours | 15 | 
| new 7 days | 70 | 
| new fortnight | 185 | 
| new month | 399 | 

|  | 

|  | 

| Servers and bandwidth provided by New York Internet, iXsystems, and RootBSD |  | Valide HTML, CSS et RSS. Droits d'auteur © 2000-2026 Dan Langille. Tous droits réservés. |
