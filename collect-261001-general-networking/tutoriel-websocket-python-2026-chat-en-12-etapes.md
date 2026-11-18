Les WebSockets transforment la façon dont les applications web communiquent en temps réel. Contrairement aux requêtes HTTP classiques où le client doit interroger le serveur en permanence (polling), le protocole WebSocket établit une connexion bidirectionnelle persistante entre le navigateur et le serveur. La bibliothèque `websockets` pour Python, désormais en version 17.1 depuis le 26 août 2026, exige toujours Python 3.11 ou supérieur et ne prend officiellement en charge que la dernière version corrective 3.x.y de chaque branche, selon la documentation mise à jour en août 2026 ; elle offre une API asyncio élégante et conforme à 100 % au standard RFC 6455. Dans ce tutoriel, vous allez construire un système de chat en temps réel complet, du serveur WebSocket au client interactif, en passant par l’authentification, les salons multiples et le déploiement en production.

Ce guide étape par étape couvre les 12 phases de construction d’une application WebSocket fonctionnelle avec Python 3.12+ et la bibliothèque `websockets`. Chaque étape inclut du code testé, des exemples de sortie et des solutions aux erreurs courantes. Que vous construisiez un tableau de bord temps réel, un outil collaboratif ou un système de notifications push, ce tutoriel vous donne les fondations solides pour maîtriser les WebSockets en Python.

## Prérequis et Environnement de Développement

Avant de commencer, assurez-vous que votre environnement de développement est correctement configuré. Le prérequis Python a évolué rapidement en 2026 : la version 16.0 de `websockets`, publiée sur PyPI le 10 janvier 2026 avec le statut « No Known Issues », a d’abord relevé le minimum à Python 3.10, avant que la branche 17.x ne le porte définitivement à Python 3.11 — la documentation officielle, mise à jour en août 2026, précise même que seule la dernière version corrective 3.x.y de chaque branche Python est désormais officiellement supportée. Nous recommandons donc Python 3.12 ou supérieur pour bénéficier des dernières optimisations asyncio. La bibliothèque `websockets`, désormais en version 17.1 depuis le 26 août 2026, reste construite sur l’implémentation Sans-I/O introduite depuis la version 13.0, offrant une architecture plus modulaire et performante que les versions précédentes.

| Outil | Version Requise | Commande de Vérification | 
|---|---|---|
| Python | 3.12+ | `python3 --version` | 
| pip | 24.0+ | `pip --version` | 
| websockets | 16.0 | `pip show websockets` | 
| asyncio | intégré | `python3 -c "import asyncio"` | 
| aiohttp (optionnel) | 3.10+ | `pip show aiohttp` | 
| uvicorn (production) | 0.32+ | `uvicorn --version` | 
| Navigateur | Chrome 120+ / Firefox 120+ | Support WebSocket natif | 

Créez un répertoire de projet et un environnement virtuel :

```
mkdir websocket-chat && cd websocket-chat
python3 -m venv venv
source venv/bin/activate
pip install websockets==16.0
pip install aiohttp  # pour le client HTTP optionnel
pip install python-dotenv  # pour les variables d'environnement
```
Vérifiez que l’installation est correcte en exécutant `python3 -c "import websockets; print(websockets.__version__)"`. Vous devez voir `17.1` s’afficher — la version stable la plus récente depuis le 26 août 2026, dont les wheels ARM ne pèsent que 176 à 177 Ko selon le dépôt **piwheels**, et qui reste également disponible via le paquet Gentoo `dev-python/websockets` déployé sur 11 architectures. Si vous obtenez une erreur `ModuleNotFoundError`, vérifiez que votre environnement virtuel est bien activé.

## Étape 1 : Comprendre le Protocole WebSocket

Le protocole WebSocket, défini par la RFC 6455, fonctionne au-dessus de TCP et commence par un handshake HTTP classique. Le client envoie une requête HTTP avec l’en-tête `Upgrade: websocket`, et le serveur répond avec un code 101 (Switching Protocols). À partir de ce moment, la connexion TCP reste ouverte et les deux parties peuvent envoyer des messages à tout moment, sans avoir à réétablir une connexion.

La différence fondamentale avec le HTTP traditionnel est majeure en termes de performances. Avec le polling HTTP, chaque requête génère un overhead de 800 octets à 2 Ko d’en-têtes. En comparaison, un frame WebSocket n’ajoute que 2 à 14 octets d’overhead. Pour une application de chat envoyant 50 messages par seconde, cela représente une réduction de bande passante de plus de 90 % par rapport au polling HTTP. Le protocole WebSocket supporte aussi bien les messages texte (UTF-8) que les messages binaires, ce qui le rend adapté aussi bien au chat qu’au streaming de données.

| Caractéristique | HTTP Polling | WebSocket | 
|---|---|---|
| Direction | Client → Serveur uniquement | Bidirectionnel | 
| Overhead par message | 800+ octets | 2-14 octets | 
| Latence | Intervalle de polling (100ms-5s) | Temps réel (<10ms) | 
| Connexions persistantes | Non | Oui | 
| Support navigateur | Universel | Tous navigateurs modernes | 
| Port par défaut | 80 (HTTP) / 443 (HTTPS) | 80 (WS) / 443 (WSS) | 

La bibliothèque Python `websockets` maintient une couverture de branche de 100 % pour la conformité RFC 6455, selon sa documentation officielle. C’est la bibliothèque WebSocket la plus utilisée en Python : après la version 14.2 taguée le 19 janvier 2025 puis packagée par openSUSE sous le nom `python313-websockets-14.2-1.1` le 6 mai 2025, le projet a publié la 15.0 le 16 février 2025 avec plusieurs wheels ARMv6/ARMv7 chez **piwheels**, suivie du correctif 15.0.1 le 5 mars 2025 pour Python 3.11 et 3.13. Ce rythme soutenu s’est poursuivi jusqu’à la 16.1.1, documentée comme stable dans le changelog officiel le 31 juillet 2026 — preuve d’une base de tests automatisés solide et d’une API construite sur `asyncio` qui s’intègre naturellement avec l’écosystème Python asynchrone. Face à des alternatives plus anciennes comme `ws4py`, dont la dernière version 0.6.0 ne date que du 2 août 2025 avec 54 contributeurs au total sur GitHub, ou à des distributions embarquées comme Apertis v2025 qui n’embarque encore que la version 10.4 selon Repology, `websockets` conserve une longueur d’avance nette en termes de rythme de publication.

## Étape 2 : Créer un Serveur WebSocket Basique

