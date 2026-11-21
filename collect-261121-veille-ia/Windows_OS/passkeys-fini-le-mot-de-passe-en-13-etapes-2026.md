---
id: collect-261121-veille-ia/windows-os/passkeys-fini-le-mot-de-passe-en-13-etapes-2026
title: "Passkeys : Fini le Mot de Passe en 13 Étapes [2026]"
domain: windows-os
role: reference
task: article
actors: ["Microsoft"]
dates: []
keywords: ["passe", "passkey", "meme", "passkeys", "webauthn", "deux", "partie", "comptes"]
source: https://tech-insider.org/fr/passkeys-fido2-tutoriel-2026/
source_anchor: ""
source_lines: [1, 474]
section: "Windows_OS"
---
Vingt-quatre milliards d’identifiants ont fuité en 2026, une bonne partie encore réutilisable pour se connecter à de vrais comptes, comme tech-insider.org l’a documenté en détail. Chaque nouvelle fuite pointe vers le même coupable : le mot de passe. Il se devine, se phishe, se réutilise et finit tôt ou tard dans une base de données volée. Les passkeys proposent une sortie technique à ce problème, pas une simple rustine. Ce tutoriel vous montre comment les activer sur vos comptes existants, puis comment construire de A à Z un système d’authentification par passkey dans une application Node.js, avec le code complet, les pièges à éviter et les réponses aux pannes les plus fréquentes.

Le sujet dépasse largement le confort de connexion. Pour une équipe technique française, les passkeys touchent à la fois la sécurité des comptes utilisateurs, le coût du support (réinitialisations de mot de passe, tickets liés au phishing) et, de plus en plus, la conformité réglementaire sous NIS2. Ce guide part du geste le plus simple, activer un passkey sur un compte que vous possédez déjà, jusqu’au plus technique, écrire le code serveur qui vérifie une signature WebAuthn. Vous n’avez pas besoin de suivre les deux volets dans l’ordre : un administrateur système peut s’arrêter après la partie configuration, un développeur peut sauter directement à la partie Node.js.

## Qu’est-ce qu’un passkey ? FIDO2 et WebAuthn expliqués simplement

Un passkey est un identifiant cryptographique qui remplace le couple identifiant-mot de passe. Techniquement, il repose sur deux standards complémentaires : **WebAuthn**, l’API du navigateur normalisée par le W3C, et **CTAP2**, le protocole défini par la FIDO Alliance pour parler à un authentificateur (empreinte digitale, Face ID, clé de sécurité). Ensemble, ils forment ce que l’industrie appelle FIDO2. La spécification WebAuthn Level 2 est une recommandation W3C publiée depuis avril 2021, et la version Level 3 poursuit son passage en recommandation candidate au W3C, ce qui montre que le standard continue d’évoluer plutôt que de stagner.

Contrairement à un mot de passe, un passkey génère une paire de clés asymétriques lors de l’inscription. La clé privée ne quitte jamais l’appareil ou le gestionnaire de clés de l’utilisateur. Seule la clé publique part vers le serveur. Un site piraté qui perd sa base de données ne récupère donc que des clés publiques, inutilisables pour se faire passer pour un utilisateur. C’est la différence fondamentale avec un hash de mot de passe, qui reste une cible même chiffré.

Deuxième propriété clé : la liaison à l’origine. Chaque passkey est cryptographiquement associé au domaine exact pour lequel il a été créé. Un faux site de phishing qui imite tech-insider.org sur un domaine différent ne pourra jamais déclencher une signature valide, même si la victime clique sur tout ce qu’on lui présente. Le phishing classique par formulaire devient structurellement impossible, pas seulement moins probable.

La FIDO Alliance, le consortium industriel derrière ces standards, regroupe des acteurs comme Google, Microsoft, Apple et Amazon autour d’un objectif commun : remplacer le mot de passe par de la cryptographie à clé publique pilotée depuis le navigateur ou le système d’exploitation. Le W3C, de son côté, standardise la brique WebAuthn qui permet à n’importe quel site web d’appeler ces authentificateurs depuis JavaScript, sans dépendre d’un plugin propriétaire.

Il faut distinguer deux catégories d’authentificateurs. Un **authentificateur de plateforme** vit directement dans l’appareil : la puce sécurisée d’un iPhone, le module TPM d’un PC Windows, le composant Titan M d’un Pixel. Un **authentificateur itinérant**, à l’inverse, se branche ou se connecte en Bluetooth, comme une clé YubiKey. Les deux parlent le même protocole CTAP2 et produisent le même type de credential côté serveur, ce qui simplifie grandement l’implémentation : votre backend n’a pas besoin de savoir lequel des deux l’utilisateur a en face de lui.

Autre notion utile avant de coder : le **credential découvrable** (discoverable credential, autrefois appelé resident key). Un passkey moderne stocke, en plus de la paire de clés, l’identifiant de l’utilisateur directement dans l’authentificateur. Résultat concret : l’utilisateur peut se connecter en cliquant simplement sur “se connecter avec un passkey” sans avoir d’abord tapé son nom d’utilisateur, puisque l’appareil sait déjà quels comptes il possède pour ce site. C’est ce mécanisme qui alimente l’autofill conditionnel que nous activerons à l’étape 13.

## Pourquoi passer aux passkeys en 2026

Le calcul est simple pour une équipe sécurité ou un particulier attentif. Un mot de passe seul ne résiste à aucune des attaques courantes citées dans les rapports d’incidents que couvre régulièrement ce site, du credential stuffing au phishing en passant par la réutilisation entre services. Ajouter un code SMS ou une application TOTP améliore les choses, mais ces méthodes restent vulnérables à des attaques de relais en temps réel où la victime saisit elle-même le code sur un faux site. Le tableau suivant résume les propriétés structurelles de chaque méthode, telles que définies par les spécifications WebAuthn et FIDO2.

