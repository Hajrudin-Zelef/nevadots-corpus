---
id: collect-261121-veille-ia/cursor-ide/point-sur-notre-recent-incident-de-securite-cursor
title: "Point sur notre récent incident de sécurité · Cursor"
domain: cursor-ide
role: reference
task: article
actors: ["Anysphere"]
dates: []
keywords: ["nous", "cursor", "avons", "informations", "dauthentification", "incident", "securite", "donnees"]
source: https://cursor.com/blog/security-incident-update
source_anchor: ""
source_lines: [1, 38]
section: "Cursor_IDE"
---
Le 29 octobre, nous avons annoncé que nous enquêtions sur un incident de sécurité ainsi que sur une copie malveillante et non autorisée de l’extension Claude Code d’Anthropic, apparue dans la Place de marché des extensions VS Code de Cursor. Au terme d’une investigation approfondie menée ces deux dernières semaines, nous souhaitons vous apporter davantage de contexte sur l’attaque et son impact.

La grande majorité de nos clients n’a pas été affectée, et nous avons contacté directement chaque client concerné pour lui transmettre les informations qui le concernent.

Garantir la sûreté des données de nos clients est une priorité fondamentale que nous prenons très au sérieux. Cet incident a touché des données que nous stockions et partagions, des informations d’authentification que nous avions émises et des systèmes que nous exploitons. Nous travaillons à en corriger les conséquences et renforçons nos mesures de sécurité à la lumière des enseignements tirés.

Notre enquête a permis de reconstituer les faits suivants :

1. **Tout a commencé chez Fireworks.** Un attaquant a obtenu une clé d’accès que nous avions délivrée à Fireworks. Cette clé a été exposée par Fireworks, et l’attaquant s’en est servi pour copier des données depuis un bucket de stockage en lecture seule que nous utilisions pour partager des données avec eux. Ce bucket contenait un nombre limité de traces d’agents de codage, datant pour la plupart de 2025, issues de sessions hors Privacy Mode, que nous étions autorisés à utiliser pour l’entraînement de modèles. Un petit nombre de ces traces contenaient des informations d’authentification qui n’avaient pas été masquées ; moins de 0.01 % des clients ont vu leurs informations d’authentification d’API exposées. À partir de ces informations d’authentification, l’attaquant a pu accéder à certains outils SaaS utilisés par Cursor. Depuis ces outils, il a accédé à certains dépôts de code source archivés de Cursor, hors production (dont certains contenaient des coordonnées professionnelles), à une partie limitée des données d’assistance clientèle et des échanges par e-mail, à des métriques d’utilisation du produit et à des coordonnées, à des résumés de certaines conversations avec l’agent de codage, ainsi qu’à certaines informations d’authentification que des utilisateurs avaient transmises à l’équipe d’assistance de Cursor.
2. **Nos systèmes de surveillance ont détecté l’activité non autorisée.** Dès que nous avons eu connaissance de cette activité, nous avons désactivé la clé du prestataire, lancé une procédure de réponse à incident, puis commencé à révoquer et renouveler les informations d’authentification et à rechercher tout autre accès non autorisé. Chaque nouvel accès identifié a été immédiatement coupé. Par excès de prudence, nous avons temporairement désactivé des services internes et des services du produit bien au-delà de ce que les éléments recueillis exigeaient, ce qui a perturbé temporairement certaines fonctionnalités de Cursor. Nous les avons rétablies une à une dès que cela a pu se faire en toute sécurité.
3. **L’attaquant a publié une extension malveillante.** Le 29 octobre, à l’aide d’informations d’authentification volées dans les données exposées de notre système interne, l’attaquant a placé des copies malveillantes et non autorisées de l’extension Claude Code d’Anthropic directement dans le cache servant à distribuer la Place de marché des extensions VS Code de Cursor. Anthropic n’a pas publié cette copie, et il ne s’agissait pas d’une vulnérabilité du produit d’Anthropic. La source de la place de marché, Open VSX, n’a jamais contenu ce logiciel malveillant. Au maximum 5,000 utilisateurs ayant demandé certaines versions de l’extension mises en cache le 29 octobre ont pu recevoir le build malveillant. Celui-ci est resté disponible environ 11 heures avant que nous ne le supprimions. Nous l’avons retiré, avons mis la Marketplace d’extensions hors ligne et avons collaboré avec le fournisseur cloud qui hébergeait le serveur de l’attaquant pour le faire fermer.
4. **Nous avons informé nos clients.** Le 30 octobre, nous avons publié un avis de sécurité sur notre blog et sur X, comprenant des indicateurs de compromission, des mesures de remédiation et des scripts permettant de vérifier votre système, et avons commencé à informer les utilisateurs et les administrateurs d’équipe concernés via des notifications dans l’application et par e-mail. Lorsque des informations d’authentification ont été exposées, nous avons envoyé par e-mail aux administrateurs d’équipe la liste des clés précises à renouveler, et avons demandé aux services tiers ayant émis ces informations d’authentification de les révoquer. Lorsque des données d’assistance clientèle ou des données professionnelles ont pu être concernées, nous avons informé directement les clients. Si vous avez installé la version compromise Anthropic.claude-code 2.1.312 ou 2.1.313, ou effectué une mise à jour vers celle-ci, depuis la Place de marché des extensions VS Code de Cursor pendant la période d’exposition, veuillez suivre les étapes décrites dans notre avis de sécurité. Désinstaller l’extension ne suffit pas forcément.
5. **Nous estimons que l’incident est maîtrisé. La dernière activité de l’attaquant que nous ayons observée remonte au 1er novembre, et elle a été bloquée. Nous continuons de surveiller nos systèmes afin de détecter toute activité anormale, et de renforcer nos contrôles de sécurité. Si nous identifions de nouvelles informations concernant nos clients, nous les contacterons directement.**