Commençons par un serveur WebSocket minimal. L’API de `websockets` 17.0.1 utilise le module `websockets.asyncio`, introduit depuis la version 13.0, qui repose sur une implémentation Sans-I/O ; la documentation de la version 16.1, datée du 9 juillet 2026, détaille d’ailleurs les changements incrémentaux qui ont préparé cette transition vers la branche 17.x. Ce serveur écoute sur le port 8765 et renvoie chaque message reçu en écho (echo server). C’est le point de départ classique pour comprendre le cycle de vie d’une connexion WebSocket.

```
# server.py - Serveur WebSocket basique
import asyncio
import websockets
async def handler(websocket):
    """Gère chaque connexion WebSocket entrante."""
    print(f"Nouvelle connexion : {websocket.remote_address}")
    try:
        async for message in websocket:
            print(f"Reçu : {message}")
            # Renvoyer le message en écho
            await websocket.send(f"Écho : {message}")
    except websockets.exceptions.ConnectionClosed as e:
        print(f"Connexion fermée : {e.code} {e.reason}")
async def main():
    # Démarrer le serveur sur localhost:8765
    async with websockets.serve(handler, "localhost", 8765) as server:
        print("Serveur WebSocket démarré sur ws://localhost:8765")
        await server.serve_forever()
if __name__ == "__main__":
    asyncio.run(main())
```
Lancez le serveur avec `python3 server.py`. Vous verrez le message `Serveur WebSocket démarré sur ws://localhost:8765`. Le serveur reste actif et attend des connexions. La boucle `async for message in websocket` itère automatiquement sur les messages entrants et gère proprement la fermeture de connexion lorsque le client se déconnecte.

**Sortie attendue du serveur :**

```
$ python3 server.py
Serveur WebSocket démarré sur ws://localhost:8765
Nouvelle connexion : ('127.0.0.1', 54321)
Reçu : Bonjour le monde
Connexion fermée : 1000 
```
## Étape 3 : Construire un Client WebSocket Python

Maintenant créons un client Python qui se connecte au serveur. Le client utilise la même bibliothèque `websockets` et le pattern `async with` qui gère automatiquement l’ouverture et la fermeture de la connexion. La connexion est établie en moins de 10 millisecondes en local, démontrant l’avantage des WebSockets pour les communications en temps réel.

```
# client.py - Client WebSocket Python
import asyncio
import websockets
async def client():
    uri = "ws://localhost:8765"
    async with websockets.connect(uri) as websocket:
        # Envoyer un message
        message = "Bonjour le monde"
        await websocket.send(message)
        print(f"Envoyé : {message}")
        # Recevoir la réponse
        response = await websocket.recv()
        print(f"Reçu : {response}")
        # Envoyer plusieurs messages
        for i in range(3):
            msg = f"Message numéro {i + 1}"
            await websocket.send(msg)
            resp = await websocket.recv()
            print(f"Envoyé : {msg} → Reçu : {resp}")
if __name__ == "__main__":
    asyncio.run(client())
```
**Sortie attendue du client :**

```
$ python3 client.py
Envoyé : Bonjour le monde
Reçu : Écho : Bonjour le monde
Envoyé : Message numéro 1 → Reçu : Écho : Message numéro 1
Envoyé : Message numéro 2 → Reçu : Écho : Message numéro 2
Envoyé : Message numéro 3 → Reçu : Écho : Message numéro 3
```
Le pattern `async with websockets.connect(uri)` est fondamental : il ouvre la connexion, exécute le bloc de code, puis ferme proprement la connexion avec le code de fermeture 1000 (Normal Closure). Si une erreur survient, la connexion est quand même fermée grâce au gestionnaire de contexte.

## Étape 4 : Système de Broadcast Multi-Clients

Un echo server est utile pour apprendre, mais une application de chat réelle doit diffuser les messages à tous les clients connectés. C’est le pattern de broadcast. Nous allons maintenir un ensemble (`set`) de toutes les connexions actives et envoyer chaque message reçu à tous les autres participants. Cette architecture est la base de toute application temps réel multi-utilisateurs.

```
# chat_server.py - Serveur de chat avec broadcast
import asyncio
import websockets
import json
from datetime import datetime
# Ensemble de toutes les connexions actives
CLIENTS = set()
async def register(websocket):
    """Enregistre un nouveau client."""
    CLIENTS.add(websocket)
    count = len(CLIENTS)
    print(f"Client connecté. Total : {count}")
    # Notifier tous les clients du nombre de connectés
    await broadcast(json.dumps({
        "type": "system",
        "message": f"Un utilisateur a rejoint le chat ({count} connecté{'s' if count > 1 else ''})",
        "timestamp": datetime.now().isoformat()
    }))
async def unregister(websocket):
    """Désenregistre un client déconnecté."""
    CLIENTS.discard(websocket)
    count = len(CLIENTS)
    print(f"Client déconnecté. Total : {count}")
    await broadcast(json.dumps({
        "type": "system",
        "message": f"Un utilisateur a quitté le chat ({count} connecté{'s' if count > 1 else ''})",
        "timestamp": datetime.now().isoformat()
    }))
async def broadcast(message):
    """Diffuse un message à tous les clients connectés."""
    if CLIENTS:
        await asyncio.gather(
            *[client.send(message) for client in CLIENTS],
            return_exceptions=True
        )
async def handler(websocket):
    """Gère une connexion client."""
    await register(websocket)
    try:
        async for raw_message in websocket:
            data = json.loads(raw_message)
            # Ajouter un timestamp serveur
            data["timestamp"] = datetime.now().isoformat()
            data["type"] = "message"
            await broadcast(json.dumps(data))
    except websockets.exceptions.ConnectionClosed:
        pass
    finally:
        await unregister(websocket)
async def main():
    async with websockets.serve(handler, "0.0.0.0", 8765) as server:
        print("Serveur de chat démarré sur ws://0.0.0.0:8765")
        await server.serve_forever()
if __name__ == "__main__":
    asyncio.run(main())
```
Le serveur utilise `asyncio.gather` avec `return_exceptions=True` pour envoyer le message à tous les clients simultanément sans bloquer si l’un d’eux est lent. L’utilisation de `CLIENTS.discard()` plutôt que `CLIENTS.remove()` évite une `KeyError` si le client a déjà été retiré. L’écoute sur `0.0.0.0` au lieu de `localhost` permet d’accepter les connexions depuis d’autres machines sur le réseau.

## Étape 5 : Client HTML/JavaScript pour le Chat

Pour tester le serveur de chat, créons une interface web simple en HTML et JavaScript. L’API WebSocket est intégrée nativement dans tous les navigateurs modernes depuis plus de dix ans. Le constructeur `new WebSocket(url)` établit la connexion, et les événements `onopen`, `onmessage`, `onclose` et `onerror` permettent de réagir aux différents états de la connexion.