| Méthode d’authentification | Résiste au phishing | Résiste au vol de base de données | Action requise à la connexion | 
|---|---|---|---|
| Mot de passe seul | Non | Non | Frappe manuelle | 
| Mot de passe + SMS OTP | Partiellement | Non (le mot de passe reste exposé) | Frappe + code reçu par SMS | 
| Mot de passe + TOTP (Authenticator) | Partiellement | Non (le mot de passe reste exposé) | Frappe + code à 6 chiffres | 
| Clé de sécurité physique (FIDO2) | Oui | Oui | Toucher la clé | 
| Passkey (plateforme ou synchronisable) | Oui | Oui | Biométrie ou code PIN de l’appareil | 

Cette résistance structurelle explique pourquoi les gestionnaires de mots de passe eux-mêmes intègrent désormais le stockage de passkeys plutôt que de s’y opposer. L’ANSSI, l’agence française chargée de la sécurité des systèmes d’information, pousse depuis plusieurs années vers une authentification forte pour les services sensibles, et CERT-FR documente régulièrement des campagnes de phishing qui contournent les mots de passe classiques et les seconds facteurs faibles. Les passkeys s’inscrivent directement dans cette logique de durcissement, sans demander à l’utilisateur final de retenir quoi que ce soit de nouveau.

Il y a aussi un argument économique, souvent sous-estimé côté produit. Chaque réinitialisation de mot de passe génère un ticket de support, un email de vérification, parfois un appel. Multiplié par une base d’utilisateurs de plusieurs dizaines de milliers de comptes, ce coût opérationnel pèse réellement sur une équipe support. Un passkey ne s’oublie pas de la même façon qu’un mot de passe : il vit dans l’appareil et se réutilise automatiquement, ce qui fait mécaniquement chuter le volume de demandes de réinitialisation une fois l’adoption suffisante. C’est un argument qui convainc souvent plus vite une direction produit que le discours purement sécurité.

Dernier point, moins technique mais tout aussi réel : l’expérience utilisateur. Taper un mot de passe complexe sur un clavier de smartphone reste pénible. Un passkey remplace cette friction par un geste déjà familier, celui qui déverrouille l’appareil plusieurs dizaines de fois par jour. Les équipes produit qui ont testé les deux parcours constatent généralement un taux d’abandon plus faible sur l’écran de connexion lorsque le passkey est proposé en premier, pour la simple raison qu’il retire une étape de friction plutôt que d’en ajouter une.

## Prérequis : versions, outils et compatibilité

Avant de commencer, séparez bien les deux volets de ce tutoriel. Le premier volet, rapide, consiste à activer des passkeys sur des comptes que vous possédez déjà. Il ne demande qu’un smartphone récent et un navigateur à jour. Le second volet, plus technique, construit une authentification par passkey dans votre propre application. Voici ce qu’il vous faut pour le suivre dans de bonnes conditions.

Sur l’exigence HTTPS, une précision évite bien des heures perdues. WebAuthn considère qu’une origine est “sécurisée” soit parce qu’elle utilise HTTPS avec un certificat valide, soit parce qu’il s’agit strictement de `http://localhost`. Un serveur de développement exposé sur l’adresse IP de votre machine locale, du type `http://192.168.1.20:3000`, ne remplit aucune des deux conditions et fera échouer silencieusement l’appel à l’API, souvent sans message d’erreur explicite dans la console. C’est la source de confusion numéro un chez les développeurs qui découvrent WebAuthn, d’où l’intérêt de configurer mkcert dès le départ plutôt que d’y venir seulement après un premier échec.

- **Node.js 20 LTS** ou une version plus récente, avec npm installé
- Un éditeur de code (VS Code ou équivalent)
- La bibliothèque **@simplewebauthn/server** pour le backend, activement maintenue sur GitHub par son auteur Matthew Miller (MasterKale)
- La bibliothèque **@simplewebauthn/browser** pour le frontend
- Un certificat HTTPS local (via **mkcert** ) ou un tunnel type ngrok, car WebAuthn exige une origine sécurisée en dehors de localhost strict
- Un navigateur récent : Chrome, Edge, Firefox ou Safari supportent tous WebAuthn au moment de la rédaction
- Un appareil doté d’un capteur biométrique ou d’un code PIN système (Windows Hello, Touch ID, Face ID, ou verrouillage d’écran Android)

Le tableau ci-dessous récapitule la prise en charge des passkeys selon la plateforme, telle que documentée par les éditeurs eux-mêmes et par le référentiel MDN de l’API Web Authentication.

| Plateforme | Support des passkeys | Stockage / synchronisation | 
|---|---|---|
| Google Chrome (bureau et Android) | Oui | Gestionnaire de mots de passe Google | 
| Apple Safari (macOS et iOS) | Oui | Trousseau iCloud | 
| Microsoft Edge / Windows Hello | Oui | Compte Microsoft ou stockage local lié à l’appareil | 
| Mozilla Firefox | Oui (recours à un gestionnaire tiers ou à l’OS pour la synchronisation) | Dépend du gestionnaire configuré | 
| Gestionnaires tiers (Bitwarden, 1Password) | Oui | Coffre chiffré multiplateforme | 

## Qui a déjà déployé les passkeys en 2026

Les passkeys ne sont plus un concept de laboratoire. Google, Microsoft, Apple, GitHub, Amazon et PayPal proposent tous aujourd’hui la connexion par passkey sur leurs comptes grand public, chacun avec une approche légèrement différente selon son propre écosystème. Ce large soutien industriel compte autant que la technique elle-même : un standard n’a de valeur pratique que si les navigateurs, les systèmes d’exploitation et les grands services web l’implémentent tous de la même façon.

