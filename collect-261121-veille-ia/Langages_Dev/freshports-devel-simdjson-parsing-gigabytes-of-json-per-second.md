---
id: collect-261121-veille-ia/langages-dev/freshports-devel-simdjson-parsing-gigabytes-of-json-per-second
title: "FreshPorts -- devel/simdjson: Parsing gigabytes of JSON per second"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["simdjson", "devel", "changelog", "github", "releases", "update", "robert", "clausecker"]
source: https://www.freshports.org/devel/simdjson
source_anchor: ""
source_lines: [1, 130]
section: "Langages_Dev"
---
| Searches without login should now work. You must get asked to click a button. | 
| Some translations wanted please. | 
| New RSS 2.0 and Atom feeds are here. The old /backend/ feeds still work, but will be redirected later, so please update your subscriptions. Details. | 

| Nombre de commits trouvés : 58 # Nombre de commits trouvés : 58 |  | 

| Port details | 
|    | 

| Historique des commits - (peut être incomplet : pour tous les détails, voir les liens vers les dépôts en haut de la page) |  |  | 
| Commit | Crédits | Message | 
|---|---|---|
| 5.1.1 7 nov. 2026, 12 h 57 min 43 s | Robert Clausecker (fuz) | devel/simdjson: update to 5.1.1  This unbreaks the test suite.  Changelog: https://github.com/simdjson/simdjson/releases/tag/v5.1.1 | 
| 5.1.0 31 oct. 2026, 19 h 55 min 43 s | Robert Clausecker (fuz) | devel/simdjson: update to 5.1.0  Changelog: https://github.com/simdjson/simdjson/releases/tag/v5.1.0  MFH:		no (soname change) | 
| 5.0.5 16 oct. 2026, 19 h 02 min 01 s | Robert Clausecker (fuz) | devel/simdjson: update to 5.0.5  Changelog: https://github.com/simdjson/simdjson/releases/tag/v5.0.2 Changelog: https://github.com/simdjson/simdjson/releases/tag/v5.0.3 Changelog: https://github.com/simdjson/simdjson/releases/tag/v5.0.4 Changelog: https://github.com/simdjson/simdjson/releases/tag/v5.0.5  MFH:		2026Q4 | 
| 5.0.1 30 sept. 2026, 17 h 46 min 55 s | Robert Clausecker (fuz) | devel/simdjson: update to 5.0.1  A major update with some minor ABI changes.  Changelog: https://github.com/simdjson/simdjson/releases/tag/v5.0.0 Changelog: https://github.com/simdjson/simdjson/releases/tag/v5.0.1  MFH:		no (soname change) | 
| 4.6.11_1 11 sept. 2026, 12 h 58 min 45 s | Robert Clausecker (fuz) | devel/simdjson: do not pass -mpower8-vector on powerpc  According to pkubaj, this makes the port build on machines older than POWER 8.  Reported by:	pkubaj MFH:		2026Q3 | 
| 4.6.11 11 sept. 2026, 12 h 49 min 21 s | Piotr Kubaj (pkubaj) | devel/simdjson: drop power8-vector requirement on powerpc64  Allows building on baseline G5.  simdjson 4.6.11 already gates its PPC64 SIMD paths on __POWER8_VECTOR__, so dropping the flag is enough. | 
| 4.6.11 6 sept. 2026, 16 h 16 min 57 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.6.11  A bug fix release fixing an issue on older FreeBSD ppc* as well as several other issues.  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.6.10 Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.6.11  MFH:		2026Q3 Event:		Berlin Hackathon 202609 Reported by:	pkubaj | 
| 4.6.9 2 sept. 2026, 10 h 49 min 35 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.6.9  A bug fix release based on 4.6.8  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.6.9 | 
| 4.6.8 23 août 2026, 20 h 45 min 57 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.6.8  A bug fix release.  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.6.7 Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.6.8  MFH:		2026Q3 | 
| 4.6.6 3 août 2026, 15 h 01 min 08 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.6.6  Another bug fix release.  MFH:		2026Q3 | 
| 4.6.5 30 juill. 2026, 16 h 52 min 41 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.6.5  A bug fix release. While we are at it, at textproc as a secondary category.  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.6.5  MFH:		2026Q3 | 
| 4.6.4 8 mai 2026, 22 h 55 min 20 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.6.4  A bug fix release.  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.6.4  MFH:		no (soname change) | 
| 4.6.3 21 avr. 2026, 20 h 14 min 30 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.6.2  A bug fix release.  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.6.3  MFH:		no (different soname) | 
| 4.6.2 18 avr. 2026, 18 h 45 min 52 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.6.2  A bug fix release.  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.6.2 | 
| 4.6.1 7 avr. 2026, 14 h 22 min 48 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.6.1  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.6.0 Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.6.1  MFH:		no (soname change) | 
| 4.5.0 30 mars 2026, 15 h 44 min 02 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.5.0   - note soname change  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.5.0 | 
| 4.4.2 24 mars 2026, 19 h 12 min 23 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.4.2  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.4.1 Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.4.2 | 
| 4.4.0 15 mars 2026, 15 h 28 min 57 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.4.0  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.4.0 | 
| 4.3.1 24 févr. 2026, 00 h 44 min 48 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.3.1  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.3.1 | 
| 4.3.0 24 févr. 2026, 00 h 44 min 47 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.3.0  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.3.0 | 
| 4.2.4 21 déc. 2025, 00 h 04 min 01 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.2.4  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.2.4 | 
| 4.2.3 21 déc. 2025, 00 h 04 min 00 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.2.3  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.2.3 | 
| 4.2.2 12 nov. 2025, 13 h 53 min 19 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.2.2  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.2.0 Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.2.1 Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.2.2  MFH:		no (soname change) | 
| 4.1.0 31 oct. 2025, 17 h 14 min 39 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.1.0  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.1.0  MFH:		no (soname change) | 
| 4.0.7 5 oct. 2025, 13 h 44 min 01 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.0.7  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.0.7  MFH:		2025Q4 | 
| 4.0.6 28 sept. 2025, 09 h 29 min 33 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.0.6  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.0.6 | 
| 4.0.5 20 sept. 2025, 23 h 47 min 25 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.0.5  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.0.5 | 
| 4.0.3 19 sept. 2025, 08 h 24 min 02 s | Robert Clausecker (fuz) | devel/simdjson: update to 4.0.3  Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.0.0 Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.0.1 Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.0.2 Changelog: https://github.com/simdjson/simdjson/releases/tag/v4.0.3 | 
| 3.13.0 9 juin 2025, 13 h 27 min 12 s | Robert Clausecker (fuz) | devel/simdjson: update to 3.13.0  Changelog: https://github.com/simdjson/simdjson/releases/tag/v3.13.0 | 
| 3.12.3 14 avr. 2025, 12 h 12 min 46 s | Robert Clausecker (fuz) | devel/simdjson: update to 3.12.3  Changelog: https://github.com/simdjson/simdjson/releases/tag/v3.12.3  MFH:		2025Q2 | 
| 3.12.2 18 févr. 2025, 14 h 16 min 19 s | Robert Clausecker (fuz) | devel/simdjson: update to 3.12.2   - license change  Changelog: https://github.com/simdjson/simdjson/releases | 
| 3.11.5 17 janv. 2025, 12 h 36 min 44 s | Robert Clausecker (fuz) | devel/simdjson: take maintainership  Grab maintainership. Touch up port lightly. | 
| 3.11.5 13 janv. 2025, 16 h 42 min 34 s | Fernando Apesteguía (fernape) | devel/simdjson: update to 3.11.5  ChangeLog: https://github.com/simdjson/simdjson/releases/tag/v3.11.5  Patch for bug #2322.  Reported by:	portscout | 
| 3.11.4 7 janv. 2025, 07 h 11 min 42 s | Vanilla I. Shu (vanilla) | devel/simdjson: update to 3.11.4. | 
| 3.11.3 22 déc. 2024, 04 h 45 min 01 s | Vanilla I. Shu (vanilla) | devel/simdjson: update 3.6.2 -> 3.11.3, also reset MAINTAINER to ports@  PR:		Älven <alster at vinterdalen.se> Approved by:	olevole at olevole.ru | 
| 3.6.2_1 16 août 2024, 15 h 33 min 49 s | Dimitry Andric (dim) | ``` devel/simdjson: fix build with clang 19  With clang 19, if a port uses simdjson.h, it leads to compile errors:      In file included from /wrkdirs/usr/ports/sysutils/intel-pcm/work/pcm-202405/src/pcm-raw.cpp:31:     /usr/local/include/simdjson.h:6156:13: error: no member named 'print_newline' in 'base_formatter<formatter>'      6156 \|       this->print_newline();           \|       ~~~~  ^     /usr/local/include/simdjson.h:6160:13: error: no member named 'print_indents' in 'base_formatter<formatter>'      6160 \|       this->print_indents(depth);           \|       ~~~~  ^     /usr/local/include/simdjson.h:6164:13: error: no member named 'print_space' in 'base_formatter<formatter>'      6164 \|       this->print_space();           \|       ~~~~  ^  Upstream <https://github.com/simdjson/simdjson/commit/5d35e7ca> fixes this, but it does not directly add the change to the generated single header version of simdjson.h. So apply the fix to both the individual header and the amalgamated version.  PR:		280590 Approved by:	olevole@olevole.ru (maintainer) MFH:		2024Q3 ```  | 
| 3.6.2 11 déc. 2023, 14 h 44 min 07 s | R. Christian McDonald (rcm) | devel/simdjson: update to v3.6.2  ChangeLogs: https://github.com/simdjson/simdjson/releases/tag/v3.1.6 https://github.com/simdjson/simdjson/releases/tag/v3.1.7 https://github.com/simdjson/simdjson/releases/tag/v3.1.8 https://github.com/simdjson/simdjson/releases/tag/v3.2.0 https://github.com/simdjson/simdjson/releases/tag/v3.2.1 https://github.com/simdjson/simdjson/releases/tag/v3.2.2 https://github.com/simdjson/simdjson/releases/tag/v3.2.3 https://github.com/simdjson/simdjson/releases/tag/v3.3.0 https://github.com/simdjson/simdjson/releases/tag/v3.4.0 https://github.com/simdjson/simdjson/releases/tag/v3.5.0 https://github.com/simdjson/simdjson/releases/tag/v3.6.0 https://github.com/simdjson/simdjson/releases/tag/v3.6.1 https://github.com/simdjson/simdjson/releases/tag/v3.6.2  PR:		275451 Reviewed by:	garga Approved by:	maintainer Sponsored by:	Rubicon Communications, LLC ("Netgate") | 
| 3.1.5 13 mars 2023, 10 h 02 min 55 s | Daniel Engberg (diizzy) Auteur : Oleg Ginzburg | devel/simdjson: Update to 3.1.5  Changelog: https://github.com/simdjson/simdjson/releases/tag/v3.1.5  PR:		270047 | 
| 3.1.2 24 févr. 2023, 16 h 35 min 11 s | Fernando Apesteguía (fernape) Auteur : Oleg Ginzburg | devel/simdjson: update to 3.1.2  ChangeLog: https://github.com/simdjson/simdjson/compare/v3.1.0...v3.1.2  PR:		269774 Reported by:	olevole@olevole.ru (maintainer) | 
| 3.1.0 27 janv. 2023, 16 h 40 min 22 s | Fernando Apesteguía (fernape) Auteur : Oleg Ginzburg | devel/simdjson: update to 3.1.0  ChangeLog: https://github.com/simdjson/simdjson/releases   * Disabling fallback kernel on systems where it is not needed.  * Testing and fixing log issues.  * complete number_in_string for documents, stringify the version macro and    improving trailing content errors.  PR:		269155 Reported by:	olevole@olevole.ru (maintainer) | 
| 3.0.1 4 janv. 2023, 21 h 16 min 58 s | Daniel Engberg (diizzy) | devel/simdjson: Update to 3.0.1  Fix "make test", it should now be available for offline testing  Changelog: https://github.com/simdjson/simdjson/releases/tag/v3.0.1  PR:		268729 Reviewed by:	Oleg Ginzburg <olevole@olevole.ru> (maintainer) | 
| 3.0.0_1 30 déc. 2022, 22 h 39 min 00 s | Nuno Teixeira (eduardo) Auteur : Dimitry Andric | devel/simdjson: Re-enable build for i386   - pet portlint  PR:		268646 | 
| 3.0.0 30 oct. 2022, 18 h 13 min 48 s | Yuri Victorovich (yuri) | devel/simdjson: Update 2.2.0 -> 3.0.0; Add tests  PR:		265305 Approved by:	olevole@olevole.ru (mnaintainer's timeout; 115 days) | 
| 7 sept. 2022, 21 h 58 min 51 s | Stefan Eßer (se) | Remove WWW entries moved into port Makefiles  Commit b7f05445c00f has added WWW entries to port Makefiles based on WWW: lines in pkg-descr files.  This commit removes the WWW: lines of moved-over URLs from these pkg-descr files.  Approved by:		portmgr (tcberner) | 
| 2.2.0 7 sept. 2022, 21 h 10 min 59 s | Stefan Eßer (se) | Add WWW entries to port Makefiles  It has been common practice to have one or more URLs at the end of the ports' pkg-descr files, one per line and prefixed with "WWW:". These URLs should point at a project website or other relevant resources.  Access to these URLs required processing of the pkg-descr files, and they have often become stale over time. If more than one such URL was present in a pkg-descr file, only the first one was tarnsfered into the port INDEX, but for many ports only the last line did contain the port specific URL to further information.  There have been several proposals to make a project URL available as a macro in the ports' Makefiles, over time.  | 
| 2.2.0 13 juill. 2022, 05 h 01 min 04 s | Yuri Victorovich (yuri) | devel/simdjson: Update 0.9.6 -> 2.2.0  PR:		258373 Approved by:	olevole@olevole.ru (maintainer's timeout; 10 months) | 
| 0.9.6 17 juin 2021, 00 h 16 min 14 s | Guangyuan Yang (ygy) Auteur : Oleg Ginzburg | devel/simdjson: Update to 0.9.6  Changes:	https://github.com/simdjson/simdjson/releases/tag/v0.9.6  PR:		256649 Approved by:	lwhsu (mentor, implicit) | 
| 0.9.5 15 juin 2021, 08 h 41 min 07 s | Piotr Kubaj (pkubaj) | devel/simdjson: fix build on powerpc64  FreeBSD uses sys/endian.h header instead of byteswap.h and bswap64() instead of bswap_64().  -mpower8-vector is necessary because the port uses new SIMD introduced in POWER8. /wrkdirs/usr/ports/devel/simdjson/work/simdjson-0.9.5/include/simdjson/ppc64/numberparsing.h:4:10: fatal error: 'byteswap.h' file not found | 
| 0.9.5 2 juin 2021, 07 h 59 min 17 s | Li-Wen Hsu (lwhsu) Auteur : Oleg Ginzburg | devel/simdjson: Update to 0.9.5  PR:		256262 | 
| 0.9.4 25 mai 2021, 09 h 46 min 41 s | Roman Bogorodskiy (novel) Auteur : Oleg Ginzburg | devel/simdjson: update to 0.9.4  Changes:	https://github.com/simdjson/simdjson/releases/tag/v0.9.4 PR:		256049 | 
| 0.9.3 16 mai 2021, 09 h 34 min 05 s | Roman Bogorodskiy (novel) Auteur : Oleg Ginzburg | devel/simdjson: update to 0.9.3  Changes:	https://github.com/simdjson/simdjson/releases PR:		255911 | 
| 0.6.0 7 avr. 2021, 08 h 09 min 01 s | Mathieu Arnold (mat) | One more small cleanup, forgotten yesterday. Reported by:	lwhsu | 
| 0.6.0 6 avr. 2021, 14 h 31 min 07 s | Mathieu Arnold (mat) | Remove # $FreeBSD$ from Makefiles. | 
| 0.6.0 1 nov. 2020, 00 h 56 min 56 s | dbaio | devel/simdjson: Update to 0.6.0  Changelog:	https://github.com/simdjson/simdjson/releases/tag/v0.6.0  PR:		250751 Submitted by:	olevole@olevole.ru (maintainer) | 
| 0.5.0 20 août 2020, 18 h 21 min 02 s | fernape | devel/simdjson: update to 0.5.0  From ChangeLog: https://github.com/simdjson/simdjson/releases/tag/v0.5.0  Performance   * Faster and simpler UTF-8 validation with the lookup4 algorithm  * We improved the performance of simdjson under Visual Studio by about 25%.    Users will still get better performance with clang-cl (+30%) but the gap    has been reduced.  Code usability   * In parse_many, when parsing streams of JSON documetns, we give to the users    runtime control as to whether threads are used (via the parser.threaded | 
| 0.4.7 9 août 2020, 06 h 17 min 55 s | lwhsu | Update to 0.4.7  PR:		248538 Submitted by:	olevole@olevole.ru (maintainer) | 
| 0.4.6 1 juill. 2020, 18 h 42 min 48 s | yuri | devel/simdjson: Update 0.3.1 -> 0.4.6  PR:		247669 Approved by:	olevole@olevole.ru | 
| 0.3.1 22 avr. 2020, 17 h 15 min 44 s | lbartoletti | [NEW PORT] devel/simdjson: Parsing gigabytes of JSON per second  JSON is everywhere on the Internet. Servers spend a lot of time parsing it. The simdjson library uses commonly available SIMD instructions and microparallel algorithms to parse JSON 2.5 times faster than anything else out there.  WWW: https://simdjson.org/  NOTA on Current, clang crashes on AMD processors before revision 359826. See: https://bugs.freebsd.org/bugzilla/show_bug.cgi?id=245745  PR:		245744 Submitted by:	Oleg Ginzburg <olevole at olevole dot ru> Approved by:	tcberner (mentor) Differential Revision:	https://reviews.freebsd.org/D24505 | 

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

| Port count | 35636 | 
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
| new 24 hours | 12 | 
| new 48 hours | 12 | 
| new 7 days | 67 | 
| new fortnight | 183 | 
| new month | 397 | 

|  | 

|  | 

| Servers and bandwidth provided by New York Internet, iXsystems, and RootBSD |  | Valide HTML, CSS et RSS. Droits d'auteur © 2000-2026 Dan Langille. Tous droits réservés. |