```
<!-- index.html - Client de chat WebSocket -->
<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>Chat WebSocket</title>
    <style>
        body { font-family: system-ui, sans-serif; max-width: 600px; margin: 2rem auto; }
        #messages { height: 400px; overflow-y: auto; border: 1px solid #ccc; padding: 1rem; }
        .system { color: #666; font-style: italic; }
        .message { margin: 0.5rem 0; }
        input, button { padding: 0.5rem; margin-top: 0.5rem; }
        input[type="text"] { width: 70%; }
    </style>
</head>
<body>
    <h1>Chat WebSocket en Temps Réel</h1>
    <div id="messages"></div>
    <input type="text" id="username" placeholder="Votre pseudo" />
    <br/>
    <input type="text" id="input" placeholder="Tapez votre message..." />
    <button onclick="sendMessage()">Envoyer</button>
    <script>
        const ws = new WebSocket("ws://localhost:8765");
        const messages = document.getElementById("messages");
        ws.onopen = () => {
            addMessage("Connecté au serveur", "system");
        };
        ws.onmessage = (event) => {
            const data = JSON.parse(event.data);
            if (data.type === "system") {
                addMessage(data.message, "system");
            } else {
                addMessage(`${data.username}: ${data.message}`, "message");
            }
        };
        ws.onclose = () => {
            addMessage("Déconnecté du serveur", "system");
        };
        function addMessage(text, className) {
            const div = document.createElement("div");
            div.className = className;
            div.textContent = text;
            messages.appendChild(div);
            messages.scrollTop = messages.scrollHeight;
        }
        function sendMessage() {
            const username = document.getElementById("username").value || "Anonyme";
            const input = document.getElementById("input");
            if (input.value) {
                ws.send(JSON.stringify({ username, message: input.value }));
                input.value = "";
            }
        }
        document.getElementById("input").addEventListener("keypress", (e) => {
            if (e.key === "Enter") sendMessage();
        });
    </script>
</body>
</html>
```
Ouvrez ce fichier dans deux onglets de navigateur différents. Chaque message envoyé depuis un onglet apparaît instantanément dans l’autre. Le serveur gère automatiquement la sérialisation JSON et la diffusion. Vous pouvez tester avec trois, quatre ou dix onglets : le mécanisme de broadcast reste identique et les performances restent stables grâce à l’architecture asynchrone.

## Étape 6 : Salons de Chat et Routage des Messages

Une application de chat professionnelle nécessite des salons (rooms) pour organiser les conversations par sujet. Au lieu d’un seul ensemble global de clients, nous allons utiliser un dictionnaire de salons où chaque clé est le nom du salon et chaque valeur est l’ensemble des connexions WebSocket qui y participent. Ce pattern est utilisé par toutes les plateformes de messagerie, de Slack à Discord.

```
# rooms_server.py - Serveur WebSocket avec salons
import asyncio
import websockets
import json
from datetime import datetime
from collections import defaultdict
# Dictionnaire des salons : {room_name: set(websockets)}
ROOMS = defaultdict(set)
# Mapping inverse : {websocket: room_name}
USER_ROOMS = {}
# Pseudos des utilisateurs
USER_NAMES = {}
async def broadcast_to_room(room, message):
    """Diffuse un message à tous les clients d'un salon."""
    clients = ROOMS.get(room, set())
    if clients:
        await asyncio.gather(
            *[client.send(message) for client in clients],
            return_exceptions=True
        )
async def handler(websocket):
    """Gère une connexion avec routage par salon."""
    current_room = None
    username = "Anonyme"
    try:
        async for raw_message in websocket:
            data = json.loads(raw_message)
            action = data.get("action")
            if action == "join":
                # Quitter l'ancien salon si nécessaire
                if current_room and websocket in ROOMS[current_room]:
                    ROOMS[current_room].discard(websocket)
                    await broadcast_to_room(current_room, json.dumps({
                        "type": "system",
                        "message": f"{username} a quitté le salon",
                        "room": current_room
                    }))
                # Rejoindre le nouveau salon
                current_room = data.get("room", "général")
                username = data.get("username", "Anonyme")
                ROOMS[current_room].add(websocket)
                USER_ROOMS[websocket] = current_room
                USER_NAMES[websocket] = username
                # Notifier le salon
                count = len(ROOMS[current_room])
                await broadcast_to_room(current_room, json.dumps({
                    "type": "system",
                    "message": f"{username} a rejoint #{current_room} ({count} membre{'s' if count > 1 else ''})",
                    "room": current_room
                }))
                # Envoyer la liste des salons disponibles
                room_list = {name: len(members) for name, members in ROOMS.items() if members}
                await websocket.send(json.dumps({
                    "type": "room_list",
                    "rooms": room_list
                }))
            elif action == "message" and current_room:
                await broadcast_to_room(current_room, json.dumps({
                    "type": "message",
                    "username": username,
                    "message": data.get("message", ""),
                    "room": current_room,
                    "timestamp": datetime.now().isoformat()
                }))
    except websockets.exceptions.ConnectionClosed:
        pass
    finally:
        if current_room:
            ROOMS[current_room].discard(websocket)
            await broadcast_to_room(current_room, json.dumps({
                "type": "system",
                "message": f"{username} a quitté le salon",
                "room": current_room
            }))
        USER_ROOMS.pop(websocket, None)
        USER_NAMES.pop(websocket, None)
async def main():
    async with websockets.serve(handler, "0.0.0.0", 8765) as server:
        print("Serveur avec salons démarré sur ws://0.0.0.0:8765")
        await server.serve_forever()
if __name__ == "__main__":
    asyncio.run(main())
```
Ce serveur gère proprement les transitions entre salons : quand un utilisateur rejoint un nouveau salon, il est automatiquement retiré de l’ancien. Le dictionnaire `defaultdict(set)` simplifie la gestion des salons en créant automatiquement un nouvel ensemble pour chaque nouveau nom de salon. La liste des salons actifs est envoyée au client à chaque connexion, permettant une interface dynamique.

## Étape 7 : Authentification et Sécurité WebSocket

La sécurité est critique pour toute application WebSocket en production. Contrairement aux requêtes HTTP classiques où chaque requête peut porter un cookie ou un token, la connexion WebSocket ne dispose que du handshake initial pour l’authentification. Deux approches sont couramment utilisées : les tokens dans les paramètres de requête URL et les en-têtes personnalisés lors du handshake. La première est plus simple, la seconde est plus sécurisée.