Chaque acteur illustre un cas d’usage différent. Google, avec l’échelle de son parc d’utilisateurs, a fait du passkey l’option mise en avant lors de la création ou de la sécurisation d’un compte, pour réduire mécaniquement le volume d’attaques par bourrage d’identifiants sur son infrastructure. GitHub cible un public de développeurs habitués à manier des clés SSH et des jetons d’accès, pour qui un passkey remplace naturellement un mot de passe de compte devenu la porte d’entrée vers du code source et des secrets de déploiement. PayPal, dans un contexte où chaque connexion touche potentiellement à de l’argent, met en avant la résistance au phishing comme argument de confiance directement dans son parcours de connexion. Microsoft, de son côté, capitalise sur Windows Hello déjà présent sur la majorité des postes professionnels pour convertir un geste de déverrouillage de session en méthode de connexion web à part entière.

Cette diversité de cas d’usage est une bonne nouvelle pour quiconque hésite encore à se lancer. Il n’existe pas un seul “bon” moment ou un seul “bon” secteur pour adopter les passkeys : le standard sert aussi bien un grand public non technique qu’une audience de développeurs, aussi bien un service financier qu’un outil de productivité interne.

## Étapes 1 à 4 : activer les passkeys sur vos comptes Google, Microsoft, Apple et GitHub

Avant de coder quoi que ce soit, prenez cinq minutes pour sécuriser vos comptes personnels. C’est la partie la plus rentable de ce tutoriel pour un lecteur non développeur.

### Étape 1 et 2 : Google et Microsoft

Sur un compte Google, ouvrez la gestion du compte, puis la section consacrée à la sécurité et à la connexion. Vous y trouverez une option pour créer un passkey, qui déclenche immédiatement une demande de vérification biométrique ou de code PIN via votre appareil. Une fois validée, le passkey apparaît dans la liste de vos méthodes de connexion, aux côtés du mot de passe que vous pouvez conserver comme méthode de secours ou supprimer plus tard.

Sur un compte Microsoft, la démarche est similaire depuis les paramètres de sécurité avancés. Windows Hello, s’il est déjà configuré sur votre PC, sert directement d’authentificateur de plateforme. L’intérêt pour une entreprise qui utilise déjà l’écosystème Microsoft est immédiat, puisque le passkey s’appuie sur un mécanisme que les utilisateurs connaissent déjà pour déverrouiller leur session.

Un détail pratique mérite d’être signalé pour ces deux comptes : une fois le passkey créé, ne supprimez pas immédiatement le mot de passe associé. Laissez-le comme filet de sécurité pendant quelques semaines, le temps de confirmer que la connexion par passkey fonctionne sans accroc sur tous vos appareils habituels, y compris après une mise à jour du système d’exploitation ou un changement de navigateur par défaut.

### Étape 3 et 4 : Apple et GitHub

Sur un iPhone ou un Mac, un passkey Apple se crée depuis les réglages du compte iCloud ou directement au moment de la connexion à un site compatible, via Touch ID ou Face ID. Le passkey se synchronise ensuite sur tous les appareils reliés au même identifiant Apple grâce au trousseau iCloud, chiffré de bout en bout.

GitHub, très utilisé par le lectorat technique de ce site, propose l’ajout d’un passkey depuis les paramètres de sécurité du compte, dans la même section que les clés SSH et les jetons d’accès personnels. Pour une équipe de développement, remplacer le mot de passe GitHub par un passkey ferme une porte d’entrée classique vers le code source et les secrets de déploiement.

Pour ces quatre comptes, répétez la même vérification finale : déconnectez-vous complètement, puis reconnectez-vous en choisissant explicitement l’option passkey plutôt que le mot de passe, pour confirmer que le geste fonctionne réellement de bout en bout et pas seulement au moment de la création. Profitez-en aussi pour créer un deuxième passkey depuis un appareil différent lorsque le service le permet, ce qui vous évite de dépendre d’un unique téléphone pour l’accès à des comptes que vous utilisez quotidiennement.

## Étape 5 : choisir et synchroniser votre gestionnaire de passkeys

Un point bloque souvent les nouveaux utilisateurs : où vivent exactement ces passkeys, et que se passe-t-il en cas de changement d’appareil. Deux familles existent. Les passkeys liés à l’appareil (device-bound) restent physiquement sur une clé de sécurité ou une puce sécurisée et ne se copient jamais. Les passkeys synchronisables se répliquent via un écosystème cloud chiffré, que ce soit celui de Google, d’Apple, de Microsoft ou d’un gestionnaire tiers comme Bitwarden.

Pour un usage personnel multiplateforme, un gestionnaire tiers évite de rester enfermé dans un seul écosystème. Si vous hébergez déjà vos identifiants avec Vaultwarden, la version auto-hébergée de Bitwarden, vous pouvez y stocker vos passkeys au même endroit que vos mots de passe restants, avec le même niveau de contrôle sur vos données. C’est l’option la plus cohérente pour quelqu’un qui a déjà investi dans une infrastructure d’identifiants souveraine.

Pour un déploiement en entreprise, la question se pose différemment. Un fournisseur d’identité comme Okta, Entra ID ou Ping Identity centralise la politique d’authentification et impose ou non les passkeys selon le profil de risque de chaque utilisateur. Nous reviendrons sur cette intégration plus loin, une fois le projet de démonstration construit.