- Vos requêtes et votre code si vous utilisez Privacy Mode.
  - Pour les utilisateurs n'utilisant pas Privacy Mode, des traces d'agent de codage ont été exposées pour **moins de 0,05 %** des utilisateurs. Nous avons identifié ces utilisateurs et les avons tous informés directement. Si vous n'avez pas reçu de notification, vos données ne figuraient pas dans ces traces.
- Pour les utilisateurs n'utilisant pas Privacy Mode, des traces d'agent de codage ont été exposées pour 
- Les données ou le trafic en direct que vous envoyez actuellement à Cursor. Les données de trace exposées se limitaient au stockage utilisé pour l'analyse hors ligne de données historiques.
- Les systèmes de production et l'infrastructure qui font fonctionner Cursor.
- L'application Cursor et ses mises à jour officielles, ainsi que le code que nous livrons au client et à nos services cloud. Nous livrons Cursor à partir d'un système d'hébergement de code distinct auquel l'attaquant n'a jamais eu accès, et aucune clé de signature des versions n'a été exposée.
- Nos autres produits, y compris les Agents Cloud pour les équipes clientes, Bot Grok et tous les autres produits SpaceX SI.
- Les autres extensions de notre Marketplace d'extensions. Nous avons vérifié l'authenticité de toutes les autres extensions, puis examiné et reconstruit le stockage de la place de marché.
- Les requêtes d'inférence en cours. Elles n'ont pas été exposées et, par précaution, nous avons retiré toute l'inférence du prestataire tiers d'entraînement et d'inférence.
- Les plugins téléchargés depuis le Marketplace de Cursor.
- Les dépôts de code des clients.

- Nous avons transféré l’intégralité du trafic d’inférence hors de Fireworks, désactivé la clé que nous lui avions attribuée et révoqué son accès à tous les jeux de données que nous avions partagés avec lui.
- Nous avons renouvelé toutes les informations d’authentification Cursor exposées que nous avons identifiées, en commençant par les plus à risque, et, par précaution, renouvelé toutes les informations d’authentification utilisées dans nos systèmes internes, même en l’absence de toute preuve d’exposition.
- Nous avons vérifié l’authenticité de toutes les autres extensions de notre place de marché, puis examiné et reconstruit notre Marketplace d’extensions VS Code.
- Nous avons renforcé les restrictions d’accès au réseau chez nos prestataires de services, lorsque c’était possible, afin que des informations d’authentification compromises ne puissent pas être utilisées en dehors de leur emplacement prévu.
- Nous avons mis en place l’analyse et la protection des secrets sur tous nos dépôts de code, et étendu cette analyse à d’autres sources de données internes.
- Nous avons redéployé l’ensemble de notre infrastructure et de nos services de production par précaution, et examiné chaque service interne avant de le remettre en service.
- Nous avons fait appel à des experts externes en investigation numérique pour appuyer notre enquête.
- Nous avons contacté toutes les personnes concernées.

Nous présentons nos excuses aux personnes touchées par cet incident : la confidentialité et la sécurité sont pour nous une priorité absolue, et nous pouvons et devons faire mieux. Nous tirons les leçons de cet incident pour rendre Cursor plus sécurisé. En cas de changement important, nous mettrons à jour cette publication.

Des questions ? Écrivez-nous à security@cursor.com.