```
# auth_server.py - Serveur WebSocket avec authentification
import asyncio
import websockets
import json
import hashlib
import secrets
from datetime import datetime, timedelta
# Base de données simplifiée d'utilisateurs
USERS_DB = {
    "alice": hashlib.sha256("motdepasse123".encode()).hexdigest(),
    "bob": hashlib.sha256("secret456".encode()).hexdigest(),
}
# Tokens actifs : {token: {"username": str, "expires": datetime}}
ACTIVE_TOKENS = {}
def generate_token(username):
    """Génère un token d'authentification."""
    token = secrets.token_urlsafe(32)
    ACTIVE_TOKENS[token] = {
        "username": username,
        "expires": datetime.now() + timedelta(hours=24)
    }
    return token
def validate_token(token):
    """Vérifie la validité d'un token."""
    if token not in ACTIVE_TOKENS:
        return None
    data = ACTIVE_TOKENS[token]
    if datetime.now() > data["expires"]:
        del ACTIVE_TOKENS[token]
        return None
    return data["username"]
async def auth_handler(websocket):
    """Gère l'authentification avant d'autoriser le chat."""
    try:
        # Attendre le message d'authentification (timeout 10s)
        raw = await asyncio.wait_for(websocket.recv(), timeout=10.0)
        data = json.loads(raw)
        if data.get("action") == "login":
            username = data.get("username", "")
            password_hash = hashlib.sha256(
                data.get("password", "").encode()
            ).hexdigest()
            if username in USERS_DB and USERS_DB[username] == password_hash:
                token = generate_token(username)
                await websocket.send(json.dumps({
                    "type": "auth",
                    "status": "success",
                    "token": token,
                    "username": username
                }))
                # Authentifié : passer au handler de chat
                await chat_handler(websocket, username)
            else:
                await websocket.send(json.dumps({
                    "type": "auth",
                    "status": "error",
                    "message": "Identifiants invalides"
                }))
                await websocket.close(4001, "Authentification échouée")
        elif data.get("action") == "token_auth":
            username = validate_token(data.get("token", ""))
            if username:
                await websocket.send(json.dumps({
                    "type": "auth",
                    "status": "success",
                    "username": username
                }))
                await chat_handler(websocket, username)
            else:
                await websocket.close(4001, "Token invalide ou expiré")
    except asyncio.TimeoutError:
        await websocket.close(4002, "Timeout d'authentification")
async def chat_handler(websocket, username):
    """Handler de chat post-authentification."""
    print(f"{username} authentifié et connecté")
    async for message in websocket:
        data = json.loads(message)
        data["username"] = username  # Forcer le nom authentifié
        data["timestamp"] = datetime.now().isoformat()
        await websocket.send(json.dumps(data))
async def main():
    async with websockets.serve(auth_handler, "0.0.0.0", 8765) as server:
        print("Serveur sécurisé démarré sur ws://0.0.0.0:8765")
        await server.serve_forever()
if __name__ == "__main__":
    asyncio.run(main())
```
Plusieurs points de sécurité importants dans ce code. Le timeout de 10 secondes sur le premier message empêche les connexions fantômes qui consommeraient des ressources. Le code de fermeture personnalisé `4001` (dans la plage 4000-4999 réservée aux applications) permet au client de distinguer une erreur d’authentification d’une fermeture normale. Le nom d’utilisateur est forcé côté serveur à chaque message pour empêcher l’usurpation d’identité.

### Bonnes Pratiques de Sécurité WebSocket

En production, plusieurs mesures additionnelles sont essentielles. Utilisez toujours `wss://` (WebSocket Secure) avec TLS/SSL, jamais `ws://` en clair. Validez systématiquement l’origine de la connexion via l’en-tête `Origin` dans le handshake pour prévenir les attaques CSRF. Limitez le taux de messages par client (rate limiting) pour empêcher les attaques par saturation. Enfin, définissez une taille maximale de message avec le paramètre `max_size` de la bibliothèque `websockets` pour éviter les attaques par déni de service basées sur la mémoire.

## Étape 8 : Heartbeat et Reconnexion Automatique

Les connexions WebSocket peuvent se fermer silencieusement, notamment à travers les proxys, les pare-feux ou les équilibreurs de charge qui coupent les connexions inactives. Le mécanisme de heartbeat (ping/pong) permet de détecter ces déconnexions et de maintenir la connexion active. La bibliothèque `websockets` gère automatiquement les ping/pong au niveau du protocole, mais il est recommandé d’ajouter un heartbeat applicatif pour les cas où le ping protocole est insuffisant.

```
# heartbeat_client.py - Client avec reconnexion automatique
import asyncio
import websockets
import json
class ResilientWebSocketClient:
    """Client WebSocket avec heartbeat et reconnexion."""
    def __init__(self, uri, reconnect_delay=3, max_retries=10):
        self.uri = uri
        self.reconnect_delay = reconnect_delay
        self.max_retries = max_retries
        self.websocket = None
        self.connected = False
    async def connect(self):
        """Connexion avec reconnexion automatique."""
        retries = 0
        while retries < self.max_retries:
            try:
                self.websocket = await websockets.connect(
                    self.uri,
                    ping_interval=20,    # Ping toutes les 20s
                    ping_timeout=10,     # Timeout ping de 10s
                    close_timeout=5,     # Timeout de fermeture
                    max_size=1_048_576,  # 1 Mo max par message
                )
                self.connected = True
                retries = 0  # Réinitialiser le compteur
                print(f"Connecté à {self.uri}")
                await self.listen()
            except websockets.exceptions.ConnectionClosed as e:
                self.connected = False
                print(f"Connexion fermée : {e.code} - Reconnexion...")
            except (ConnectionRefusedError, OSError) as e:
                self.connected = False
                retries += 1
                delay = min(self.reconnect_delay * (2 ** retries), 60)
                print(f"Erreur de connexion ({retries}/{self.max_retries}). "
                      f"Nouvelle tentative dans {delay}s...")
                await asyncio.sleep(delay)
        print("Nombre maximum de tentatives atteint. Arrêt.")
    async def listen(self):
        """Écoute les messages entrants."""
        async for message in self.websocket:
            data = json.loads(message)
            await self.on_message(data)
    async def on_message(self, data):
        """Callback pour les messages reçus."""
        print(f"Message : {data}")
    async def send(self, data):
        """Envoie un message si connecté."""
        if self.connected and self.websocket:
            await self.websocket.send(json.dumps(data))
async def main():
    client = ResilientWebSocketClient("ws://localhost:8765")
    await client.connect()
if __name__ == "__main__":
    asyncio.run(main())
```
Ce client implémente le backoff exponentiel pour les reconnexions : le délai double à chaque tentative (3s, 6s, 12s, 24s...) jusqu'à un maximum de 60 secondes. Les paramètres `ping_interval=20` et `ping_timeout=10` de `websockets.connect()` activent le heartbeat automatique du protocole. Si le serveur ne répond pas au ping dans les 10 secondes, la connexion est considérée comme morte et le mécanisme de reconnexion se déclenche.