Un débat revient régulièrement dans les équipes sécurité au sujet de la portabilité : que se passe-t-il si un utilisateur quitte l’écosystème Apple pour Android, ou change de gestionnaire de mots de passe ? Le standard WebAuthn ne définit pas de mécanisme universel d’export d’un passkey d’un écosystème vers un autre, chaque fournisseur gère sa propre synchronisation en interne. Dans la pratique, cela signifie que l’utilisateur doit recréer un nouveau passkey dans le nouvel écosystème plutôt que de migrer l’ancien. Ce n’est pas idéal, mais ce n’est pas non plus bloquant tant qu’une méthode de secours reste disponible pendant la transition, un point sur lequel nous revenons dans la section consacrée aux erreurs à éviter.

## Étapes 6 à 8 : construire le backend WebAuthn avec Node.js

Place à la partie développeur. L’objectif : un projet minimal mais complet, qui gère l’inscription et la connexion par passkey de bout en bout. Nous utilisons Express pour le serveur et la bibliothèque SimpleWebAuthn, qui encapsule la complexité de l’encodage CBOR et de la vérification des signatures WebAuthn derrière une API JavaScript lisible.

L’architecture générale tient en quatre échanges entre le navigateur et le serveur. Pour l’inscription : le navigateur demande des options d’enregistrement, le serveur répond avec un challenge et des paramètres, le navigateur transmet ces paramètres à l’authentificateur local puis renvoie l’attestation obtenue, et le serveur vérifie cette attestation avant de sauvegarder le nouveau credential. Pour la connexion, le schéma se répète à l’identique en remplaçant l’attestation par une assertion. Écrire cette logique à la main serait long et risqué sur le plan cryptographique, raison pour laquelle nous nous appuyons sur SimpleWebAuthn plutôt que de réimplémenter l’encodage CBOR ou la vérification de signature nous-mêmes.

### Étape 6 : initialiser le projet

```
mkdir passkey-demo && cd passkey-demo
npm init -y
npm install express express-session @simplewebauthn/server
npm install --save-dev nodemon
```
Créez ensuite un module de stockage minimal. Pour ce tutoriel, un stockage en mémoire suffit à démontrer le flux complet. En production, remplacez-le par une vraie base de données (PostgreSQL, MySQL ou SQLite), en conservant la même structure de champs.

### Étape 7 : le modèle de données

```
// db.js : stockage en mémoire pour la démo
// En production, remplacez cette Map par une vraie table utilisateurs/credentials
const users = new Map();
function getUser(username) {
  if (!users.has(username)) {
    users.set(username, {
      id: username,
      devices: [],           // liste des passkeys enregistrés
      currentChallenge: undefined,
    });
  }
  return users.get(username);
}
module.exports = { getUser };
```
### Étape 8 : générer les options d’enregistrement

Le serveur doit d’abord générer un défi cryptographique aléatoire (challenge) et des paramètres d’inscription, que le navigateur transmettra ensuite à l’authentificateur de l’utilisateur.

```
// server.js
const express = require('express');
const session = require('express-session');
const {
  generateRegistrationOptions,
  verifyRegistrationResponse,
  generateAuthenticationOptions,
  verifyAuthenticationResponse,
} = require('@simplewebauthn/server');
const { getUser } = require('./db');
const app = express();
app.use(express.json());
app.use(session({
  secret: process.env.SESSION_SECRET || 'changez-moi-en-production',
  resave: false,
  saveUninitialized: false,
}));
const rpName = 'Mon Application';
const rpID = 'localhost';           // votre domaine réel en production
const origin = `https://${rpID}:3000`;
app.post('/register/options', async (req, res) => {
  const { username } = req.body;
  const user = getUser(username);
  const options = await generateRegistrationOptions({
    rpName,
    rpID,
    userName: username,
    attestationType: 'none',
    excludeCredentials: user.devices.map(d => ({
      id: d.credentialID,
      transports: d.transports,
    })),
    authenticatorSelection: {
      residentKey: 'required',
      userVerification: 'preferred',
    },
  });
  user.currentChallenge = options.challenge;
  res.json(options);
});
```
Notez le paramètre **attestationType: ‘none’**. Il évite de demander une preuve d’origine matérielle de l’authentificateur, une information rarement utile hors contextes réglementés et qui soulève des questions de confidentialité si elle est collectée sans raison. Le paramètre **residentKey: ‘required’** force la création d’une clé découvrable, ce qui permettra plus tard la connexion sans même saisir de nom d’utilisateur.

## Étapes 9 et 10 : la cérémonie d’enregistrement côté client et sa vérification

Côté navigateur, la bibliothèque **@simplewebauthn/browser** encapsule l’appel à `navigator.credentials.create()`. C’est cet appel qui déclenche la fenêtre native de votre système (empreinte, Face ID, code PIN).

```
// public/register.js
import { startRegistration } from '@simplewebauthn/browser';
async function inscrirePasskey(username) {
  const optionsResponse = await fetch('/register/options', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ username }),
  });
  const options = await optionsResponse.json();
  let attestation;
  try {
    attestation = await startRegistration({ optionsJSON: options });
  } catch (error) {
    console.error('Enregistrement annulé ou échoué :', error);
    return;
  }
  const verifyResponse = await fetch('/register/verify', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ username, response: attestation }),
  });
  const { verified } = await verifyResponse.json();
  console.log(verified ? 'Passkey enregistré avec succès' : 'Échec de l’enregistrement');
}
```
Le serveur reçoit ensuite cette réponse et doit la vérifier avant de faire confiance au nouveau passkey. C’est l’étape où **@simplewebauthn/server** valide la signature, le challenge et l’origine.

```
app.post('/register/verify', async (req, res) => {
  const { username, response } = req.body;
  const user = getUser(username);
  try {
    const verification = await verifyRegistrationResponse({
      response,
      expectedChallenge: user.currentChallenge,
      expectedOrigin: origin,
      expectedRPID: rpID,
    });
    const { verified, registrationInfo } = verification;
    if (verified && registrationInfo) {
      user.devices.push({
        credentialID: registrationInfo.credential.id,
        credentialPublicKey: registrationInfo.credential.publicKey,
        counter: registrationInfo.credential.counter,
        transports: response.response.transports,
      });
    }
    res.json({ verified });
  } catch (error) {
    console.error(error);
    res.status(400).json({ error: error.message });
  }
});
```
## Étapes 11 et 12 : la connexion sans mot de passe

L’inscription est terminée. Reste à permettre à l’utilisateur de revenir se connecter sans jamais retaper de mot de passe. Le principe est symétrique : le serveur génère des options d’authentification, le navigateur les transmet à l’authentificateur via `navigator.credentials.get()`, et le serveur vérifie la signature retournée.

```
app.post('/login/options', async (req, res) => {
  const { username } = req.body;
  const user = getUser(username);
  const options = await generateAuthenticationOptions({
    rpID,
    allowCredentials: user.devices.map(d => ({
      id: d.credentialID,
      transports: d.transports,
    })),
    userVerification: 'preferred',
  });
  user.currentChallenge = options.challenge;
  res.json(options);
});
app.post('/login/verify', async (req, res) => {
  const { username, response } = req.body;
  const user = getUser(username);
  const device = user.devices.find(d => d.credentialID === response.id);
  if (!device) {
    return res.status(400).json({ error: 'Passkey inconnu pour cet utilisateur' });
  }
  try {
    const verification = await verifyAuthenticationResponse({
      response,
      expectedChallenge: user.currentChallenge,
      expectedOrigin: origin,
      expectedRPID: rpID,
      credential: {
        id: device.credentialID,
        publicKey: device.credentialPublicKey,
        counter: device.counter,
      },
    });
    if (verification.verified) {
      device.counter = verification.authenticationInfo.newCounter;
      req.session.username = username;
    }
    res.json({ verified: verification.verified });
  } catch (error) {
    console.error(error);
    res.status(400).json({ error: error.message });
  }
});
app.listen(3000, () => console.log('Serveur démarré sur https://localhost:3000'));
```
Le compteur **counter** mérite une explication. Chaque authentificateur incrémente en théorie un compteur à chaque signature. Si le serveur reçoit un jour un compteur inférieur à celui déjà enregistré, cela peut signaler un clonage de l’authentificateur, et la connexion doit être rejetée. Certains authentificateurs de plateforme renvoient systématiquement zéro : la bibliothèque SimpleWebAuthn gère ce cas particulier automatiquement, mais il faut le savoir avant de déboguer un faux problème.

Côté client, la fonction équivalente à l’inscription s’appelle **startAuthentication**.

```
// public/login.js
import { startAuthentication } from '@simplewebauthn/browser';
async function connexionPasskey(username) {
  const optionsResponse = await fetch('/login/options', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ username }),
  });
  const options = await optionsResponse.json();
  const assertion = await startAuthentication({ optionsJSON: options });
  const verifyResponse = await fetch('/login/verify', {
    method: 'POST',
    headers: { 'Content-Type': 'application/json' },
    body: JSON.stringify({ username, response: assertion }),
  });
  const { verified } = await verifyResponse.json();
  if (verified) {
    window.location.href = '/tableau-de-bord';
  }
}
```
## Étape 13 : tester en HTTPS local, déployer et activer l’autofill conditionnel

WebAuthn refuse de fonctionner sur une origine HTTP non sécurisée, à l’exception stricte de localhost. Dès que vous testez sur un autre nom d’hôte ou que vous préparez un déploiement, générez un certificat local avec mkcert.

```
# Installe une autorité de certification locale de confiance
mkcert -install
# Génère un certificat valable pour localhost et ses variantes
mkcert localhost 127.0.0.1 ::1
# Démarrez ensuite votre serveur en HTTPS avec les fichiers générés
# (adaptez server.js pour charger localhost-key.pem et localhost.pem)
node server-https.js
```
Une fois le certificat en place, un test manuel complet ressemble à ceci. Ouvrez la page d’inscription, saisissez un nom d’utilisateur, cliquez sur le bouton de création de passkey. Le navigateur affiche sa fenêtre native, vous demande votre empreinte ou votre code PIN, puis la console affiche la confirmation suivante :

```
$ curl -X POST https://localhost:3000/register/verify \
  -H "Content-Type: application/json" \
  -d '{"username":"denis","response":{"id":"...","rawId":"...","type":"public-key"}}'
{"verified":true}
```
Dernier raffinement, particulièrement apprécié des utilisateurs : l’autofill conditionnel. Il permet au navigateur de suggérer directement un passkey disponible dès que l’utilisateur clique dans le champ de connexion, sans bouton dédié. Deux conditions techniques suffisent : ajouter l’attribut `autocomplete="username webauthn"` sur le champ de saisie, et déclencher l’appel avec l’option **useBrowserAutofill**.

```
import { startAuthentication, browserSupportsWebAuthnAutofill } from '@simplewebauthn/browser';
if (await browserSupportsWebAuthnAutofill()) {
  startAuthentication({ optionsJSON: options, useBrowserAutofill: true })
    .then(handleAuthenticationResponse);
}
```
Une dernière brique manque pour que le projet soit vraiment utilisable : la possibilité, pour l’utilisateur, de révoquer un passkey perdu ou compromis sans devoir contacter le support. Ajoutez une route dédiée qui retire le credential de la liste des appareils enregistrés.

```
app.delete('/devices/:credentialID', requireAuth, (req, res) => {
  const user = getUser(req.session.username);
  const before = user.devices.length;
  user.devices = user.devices.filter(
    d => d.credentialID !== req.params.credentialID
  );
  if (user.devices.length === before) {
    return res.status(404).json({ error: 'Passkey introuvable' });
  }
  res.json({ removed: true, remaining: user.devices.length });
});
// Middleware minimal : exige une session déjà authentifiée
function requireAuth(req, res, next) {
  if (!req.session.username) {
    return res.status(401).json({ error: 'Non authentifié' });
  }
  next();
}
```
Affichez cette liste d’appareils dans un écran de paramètres, avec la date d’ajout et, si possible, un nom lisible par l’utilisateur (“iPhone de Denis”, “PC bureau”). C’est ce détail d’interface, plus que le code cryptographique lui-même, qui détermine si vos utilisateurs feront réellement confiance au système le jour où ils perdent un appareil.

Votre projet de démonstration est désormais complet : inscription, stockage, connexion, gestion du compteur anti-clonage, révocation et autofill conditionnel. Il reste minimal volontairement, pour que chaque brique reste lisible, mais il couvre exactement le flux qu’utilisent en production Google, Microsoft ou GitHub.

## Intégrer les passkeys avec Keycloak et Authelia en entreprise

Coder son propre backend WebAuthn a un intérêt pédagogique réel, mais une entreprise gère rarement l’authentification application par application. Elle centralise ce travail dans un fournisseur d’identité. Si vous avez suivi notre tutoriel Keycloak, vous savez déjà que cette plateforme open source gère nativement WebAuthn comme méthode de connexion, avec une politique configurable par royaume (realm) : passkey obligatoire, optionnel, ou réservé à certains groupes d’utilisateurs.

Authelia propose une approche complémentaire pour qui protège déjà un reverse proxy avec du 2FA auto-hébergé : elle accepte WebAuthn comme second facteur ou, selon la configuration, comme facteur unique suffisant. Dans les deux cas, le principe reste identique à celui que vous venez de coder à la main, sauf que le fournisseur d’identité absorbe toute la complexité de génération des challenges et de vérification des signatures. Le choix entre coder sa propre couche WebAuthn ou s’appuyer sur un IAM dépend surtout du nombre d’applications à couvrir. En dessous de trois ou quatre services internes, une implémentation directe comme celle de ce tutoriel reste raisonnable. Au-delà, un fournisseur d’identité centralisé évite de dupliquer la logique de sécurité partout.

Concrètement, la décision se résume souvent à trois questions. Combien d’applications internes doivent partager la même politique d’authentification ? Votre équipe a-t-elle déjà une brique SSO en place, ou faut-il la construire en même temps que le support passkey ? Et surtout, qui doit pouvoir forcer ou désactiver le passkey pour un groupe d’utilisateurs donné, un administrateur IAM ou chaque équipe applicative séparément ? Une startup avec une poignée de services internes gagne du temps avec l’implémentation directe présentée dans ce tutoriel. Une organisation soumise à NIS2, avec des dizaines d’applications et des exigences d’audit, tire davantage de valeur d’un IAM centralisé qui journalise chaque authentification à un seul endroit.

## 5 erreurs courantes à éviter avec les passkeys

La plupart des implémentations WebAuthn ratées ne viennent pas d’une faille cryptographique, le protocole est solide par construction, mais de décisions de conception prises trop vite. Voici les erreurs qui reviennent le plus souvent dans les retours d’expérience d’équipes ayant déployé les passkeys en production.

- **Ne prévoir aucune méthode de récupération.** Un utilisateur qui perd son seul appareil et n’a synchronisé aucun passkey ailleurs se retrouve bloqué. Prévoyez toujours un second facteur alternatif ou des codes de secours à usage unique, générés et affichés une seule fois au moment de l’inscription.
- **Confondre passkey synchronisable et passkey lié à l’appareil dans l’interface.** Les utilisateurs doivent comprendre si leur passkey survit au remplacement de leur téléphone ou non, sans quoi la confusion génère des tickets de support inutiles. Un simple badge “synchronisé” ou “lié à cet appareil” dans l’écran de gestion des passkeys suffit à lever l’ambiguïté.
- **Fixer attestationType sur ‘direct’ sans raison précise.** Cette option complexifie l’implémentation, ralentit l’inscription et soulève des questions de confidentialité, pour un bénéfice réel que seuls certains secteurs réglementés exigent, comme la finance ou l’accès à des systèmes gouvernementaux.
- **Changer de domaine sans anticiper l’impact.** Le rpID est gravé dans chaque passkey créé. Migrer de app.exemple.fr vers exemple.fr casse tous les passkeys existants et force une réinscription générale. Si un changement de domaine est prévu, planifiez une période de transition avec les deux méthodes actives en parallèle.
- **Retirer le mot de passe trop tôt.** Tant qu’une partie des utilisateurs n’a pas adopté les passkeys, coupez la corde de rappel progressivement, service par service, plutôt que d’un coup pour tout le monde.
- **Sous-dimensionner les tests multi-navigateurs.** Un flux validé uniquement sur Chrome desktop peut se comporter différemment sur Safari mobile, notamment sur la gestion de l’autofill conditionnel. Testez systématiquement sur au moins un navigateur mobile et un navigateur desktop avant chaque mise en production.

## Dépannage : 8 problèmes fréquents et leurs solutions

Voici les erreurs les plus signalées lors de l’implémentation de WebAuthn, avec la cause réelle derrière chaque message. Gardez cette liste ouverte dans un onglet pendant vos premiers tests, la plupart des blocages viennent d’une des huit causes ci-dessous.

- **“SecurityError: The relying party ID is not a registrable domain suffix”** : le rpID déclaré côté serveur ne correspond pas exactement au domaine affiché dans la barre d’adresse. Vérifiez qu’il n’inclut ni protocole, ni port, ni sous-domaine incorrect.
- **“Origin does not match” lors de la vérification** : expectedOrigin doit reprendre le protocole complet (https://) et le port exact si celui-ci n’est pas standard. Une simple différence entre 3000 et 443 suffit à faire échouer la vérification.
- **WebAuthn refuse de se déclencher en HTTP** : en dehors de localhost strict, une origine sécurisée HTTPS est obligatoire. Utilisez mkcert en développement.
- **Un passkey créé sur un appareil n’apparaît pas sur un autre** : vérifiez que le compte cloud (iCloud, Google, Microsoft) est bien connecté et synchronisé sur les deux appareils, et que la synchronisation du trousseau est activée dans les réglages système.
- **“NotAllowedError: The operation either timed out or was not allowed”** : l’utilisateur a annulé la demande biométrique, le délai par défaut a expiré, ou l’appel n’a pas été déclenché directement par une interaction utilisateur (clic, tap).
- **L’autofill conditionnel n’apparaît jamais dans le champ de connexion** : vérifiez la présence de l’attribut autocomplete=”username webauthn” sur le champ, et confirmez via browserSupportsWebAuthnAutofill() que le navigateur teste supporte réellement cette fonctionnalité.
- **Erreur CORS entre le frontend et le backend** : si les deux tournent sur des ports ou domaines différents en développement, configurez credentials: ‘include’ côté fetch et les en-têtes CORS appropriés côté serveur.
- **“Unexpected registration response challenge”** : le challenge stocké en session a expiré ou a été écrasé par une requête concurrente. Vérifiez la durée de vie de votre session et évitez de partager le même challenge entre deux onglets ouverts simultanément.

## Passkeys vs clés de sécurité physiques vs TOTP : que choisir ?

Ces trois méthodes ne s’excluent pas forcément. Beaucoup d’organisations les combinent selon le profil de l’utilisateur et la criticité du compte protégé, plutôt que d’en imposer une seule à l’ensemble des effectifs.

La confusion la plus fréquente concerne la résistance au phishing d’une application TOTP. Le code à six chiffres généré par Google Authenticator ou une application équivalente reste, sur le papier, un facteur “que vous possédez”. Le problème, c’est qu’il n’est lié à aucune origine web. Un attaquant qui présente un faux formulaire de connexion peut relayer ce code en temps réel vers le vrai site, en profitant de la fenêtre de validité de quelques dizaines de secondes. C’est exactement le type d’attaque que documentent régulièrement les rapports d’incidents sur le phishing avancé. Un passkey échappe structurellement à ce scénario, puisque la signature produite par l’authentificateur est mathématiquement liée au domaine réel et ne peut pas être rejouée ailleurs, même par un relais en temps réel.

| Critère | Passkey (plateforme) | Clé de sécurité physique | Application TOTP | 
|---|---|---|---|
| Résistance au phishing | Élevée | Élevée | Faible à moyenne | 
| Coût | Gratuit (intégré à l’appareil) | Achat matériel requis | Gratuit | 
| Portabilité entre appareils | Selon synchronisation | Totale, tant que la clé est sur soi | Manuelle (export/QR code) | 
| Résilience si appareil perdu | Bonne si synchronisé dans le cloud | Nécessite une clé de secours | Nécessite les codes de récupération | 
| Cas d’usage recommandé | Grand public, usage quotidien multi-appareils | Comptes à très haut privilège, administrateurs systèmes | Services ne supportant pas encore WebAuthn | 

Pour un compte administrateur ou un accès à de l’infrastructure critique, une architecture Zero Trust combine souvent un passkey lié à l’appareil avec une clé de sécurité physique dédiée aux opérations les plus sensibles, en plus d’une vérification contextuelle (localisation, appareil connu, horaires habituels).

## Conseils avancés pour un déploiement en production

Une fois le flux de base validé, quelques ajustements séparent une démonstration d’un système prêt pour de vrais utilisateurs.

Stockez le challenge côté serveur, jamais dans un cookie visible côté client, et donnez-lui une durée de vie courte, de l’ordre de deux à cinq minutes. Journalisez chaque tentative d’enregistrement et de connexion avec l’horodatage, l’adresse IP et le résultat, ces journaux deviennent la première source d’investigation en cas d’incident. Proposez systématiquement un second passkey ou une clé de secours dès l’inscription plutôt que d’attendre que l’utilisateur en fasse la demande après avoir déjà perdu l’accès à son compte.

Ajoutez une limite de fréquence (rate limiting) sur les routes d’options d’enregistrement et de connexion. Rien n’empêche techniquement un script d’appeler `/login/options` en boucle pour un nom d’utilisateur donné, même si l’attaque ne peut pas aboutir sans l’authentificateur physique de la victime. Limiter ces routes à quelques appels par minute et par adresse IP évite malgré tout de transformer votre serveur en cible facile pour du bruit inutile ou une tentative d’énumération de comptes existants.

Surveillez également le taux d’échec de vérification dans le temps. Un pic soudain d’échecs sur `/register/verify` ou `/login/verify` signale souvent un problème de configuration après un déploiement, comme un rpID ou un origin mal mis à jour, plutôt qu’une attaque. Un tableau de bord simple qui trace ce taux d’échec heure par heure suffit à détecter ce genre de régression avant qu’elle n’atteigne tous vos utilisateurs.

Pensez aussi à la migration progressive plutôt qu’au basculement brutal. Affichez le passkey comme option recommandée à côté du mot de passe existant pendant plusieurs semaines, mesurez le taux d’adoption réel, puis seulement ensuite programmez la suppression du mot de passe pour les comptes qui ont confirmé leur passkey. Cette approche progressive réduit considérablement le volume de tickets de support au moment du changement. Enfin, documentez clairement pour votre équipe support les trois ou quatre scénarios de perte d’accès les plus probables (appareil perdu, changement d’écosystème, désinstallation accidentelle du gestionnaire de mots de passe) avec, pour chacun, la procédure exacte à suivre. Un support qui improvise face à un utilisateur bloqué crée plus de friction que le mot de passe qu’il était censé remplacer.

## Passkeys, RGPD et conformité en France

Du point de vue du RGPD, un passkey traite en réalité moins de données personnelles qu’un système de mot de passe classique. Aucun secret partagé n’est stocké côté serveur, seule une clé publique inutilisable en dehors de son contexte cryptographique. En choisissant **attestationType: ‘none’** comme recommandé plus haut, vous évitez même de collecter des informations sur le matériel exact de l’utilisateur, ce qui simplifie l’analyse d’impact relative à la protection des données pour ce traitement spécifique.

Sur le terrain réglementaire, la directive NIS2 pousse un nombre croissant d’entités françaises vers une authentification forte pour l’accès aux systèmes sensibles, un sujet que nous avons détaillé dans notre couverture de l’actualité cybersécurité du site. L’ANSSI recommande depuis longtemps l’authentification multifacteur pour les comptes à privilèges, et les passkeys remplissent cette exigence tout en supprimant le facteur de risque humain lié à la saisie d’un mot de passe sur un site frauduleux. En cas d’incident malgré tout, CERT-FR reste le point de contact national pour le signalement, joignable par courriel à [email protected] ou par téléphone au 3218, un service gratuit accessible depuis la France. Au niveau européen, l’ENISA documente également les bonnes pratiques d’authentification forte pour l’ensemble des États membres.

Pour une DSI qui prépare un dossier de conformité, gardez une trace écrite de trois éléments : la nature exacte des données collectées lors de l’inscription d’un passkey (avec attestationType à ‘none’, il s’agit essentiellement d’une clé publique et d’un identifiant de credential, rien de plus), la durée de conservation de ces données, et la procédure de suppression lorsqu’un utilisateur supprime son compte ou révoque un passkey. Cette documentation, courte à rédiger puisque le traitement est volontairement minimal, facilite grandement les échanges avec un délégué à la protection des données ou, le cas échéant, avec la CNIL.

## Foire aux questions

**Qu’est-ce qu’un passkey exactement ?**

Un passkey est une paire de clés cryptographiques qui remplace le couple identifiant-mot de passe. La clé privée reste sur votre appareil ou votre gestionnaire de mots de passe, la clé publique seule est envoyée au site auquel vous vous connectez.

**Les passkeys remplacent-elles complètement les mots de passe ?**

Pas du jour au lendemain. La plupart des services proposent le passkey en option à côté du mot de passe existant, puis encouragent progressivement la bascule complète une fois l’adoption suffisante.

**Que se passe-t-il si je perds mon téléphone contenant mes passkeys ?**

Si vos passkeys sont synchronisés via iCloud, Google ou un gestionnaire tiers, vous les récupérez en vous reconnectant à ce même compte cloud sur un nouvel appareil, après avoir vérifié votre identité par les moyens habituels de récupération de ce compte cloud. S’ils étaient liés uniquement à l’appareil perdu, vous devez utiliser votre méthode de récupération de secours définie au moment de l’inscription, ce qui souligne l’importance de ne jamais sauter cette étape lors de la configuration initiale.

**Les passkeys sont-elles vraiment plus sûres que les mots de passe ?**

Oui, structurellement. Elles suppriment la réutilisation de mot de passe, la vulnérabilité au phishing par formulaire et le risque lié au vol d’une base de données de hashs, trois causes majeures des compromissions de comptes documentées dans les rapports d’incidents. Aucune sécurité n’est absolue, mais ces trois vecteurs d’attaque comptent parmi les plus exploités au quotidien, ce qui rend leur suppression particulièrement significative.

**Puis-je utiliser un passkey créé sur iPhone pour me connecter depuis un PC Windows ?**

Oui, via un mécanisme appelé hybrid transport : le PC affiche un QR code, vous le scannez avec votre iPhone, qui confirme la connexion via Bluetooth de proximité sans jamais transmettre la clé privée elle-même.

**Les passkeys sont-elles compatibles avec le RGPD ?**

Oui. Elles traitent en réalité moins de données personnelles qu’un système de mot de passe classique, puisque aucun secret partagé n’est stocké côté serveur.

**Quelle est la différence entre un passkey et une clé de sécurité physique type YubiKey ?**

Les deux reposent sur le même standard FIDO2/WebAuthn. Une clé physique matérialise le secret sur un objet dédié que vous transportez, alors qu’un passkey plateforme vit dans votre téléphone ou votre ordinateur, éventuellement synchronisé dans le cloud.

**WebAuthn fonctionne-t-il sans connexion internet ?**

La cérémonie cryptographique locale (déverrouillage biométrique, signature) fonctionne hors ligne, mais l’échange avec le serveur pour vérifier cette signature nécessite bien sûr une connexion active au service concerné.

Vous disposez maintenant des deux moitiés du sujet : la partie utilisateur, pour verrouiller vos comptes personnels en quelques minutes, et la partie développeur, avec un projet Node.js fonctionnel que vous pouvez étendre ou brancher sur votre application existante. Les passkeys ne demandent ni budget matériel ni changement d’infrastructure lourd, seulement une bibliothèque bien choisie et une attention particulière aux quelques pièges décrits plus haut. Dans un contexte où les fuites d’identifiants continuent de s’accumuler, c’est l’un des rares changements techniques qui améliore simultanément la sécurité et le confort d’usage, plutôt que d’imposer l’un au détriment de l’autre.