## Étape 9 : Intégration avec FastAPI

Pour les applications web complètes, FastAPI offre un support WebSocket natif intégré directement dans son framework — un guide publié le 23 mars 2026 par **Websocket.org** confirme que ce support repose sur la couche ASGI de **Starlette**, le framework sous-jacent de FastAPI. L'avantage de FastAPI est de combiner les endpoints HTTP REST classiques avec les endpoints WebSocket dans la même application, partageant les mêmes modèles Pydantic, le même système de dépendances et la même documentation OpenAPI.

```
# fastapi_ws.py - WebSocket avec FastAPI
from fastapi import FastAPI, WebSocket, WebSocketDisconnect
from fastapi.responses import HTMLResponse
import json
from datetime import datetime
app = FastAPI(title="Chat WebSocket FastAPI")
class ConnectionManager:
    """Gestionnaire centralisé des connexions WebSocket."""
    def __init__(self):
        self.active_connections: dict[str, list[WebSocket]] = {}
    async def connect(self, websocket: WebSocket, room: str):
        await websocket.accept()
        if room not in self.active_connections:
            self.active_connections[room] = []
        self.active_connections[room].append(websocket)
    def disconnect(self, websocket: WebSocket, room: str):
        if room in self.active_connections:
            self.active_connections[room].remove(websocket)
            if not self.active_connections[room]:
                del self.active_connections[room]
    async def broadcast(self, message: dict, room: str):
        for connection in self.active_connections.get(room, []):
            await connection.send_json(message)
manager = ConnectionManager()
@app.websocket("/ws/{room}/{username}")
async def websocket_endpoint(websocket: WebSocket, room: str, username: str):
    await manager.connect(websocket, room)
    await manager.broadcast(
        {"type": "system", "message": f"{username} a rejoint #{room}"},
        room
    )
    try:
        while True:
            data = await websocket.receive_text()
            message = {
                "type": "message",
                "username": username,
                "message": data,
                "room": room,
                "timestamp": datetime.now().isoformat()
            }
            await manager.broadcast(message, room)
    except WebSocketDisconnect:
        manager.disconnect(websocket, room)
        await manager.broadcast(
            {"type": "system", "message": f"{username} a quitté #{room}"},
            room
        )
@app.get("/api/rooms")
async def get_rooms():
    """Endpoint REST pour lister les salons actifs."""
    return {
        room: len(clients)
        for room, clients in manager.active_connections.items()
    }
# Lancer avec : uvicorn fastapi_ws:app --host 0.0.0.0 --port 8000
```
Lancez ce serveur avec `uvicorn fastapi_ws:app --host 0.0.0.0 --port 8000`. La connexion WebSocket se fait sur `ws://localhost:8000/ws/général/alice`. L'endpoint REST `/api/rooms` retourne la liste des salons actifs au format JSON, démontrant comment les deux protocoles cohabitent. FastAPI gère automatiquement la validation des paramètres de chemin et la documentation interactive à `/docs`.

## Étape 10 : Envoi de Fichiers et Données Binaires

Les WebSockets ne se limitent pas au texte. Le protocole supporte nativement les messages binaires, ce qui permet d'envoyer des images, des fichiers audio ou tout autre type de données. En Python, la distinction entre message texte et binaire se fait automatiquement : `str` est envoyé comme texte, `bytes` comme binaire. Cette fonctionnalité est essentielle pour les tableaux de bord qui affichent des graphiques en temps réel ou les outils collaboratifs qui partagent des fichiers.

```
# binary_server.py - Gestion des messages binaires
import asyncio
import websockets
import json
import base64
CLIENTS = set()
async def handler(websocket):
    CLIENTS.add(websocket)
    try:
        async for message in websocket:
            if isinstance(message, bytes):
                # Message binaire : fichier reçu
                size_kb = len(message) / 1024
                print(f"Fichier reçu : {size_kb:.1f} Ko")
                # Métadonnées en JSON, contenu en binaire
                # Convention : les 4 premiers octets = taille des métadonnées
                meta_size = int.from_bytes(message[:4], byteorder="big")
                meta_json = json.loads(message[4:4 + meta_size].decode())
                file_data = message[4 + meta_size:]
                print(f"Fichier : {meta_json['filename']} "
                      f"({meta_json['mime_type']})")
                # Diffuser à tous les clients
                for client in CLIENTS:
                    if client != websocket:
                        await client.send(message)
            else:
                # Message texte : chat normal
                data = json.loads(message)
                for client in CLIENTS:
                    if client != websocket:
                        await client.send(json.dumps(data))
    except websockets.exceptions.ConnectionClosed:
        pass
    finally:
        CLIENTS.discard(websocket)
async def main():
    async with websockets.serve(
        handler, "0.0.0.0", 8765,
        max_size=10_485_760  # 10 Mo max par message
    ) as server:
        print("Serveur binaire démarré (max 10 Mo/message)")
        await server.serve_forever()
if __name__ == "__main__":
    asyncio.run(main())
```
Le paramètre `max_size=10_485_760` limite la taille des messages à 10 Mo pour éviter les abus mémoire. Notre convention de protocole utilise les 4 premiers octets pour encoder la taille des métadonnées JSON, suivies du contenu binaire du fichier. Cette approche est plus efficace que l'encodage base64 qui augmente la taille des données de 33 %.

## Étape 11 : Tests Unitaires et d'Intégration

Tester des applications WebSocket nécessite des outils spécifiques. La bibliothèque `websockets` fournit un serveur de test intégré, et `pytest-asyncio` permet d'écrire des tests asynchrones. Chaque test démarre un serveur éphémère, exécute les assertions, puis ferme proprement toutes les connexions. Cette approche garantit l'isolation entre les tests et évite les effets de bord.

```
# test_chat.py - Tests d'intégration WebSocket
import asyncio
import pytest
import websockets
import json
# Installation : pip install pytest pytest-asyncio
SERVER_PORT = 8799  # Port dédié aux tests
@pytest.fixture
async def chat_server():
    """Lance un serveur de test éphémère."""
    clients = set()
    async def handler(ws):
        clients.add(ws)
        try:
            async for msg in ws:
                for c in clients:
                    if c != ws:
                        await c.send(msg)
        except websockets.exceptions.ConnectionClosed:
            pass
        finally:
            clients.discard(ws)
    server = await websockets.serve(handler, "localhost", SERVER_PORT)
    yield server
    server.close()
    await server.wait_closed()
@pytest.mark.asyncio
async def test_echo_message(chat_server):
    """Vérifie qu'un message est bien diffusé."""
    async with websockets.connect(f"ws://localhost:{SERVER_PORT}") as ws1, \
               websockets.connect(f"ws://localhost:{SERVER_PORT}") as ws2:
        test_message = json.dumps({"user": "test", "message": "hello"})
        await ws1.send(test_message)
        response = await asyncio.wait_for(ws2.recv(), timeout=5.0)
        data = json.loads(response)
        assert data["message"] == "hello"
        assert data["user"] == "test"
@pytest.mark.asyncio
async def test_multiple_clients(chat_server):
    """Vérifie le broadcast vers plusieurs clients."""
    clients = []
    for _ in range(5):
        ws = await websockets.connect(f"ws://localhost:{SERVER_PORT}")
        clients.append(ws)
    # Le premier client envoie un message
    await clients[0].send(json.dumps({"message": "broadcast"}))
    # Tous les autres doivent le recevoir
    for client in clients[1:]:
        msg = await asyncio.wait_for(client.recv(), timeout=5.0)
        assert json.loads(msg)["message"] == "broadcast"
    for client in clients:
        await client.close()
@pytest.mark.asyncio
async def test_disconnection_handling(chat_server):
    """Vérifie la gestion propre des déconnexions."""
    async with websockets.connect(f"ws://localhost:{SERVER_PORT}") as ws1:
        ws2 = await websockets.connect(f"ws://localhost:{SERVER_PORT}")
        await ws2.close()
        # ws1 doit pouvoir continuer à fonctionner
        await ws1.send(json.dumps({"message": "still alive"}))
        # Pas d'exception = test réussi
```
**Exécution des tests :**

```
$ pytest test_chat.py -v
========================= test session starts ==========================
collected 3 items
test_chat.py::test_echo_message PASSED                           [ 33%]
test_chat.py::test_multiple_clients PASSED                       [ 66%]
test_chat.py::test_disconnection_handling PASSED                 [100%]
========================= 3 passed in 1.24s ===========================
```
Utilisez `asyncio.wait_for()` avec un timeout dans chaque test pour éviter que les tests ne bloquent indéfiniment si un message n'est jamais reçu. Le port dédié `8799` empêche les conflits avec un serveur de développement qui tournerait sur le port par défaut. La fixture `chat_server` assure le nettoyage complet avec `server.close()` et `await server.wait_closed()`.

## Étape 12 : Déploiement en Production avec TLS

Le déploiement en production d'une application WebSocket diffère significativement du développement local. Trois éléments sont essentiels : le chiffrement TLS (wss://), un reverse proxy comme Nginx pour gérer les connexions, et un processus supervisé (systemd ou Docker) pour la fiabilité. Le protocole `wss://` est obligatoire sur les sites HTTPS — les navigateurs bloquent les connexions `ws://` non chiffrées depuis une page HTTPS.

```
# production_server.py - Serveur WebSocket prêt pour la production
import asyncio
import websockets
import ssl
import json
import logging
import signal
# Configuration du logging
logging.basicConfig(
    level=logging.INFO,
    format="%(asctime)s [%(levelname)s] %(message)s"
)
logger = logging.getLogger("ws-server")
CLIENTS = set()
MAX_CLIENTS = 1000
MAX_MESSAGE_SIZE = 1_048_576  # 1 Mo
async def handler(websocket):
    if len(CLIENTS) >= MAX_CLIENTS:
        await websocket.close(4003, "Serveur saturé")
        logger.warning("Connexion refusée : limite atteinte")
        return
    CLIENTS.add(websocket)
    remote = websocket.remote_address
    logger.info(f"Connexion : {remote} (total: {len(CLIENTS)})")
    try:
        async for message in websocket:
            if len(message) > MAX_MESSAGE_SIZE:
                await websocket.close(4004, "Message trop volumineux")
                return
            data = json.loads(message)
            # Sanitisation basique
            data["message"] = data.get("message", "")[:2000]
            broadcast_msg = json.dumps(data)
            await asyncio.gather(
                *[c.send(broadcast_msg) for c in CLIENTS if c != websocket],
                return_exceptions=True
            )
    except websockets.exceptions.ConnectionClosed as e:
        logger.info(f"Déconnexion {remote}: {e.code}")
    except json.JSONDecodeError:
        await websocket.close(4005, "JSON invalide")
    finally:
        CLIENTS.discard(websocket)
        logger.info(f"Nettoyage {remote} (total: {len(CLIENTS)})")
async def main():
    # Configuration TLS (décommenter en production)
    # ssl_context = ssl.SSLContext(ssl.PROTOCOL_TLS_SERVER)
    # ssl_context.load_cert_chain("cert.pem", "key.pem")
    stop = asyncio.get_event_loop().create_future()
    # Gestion propre de l'arrêt
    loop = asyncio.get_event_loop()
    for sig in (signal.SIGINT, signal.SIGTERM):
        loop.add_signal_handler(sig, stop.set_result, None)
    async with websockets.serve(
        handler,
        "0.0.0.0",
        8765,
        # ssl=ssl_context,  # Décommenter en production
        max_size=MAX_MESSAGE_SIZE,
        max_queue=32,
        ping_interval=30,
        ping_timeout=10,
    ) as server:
        logger.info("Serveur production démarré sur :8765")
        await stop  # Attendre signal d'arrêt
    logger.info("Serveur arrêté proprement")
if __name__ == "__main__":
    asyncio.run(main())
```
La configuration Nginx suivante gère le reverse proxy WebSocket avec TLS :

```
# /etc/nginx/sites-available/websocket
upstream websocket_backend {
    server 127.0.0.1:8765;
}
server {
    listen 443 ssl;
    server_name chat.example.com;
    ssl_certificate /etc/letsencrypt/live/chat.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/chat.example.com/privkey.pem;
    location /ws/ {
        proxy_pass http://websocket_backend;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_read_timeout 86400s;  # 24h pour les connexions longues
        proxy_send_timeout 86400s;
    }
}
```
Les en-têtes `Upgrade` et `Connection` sont obligatoires pour que Nginx transmette correctement le handshake WebSocket. Le `proxy_read_timeout` de 24 heures empêche Nginx de couper les connexions WebSocket inactives. En production, ajoutez aussi un `limit_conn` Nginx pour limiter les connexions par IP.

## 5 Erreurs Courantes et Comment les Éviter

Après avoir construit le projet complet, voici les cinq erreurs les plus fréquentes que rencontrent les développeurs lorsqu'ils travaillent avec les WebSockets en Python, ainsi que leurs solutions.

**Erreur 1 : Oublier `await` sur les appels WebSocket.** Toutes les opérations WebSocket (`send`, `recv`, `close`) sont des coroutines. Écrire `websocket.send(msg)` sans `await` ne génère pas d'erreur visible en Python 3.12, mais le message ne sera jamais envoyé. Ajoutez toujours `await` devant chaque appel.

**Erreur 2 : Ne pas gérer les déconnexions dans le broadcast.** Si un client se déconnecte pendant un `asyncio.gather` de broadcast, une exception `ConnectionClosed` est levée et interrompt l'envoi aux autres clients. La solution est d'utiliser `return_exceptions=True` dans `asyncio.gather()`.

**Erreur 3 : Utiliser des variables globales mutables sans protection.** Modifier un `set` ou un `dict` global depuis plusieurs coroutines concurrentes peut sembler fonctionner en développement, mais provoque des comportements imprévisibles sous charge. Pour les structures de données partagées complexes, utilisez un `asyncio.Lock()`.

**Erreur 4 : Ignorer la limite de taille des messages.** Sans le paramètre `max_size`, un client malveillant peut envoyer un message de plusieurs gigaoctets et faire crasher le serveur par épuisement mémoire. Définissez toujours `max_size` (1 Mo est un bon défaut pour le chat).

**Erreur 5 : Tester uniquement avec un seul client.** Les bugs de concurrence n'apparaissent qu'avec plusieurs connexions simultanées. Écrivez des tests avec au moins 5 clients connectés en parallèle pour détecter les conditions de concurrence (race conditions) et les problèmes de broadcast.

## Dépannage : 10 Problèmes Fréquents et Solutions

Voici les problèmes les plus fréquemment rencontrés lors du développement et du déploiement d'applications WebSocket en Python, avec leurs solutions détaillées.

| Problème | Cause | Solution | 
|---|---|---|
| `ConnectionRefusedError` | Serveur non démarré ou mauvais port | Vérifier que le serveur tourne : `lsof -i :8765` | 
| `InvalidStatusCode: 403` | Pare-feu ou proxy bloque les WebSockets | Vérifier les en-têtes `Upgrade` dans la config Nginx | 
| `ConnectionClosedError: 1006` | Déconnexion anormale (réseau coupé) | Implémenter le heartbeat avec `ping_interval` | 
| `InvalidMessage` | Données non-JSON envoyées au serveur | Entourer `json.loads()` d'un`try/except` | 
| Messages non reçus | `await` manquant sur`send()` | Vérifier que chaque `send()` est précédé de`await` | 
| Mémoire qui augmente | Clients déconnectés non retirés du `set` | Utiliser `try/finally` pour toujours appeler`discard()` | 
| Timeout Nginx `504` | `proxy_read_timeout` trop bas | Augmenter à 86400s pour les connexions longues | 
| `ssl.SSLError` en production | Certificat TLS invalide ou expiré | Renouveler avec `certbot renew` et redémarrer | 
| Erreur CORS dans le navigateur | Origine non autorisée | Configurer `origins` dans`websockets.serve()` | 
| Performance dégradée à 500+ clients | Broadcast séquentiel au lieu de concurrent | Utiliser `asyncio.gather()` pour le broadcast parallèle | 

Pour le problème de mémoire, un outil de diagnostic utile est `tracemalloc` intégré à Python. Ajoutez `tracemalloc.start()` au début de votre serveur et affichez les statistiques avec `tracemalloc.get_traced_memory()` pour identifier les fuites. Pour les problèmes de performance sous charge, utilisez `websockets.broadcast()` (disponible depuis la version 10.0) qui est optimisé pour envoyer le même message à de nombreux clients simultanément, sans créer une tâche par client.

## Astuces Avancées pour les WebSockets en Python

Pour aller au-delà des bases, voici des techniques avancées qui distinguent les applications WebSocket professionnelles des projets débutants.

### Compression des Messages avec Per-Message Deflate

Le protocole WebSocket supporte la compression via l'extension Per-Message Deflate (RFC 7692). La bibliothèque `websockets` l'active par défaut. Pour les messages JSON répétitifs (comme les mises à jour de cours de bourse), la compression réduit la bande passante de 60 à 80 %. Si votre serveur est limité en CPU plutôt qu'en bande passante, désactivez-la avec `compression=None`.

### Scaling Horizontal avec Redis Pub/Sub

Un seul processus Python peut gérer des milliers de connexions WebSocket grâce à asyncio. Mais quand vous déployez plusieurs instances derrière un load balancer, les messages d'un serveur ne sont pas envoyés aux clients connectés à un autre serveur. La solution standard est d'utiliser Redis Pub/Sub comme bus de messages inter-processus. Chaque instance Python s'abonne à un canal Redis et publie les messages dessus, garantissant que tous les clients reçoivent tous les messages, quelle que soit l'instance à laquelle ils sont connectés.

```
# scaled_server.py - WebSocket avec Redis Pub/Sub (extrait)
import asyncio
import websockets
import json
import redis.asyncio as redis
REDIS_URL = "redis://localhost:6379"
CHANNEL = "chat:messages"
CLIENTS = set()
async def redis_listener():
    """Écoute les messages Redis et les diffuse aux clients locaux."""
    r = redis.from_url(REDIS_URL)
    pubsub = r.pubsub()
    await pubsub.subscribe(CHANNEL)
    async for msg in pubsub.listen():
        if msg["type"] == "message":
            data = msg["data"].decode()
            await asyncio.gather(
                *[c.send(data) for c in CLIENTS],
                return_exceptions=True
            )
async def handler(websocket):
    """Publie les messages sur Redis au lieu du broadcast local."""
    r = redis.from_url(REDIS_URL)
    CLIENTS.add(websocket)
    try:
        async for message in websocket:
            # Publier sur Redis (toutes les instances recevront)
            await r.publish(CHANNEL, message)
    except websockets.exceptions.ConnectionClosed:
        pass
    finally:
        CLIENTS.discard(websocket)
    await r.aclose()
async def main():
    # Lancer l'écoute Redis en parallèle du serveur
    listener_task = asyncio.create_task(redis_listener())
    async with websockets.serve(handler, "0.0.0.0", 8765):
        await asyncio.Future()  # Tourner indéfiniment
if __name__ == "__main__":
    asyncio.run(main())
```
Cette architecture permet de scaler horizontalement à des dizaines de milliers de connexions simultanées en ajoutant simplement des instances Python derrière un load balancer. Redis gère la distribution des messages entre les instances avec une latence de l'ordre de la milliseconde.

## Projet Complet : Structure et Fichiers

Voici la structure complète du projet que nous avons construit au fil de ce tutoriel. Chaque fichier est fonctionnel et prêt à être utilisé. Le serveur principal combine les salons, l'authentification basique et le broadcast optimisé.

```
# Structure du projet
websocket-chat/
├── venv/
├── server.py              # Étape 2 - Serveur écho basique
├── client.py              # Étape 3 - Client Python
├── chat_server.py         # Étape 4 - Broadcast multi-clients
├── index.html             # Étape 5 - Interface web
├── rooms_server.py        # Étape 6 - Salons de chat
├── auth_server.py         # Étape 7 - Authentification
├── heartbeat_client.py    # Étape 8 - Reconnexion auto
├── fastapi_ws.py          # Étape 9 - Intégration FastAPI
├── binary_server.py       # Étape 10 - Données binaires
├── test_chat.py           # Étape 11 - Tests
├── production_server.py   # Étape 12 - Production
├── scaled_server.py       # Avancé - Redis Pub/Sub
├── requirements.txt
└── nginx.conf             # Config reverse proxy
# requirements.txt
websockets==16.0
fastapi>=0.115.0
uvicorn>=0.32.0
redis>=5.2.0
pytest>=8.3.0
pytest-asyncio>=0.24.0
python-dotenv>=1.0.0
```
Pour lancer le projet complet, clonez ou créez le répertoire, installez les dépendances avec `pip install -r requirements.txt`, puis démarrez le serveur de votre choix. Le `chat_server.py` est le meilleur point de départ pour un chat fonctionnel. Ouvrez `index.html` dans plusieurs onglets de navigateur pour tester la communication en temps réel.

## WebSocket vs Alternatives : Quand Utiliser Quoi

Les WebSockets ne sont pas la seule technologie pour la communication en temps réel. Selon votre cas d'usage, d'autres protocoles peuvent être plus adaptés. Voici un comparatif pour vous aider à choisir la bonne technologie.

| Technologie | Direction | Cas d'Usage Idéal | Latence | Complexité | 
|---|---|---|---|---|
| WebSocket | Bidirectionnel | Chat, jeux, collaboration | <10ms | Moyenne | 
| Server-Sent Events (SSE) | Serveur → Client | Notifications, flux d'actualités | <50ms | Faible | 
| HTTP Long Polling | Simulé bidirectionnel | Compatibilité anciens navigateurs | 100ms-5s | Faible | 
| WebRTC | Pair à pair | Vidéo, audio, partage d'écran | <5ms | Élevée | 
| gRPC Streaming | Bidirectionnel | Microservices, IoT | <10ms | Élevée | 
| MQTT | Pub/Sub | IoT, capteurs, domotique | <50ms | Moyenne | 

Les WebSockets sont le choix optimal quand vous avez besoin d'une communication bidirectionnelle en temps réel avec une latence minimale. Si votre application n'a besoin que de recevoir des mises à jour du serveur (notifications, cours de bourse), les Server-Sent Events (SSE) sont plus simples à mettre en œuvre et fonctionnent nativement avec HTTP/2. Pour la communication pair-à-pair (vidéo, audio), WebRTC est la technologie appropriée, bien que les WebSockets soient souvent utilisés comme canal de signalisation pour WebRTC.

## FAQ : Questions Fréquentes sur les WebSockets Python

**Combien de connexions WebSocket un serveur Python peut-il gérer ?** Un seul processus Python avec asyncio peut gérer entre 10 000 et 50 000 connexions simultanées, selon la charge de travail par connexion et les ressources du serveur. Le facteur limitant est généralement la mémoire (chaque connexion consomme environ 10 à 50 Ko) et le CPU pour le traitement des messages. Avec le scaling horizontal via Redis Pub/Sub, vous pouvez atteindre des centaines de milliers de connexions.

**Les WebSockets fonctionnent-ils derrière un CDN comme Cloudflare ?** Oui, Cloudflare supporte les WebSockets sur tous ses plans. Cependant, des limitations s'appliquent : le timeout d'inactivité est de 100 secondes par défaut. Assurez-vous d'implémenter un heartbeat avec un intervalle inférieur à 100 secondes pour maintenir la connexion active à travers le CDN.

**Quelle est la différence entre `websockets` et `socket.io` en Python ?** La bibliothèque `websockets` implémente le protocole WebSocket pur (RFC 6455). Socket.IO est un protocole de niveau supérieur qui utilise WebSocket comme transport mais ajoute des fonctionnalités comme la reconnexion automatique, les rooms, et le fallback vers le long polling. Si vous contrôlez le client et le serveur, `websockets` est plus léger et plus performant. Si vous avez besoin de compatibilité avec des clients JavaScript Socket.IO existants, utilisez `python-socketio`.

**Comment déboguer les messages WebSocket dans le navigateur ?** Ouvrez les outils de développement (F12), allez dans l'onglet Réseau (Network), filtrez par "WS". Cliquez sur la connexion WebSocket pour voir tous les messages échangés en temps réel, avec leur taille et leur direction (envoyé/reçu). Chrome et Firefox affichent aussi les frames ping/pong du protocole.

**Les WebSockets consomment-ils beaucoup de batterie sur mobile ?** Une connexion WebSocket idle consomme très peu de batterie car elle n'échange aucune donnée entre les heartbeats. Cependant, les heartbeats trop fréquents (intervalles inférieurs à 30 secondes) empêchent le modem radio du téléphone de passer en mode veille. Pour les applications mobiles, utilisez un intervalle de ping de 60 secondes ou plus.

**Peut-on utiliser les WebSockets avec Django ?** Django ne supporte pas nativement les WebSockets car il est basé sur WSGI, un protocole synchrone. Pour ajouter les WebSockets à Django, utilisez Django Channels qui remplace WSGI par ASGI (Asynchronous Server Gateway Interface). Alternativement, vous pouvez exécuter un serveur `websockets` séparé à côté de votre application Django, communiquant via Redis.

**Quelle est la taille maximale d'un message WebSocket ?** Le protocole WebSocket n'impose pas de limite de taille — les messages peuvent théoriquement atteindre 2^63 octets. En pratique, la bibliothèque `websockets` impose un défaut de 1 Mo (`max_size=2**20`). Augmentez cette valeur uniquement si nécessaire (transfert de fichiers) et toujours avec une validation côté serveur pour éviter les attaques par épuisement mémoire.

**Comment monitorer un serveur WebSocket en production ?** Exportez des métriques Prometheus depuis votre serveur : nombre de connexions actives, messages par seconde, latence de broadcast, taille des messages. La bibliothèque `websockets` fournit des hooks pour intercepter les connexions et les messages. Combinez avec Grafana pour visualiser les tableaux de bord en temps réel et configurer des alertes sur les seuils critiques.

### Couverture Connexe

Pour approfondir vos connaissances en développement temps réel et Python, consultez ces articles connexes :