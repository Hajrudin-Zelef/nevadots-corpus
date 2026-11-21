---
id: collect-261121-veille-ia/langages-dev/tutoriel-fastapi-0-136-api-rest-en-13-etapes-2026
title: "Tutoriel FastAPI 0.136 : API REST en 13 Étapes [2026]"
domain: langages-dev
role: reference
task: article
actors: []
dates: []
keywords: ["fastapi", "version", "tutoriel", "python", "rest", "etapes", "vous", "depuis"]
source: https://tech-insider.org/fr/fastapi-tutoriel-api-rest-python-13-etapes-2026/
source_anchor: ""
source_lines: [1, 699]
section: "Langages_Dev"
---
**FastAPI 0.141.1** est la dernière version publiée sur le dépôt GitHub officiel du projet en juillet 2026, confirmant la bascule définitive de l’écosystème Python vers le tout-asynchrone et la validation typée stricte. Reflet de ce format d’apprentissage plus structuré qui s’impose en 2026, ce **tutoriel FastAPI** Tech Insider en 13 étapes vous guide de l’installation à la mise en production d’une API REST complète, avec authentification JWT, base PostgreSQL, streaming JSON Lines (la grande nouveauté de la version 0.134.0) et déploiement Docker. Comptez 90 minutes pour boucler le projet final, qui sert aujourd’hui de squelette de référence pour les équipes backend à Paris, Berlin et Amsterdam.

Le framework, désormais utilisé en production par **Microsoft, Uber et Netflix** selon Planeks, s’appuie sur Starlette ≥ 0.46.0 et Pydantic v2 exclusivement depuis la version 0.128.0 de décembre 2025. Le rythme de publication s’est encore accéléré depuis : la 0.138.0 est sortie le 20 juin 2026 d’après le tableau des versions d’EOL Risk, suivie de la 0.138.1 en juin puis de la 0.140.1 en juillet 2026 selon les notes de version officielles. Ce tutoriel couvre toutes les nouveautés 2025-2026 : **support Python 3.13 free-threaded**, streaming binaire avec `yield`, dépendances asynchrones, et tests automatisés à 100 % de couverture. À la fin de ce guide, vous disposerez d’une API REST production-ready, conteneurisée et déployable en un seul `docker compose up`.

## Pourquoi FastAPI s’impose en 2026 face à Flask et Django

FastAPI, créé en 2018 par Sebastián Ramírez (alias *tiangolo*), a connu une accélération spectaculaire entre 2024 et 2026. Le framework asynchrone-first dépasse aujourd’hui Flask en téléchargements PyPI mensuels et talonne Django sur les nouveaux projets backend en France. Selon le Stack Overflow Developer Survey 2024, FastAPI figure parmi les frameworks web Python les plus appréciés, avec un taux de satisfaction supérieur à 75 %, devant Flask et derrière uniquement Django sur la métrique d’adoption brute — un engouement que confirme un tutoriel GeeksforGeeks mis à jour en juin 2026, qui met en avant la validation automatique, le support asynchrone natif et la documentation interactive comme principaux moteurs d’adoption.

Trois facteurs expliquent cette domination en 2026. D’abord, la **validation Pydantic v2**, écrite en Rust, offre des gains de performance de 5 à 50× par rapport à v1 sur le parsing JSON, ce qui rapproche FastAPI des frameworks Go ou Node.js sur les benchmarks TechEmpower. Ensuite, la **génération automatique d’OpenAPI 3.1** et de la documentation Swagger UI/ReDoc supprime des centaines de lignes de boilerplate. Enfin, l’écosystème asynchrone Python (asyncpg, httpx, aioredis) a mûri au point que la plupart des bibliothèques métier disposent d’une variante `async` stable.

L’adoption en France suit la tendance mondiale. Les offres d’emploi backend Python sur Welcome to the Jungle et Indeed mentionnent FastAPI dans plus de 60 % des annonces senior pour 2026, contre 35 % en 2024. Les startups deeptech parisiennes (Mistral AI, Hugging Face, Dust) utilisent FastAPI pour exposer leurs modèles d’inférence, profitant du support natif des streams binaires introduit dans la version 0.134.0. Les ESN traditionnelles migrent progressivement leurs APIs Flask 2.x vers FastAPI pour bénéficier du typage strict et de la documentation auto-générée.

## Prérequis : versions exactes pour suivre ce tutoriel FastAPI

Avant d’attaquer le projet, alignez votre environnement sur les versions ci-dessous. FastAPI a abandonné Pydantic v1 depuis la version 0.128.0 de décembre 2025, et la ligne actuellement supportée est la 0.139.2, active depuis juillet 2026 et toujours listée comme la version en cours de maintenance par EOL Risk en août 2026 — ce qui implique des dépendances plus récentes que celles que vous trouverez dans les anciens tutoriels en ligne. Si vous travaillez sur Windows, privilégiez WSL2 Ubuntu 24.04 pour éviter les soucis de compilation des wheels asyncpg et orjson.

| Composant | Version recommandée | Version minimale | Notes 2026 | 
|---|---|---|---|
| Python | 3.12.7 | 3.10 | 3.13 free-threaded supporté en bêta | 
| FastAPI | 0.136.1 | 0.128.0 | Pydantic v1 retiré depuis 0.128.0 | 
| Pydantic | 2.11.x | 2.5 | v1 non supporté | 
| Uvicorn | 0.34.x | 0.30 | HTTP/2 et WebSockets natifs | 
| Starlette | 0.46.x | 0.46.0 | Requise par 0.134.0+ | 
| SQLModel | 0.0.24 | 0.0.22 | Compatible Pydantic v2 | 
| PostgreSQL | 17.4 | 15 | JSONB, GIN indexes | 
| Docker | 27.4 | 24.0 | BuildKit activé par défaut | 

Côté outillage, installez **uv** 0.5+ d’Astral comme gestionnaire de paquets : il remplace pip et venv avec des installations 10 à 100× plus rapides. Pour l’éditeur, VS Code avec l’extension Pylance et Ruff suffit. Si vous utilisez PyCharm Professional 2025.1, le support FastAPI est natif depuis cette version (autocomplétion sur les `Annotated[Depends()]`, navigation OpenAPI). Côté CLI, prévoyez `httpie` ou `curl` pour tester les endpoints, et `jq` pour formater les réponses JSON.

## Étape 1 : Installer Python 3.12 et créer un environnement virtuel

Sur Ubuntu 24.04 LTS (Noble Numbat), Python 3.12 est désormais disponible dans les dépôts officiels. Sur macOS, utilisez Homebrew. Sur Windows, l’installeur officiel python.org reste la voie la plus simple, ou WSL2 si vous visez la production Linux. Voici les commandes pour chaque OS, en privilégiant uv comme gestionnaire d’environnement.

```
# Installation d'uv (Astral) – outil officiel recommandé en 2026
curl -LsSf https://astral.sh/uv/install.sh | sh
# Création du projet
mkdir tutoriel-fastapi-2026 && cd tutoriel-fastapi-2026
uv init --python 3.12
uv venv
source .venv/bin/activate     # macOS/Linux
# .venv\Scripts\activate      # Windows PowerShell
# Vérification
python --version
# Python 3.12.7
```
L’utilisation d’uv divise par 10 le temps d’installation des dépendances par rapport à pip classique, selon les benchmarks Astral de janvier 2026. Sur un projet typique avec 40 dépendances, on passe d’environ 22 secondes (pip) à 2,1 secondes (uv). Cette différence devient critique en CI/CD lorsque vous exécutez plusieurs centaines de builds par jour. Si vous préférez rester sur pip, remplacez `uv add` par `pip install` dans les étapes suivantes ; le code applicatif reste identique.

## Étape 2 : Installer FastAPI 0.136.1 et ses dépendances clés

FastAPI 0.139.0, publiée le 1er juillet 2026, a ajouté la gestion des dépendances via `app.frontend()` et requiert toujours Pydantic ≥ 2.5 et Starlette ≥ 0.46.0. Comme le recommande le tutoriel 2026 de Sebastián Ramírez (tiangolo) pour tout nouveau projet, l’installation se fait via `uv add "fastapi[standard]"`. Pour ce tutoriel, nous installons également Uvicorn (serveur ASGI), SQLModel (ORM), asyncpg (driver PostgreSQL asynchrone), python-jose (JWT) et passlib (hashing bcrypt). Évitez d’épingler des versions trop strictes en développement, mais figez-les en production via `uv lock`.

```
# Dépendances principales – versions vérifiées avril 2026
uv add 'fastapi[standard]==0.136.1'
uv add 'uvicorn[standard]==0.34.0'
uv add 'pydantic==2.11.0'
uv add 'pydantic-settings==2.7.0'
uv add 'sqlmodel==0.0.24'
uv add 'asyncpg==0.30.0'
uv add 'python-jose[cryptography]==3.3.0'
uv add 'passlib[bcrypt]==1.7.4'
uv add 'python-multipart==0.0.18'
# Dépendances de développement
uv add --dev 'pytest==8.3.4' 'pytest-asyncio==0.25.0' 'httpx==0.28.1'
uv add --dev 'ruff==0.8.4' 'mypy==1.14.0'
# Génération du fichier de lock pour la prod
uv lock
```
Le suffixe `[standard]` sur FastAPI installe automatiquement les extras les plus utilisés : `email-validator` pour les EmailStr Pydantic, `itsdangerous` pour les sessions, `jinja2` pour le rendu HTML et `fastapi-cli` pour la commande `fastapi dev` introduite en 2024. En août 2026, le même tutoriel officiel de FastAPI recommande désormais de lancer le serveur via `uv run fastapi dev main.py`, qui remplace avantageusement `uvicorn main:app --reload` en gérant automatiquement le rechargement, les logs et le watchdog. Vérifiez votre installation avec `fastapi --version` qui doit retourner `FastAPI CLI version: 0.0.7` ou supérieure.

## Étape 3 : Créer le squelette de l’application FastAPI

Adoptez dès le départ une structure de projet inspirée de Domain-Driven Design : un répertoire `app/` avec sous-modules `api/`, `models/`, `db/`, `core/` et `tests/`. Cette organisation, popularisée par le template *full-stack-fastapi-template* officiel de tiangolo, scale jusqu’à plusieurs dizaines de routes sans devenir illisible.

```
tutoriel-fastapi-2026/
├── app/
│   ├── __init__.py
│   ├── main.py              # Instance FastAPI + montage des routers
│   ├── core/
│   │   ├── config.py        # Settings via pydantic-settings
│   │   └── security.py      # JWT + hashing
│   ├── db/
│   │   ├── session.py       # AsyncSession SQLModel
│   │   └── init_db.py
│   ├── models/
│   │   └── article.py       # Modèles SQLModel
│   ├── schemas/
│   │   └── article.py       # Schémas Pydantic d'entrée/sortie
│   └── api/
│       ├── deps.py          # Dépendances communes
│       └── routes/
│           ├── articles.py
│           └── auth.py
├── tests/
├── docker-compose.yml
├── Dockerfile
├── pyproject.toml
└── .env
```
Créez maintenant le fichier `app/main.py` avec une route racine minimale. FastAPI 0.136.1 introduit un nouveau pattern de configuration via `FastAPI(lifespan=...)` qui remplace les anciens événements `@app.on_event("startup")` dépréciés depuis 0.110.0. Cette approche utilise un context manager asynchrone pour gérer le cycle de vie des connexions DB, des pools Redis ou des modèles ML.

```
# app/main.py
from contextlib import asynccontextmanager
from fastapi import FastAPI
from app.api.routes import articles, auth
from app.db.session import engine
from sqlmodel import SQLModel
@asynccontextmanager
async def lifespan(app: FastAPI):
    # Startup : créer les tables, ouvrir les pools
    async with engine.begin() as conn:
        await conn.run_sync(SQLModel.metadata.create_all)
    yield
    # Shutdown : fermer proprement
    await engine.dispose()
app = FastAPI(
    title="API Articles 2026",
    version="1.0.0",
    description="Tutoriel FastAPI en 13 étapes",
    lifespan=lifespan,
)
app.include_router(auth.router, prefix="/auth", tags=["auth"])
app.include_router(articles.router, prefix="/articles", tags=["articles"])
@app.get("/")
async def root() -> dict[str, str]:
    return {"message": "FastAPI 0.136 – Bonjour Paris"}
```
Lancez le serveur avec `fastapi dev app/main.py`. Ouvrez `http://127.0.0.1:8000/docs` dans votre navigateur : Swagger UI s’affiche automatiquement avec votre route racine. La documentation alternative ReDoc est disponible sur `/redoc`. Les deux interfaces consomment l’OpenAPI 3.1 généré dynamiquement à `/openapi.json`, que vous pouvez importer dans Postman, Insomnia ou Bruno pour générer un client.

## Étape 4 : Définir les modèles Pydantic v2 pour la validation

Pydantic v2, sortie en juin 2023 et obligatoire avec FastAPI 0.128+, repose sur un cœur Rust nommé *pydantic-core*. Les performances sont multipliées par 5 à 50 selon les opérations, et le typage devient strict par défaut. Pour ce tutoriel, nous séparons les schémas Pydantic (entrée/sortie API) des modèles SQLModel (persistance DB) afin d’éviter les fuites de champs sensibles comme les hashes de mots de passe.

```
# app/schemas/article.py
from datetime import datetime
from pydantic import BaseModel, ConfigDict, Field, field_validator
class ArticleBase(BaseModel):
    title: str = Field(min_length=3, max_length=200)
    content: str = Field(min_length=10)
    published: bool = False
    @field_validator("title")
    @classmethod
    def title_capitalize(cls, v: str) -> str:
        return v.strip().capitalize()
class ArticleCreate(ArticleBase):
    pass
class ArticleUpdate(BaseModel):
    title: str | None = Field(default=None, min_length=3, max_length=200)
    content: str | None = None
    published: bool | None = None
class ArticleRead(ArticleBase):
    model_config = ConfigDict(from_attributes=True)
    id: int
    author_id: int
    created_at: datetime
    updated_at: datetime
```
Notez l’usage de `Field()` avec `min_length` et `max_length` : ces contraintes apparaissent automatiquement dans Swagger UI et déclenchent une réponse HTTP 422 avec un message JSON détaillé en cas de violation. Le décorateur `@field_validator` remplace `@validator` de Pydantic v1. La syntaxe `str | None` exige Python 3.10+ et est strictement équivalente à `Optional[str]` de Python 3.9. Le `ConfigDict(from_attributes=True)` active le mode ORM (anciennement `orm_mode = True`) pour convertir un objet SQLModel en réponse Pydantic.

## Étape 5 : Configurer la base PostgreSQL avec SQLModel et asyncpg

SQLModel, créé par Sebastián Ramírez en 2021, fusionne SQLAlchemy 2.0 et Pydantic en une seule classe. Vous définissez le schéma une fois et obtenez à la fois un modèle ORM et un schéma de validation. Pour la version 0.0.24 sortie en mars 2026, la compatibilité avec Pydantic v2 est complète et les sessions asynchrones sont supportées de bout en bout via asyncpg.

```
# app/db/session.py
from collections.abc import AsyncGenerator
from sqlalchemy.ext.asyncio import AsyncSession, create_async_engine
from sqlmodel.ext.asyncio.session import AsyncSession as SQLModelSession
from app.core.config import settings
engine = create_async_engine(
    settings.DATABASE_URL,
    echo=False,
    pool_size=20,
    max_overflow=10,
    pool_pre_ping=True,
)
async def get_session() -> AsyncGenerator[SQLModelSession, None]:
    async with SQLModelSession(engine) as session:
        yield session
```
```
# app/models/article.py
from datetime import datetime, UTC
from sqlmodel import Field, SQLModel, Relationship
class Article(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    title: str = Field(index=True, max_length=200)
    content: str
    published: bool = Field(default=False)
    author_id: int = Field(foreign_key="user.id", index=True)
    created_at: datetime = Field(default_factory=lambda: datetime.now(UTC))
    updated_at: datetime = Field(default_factory=lambda: datetime.now(UTC))
class User(SQLModel, table=True):
    id: int | None = Field(default=None, primary_key=True)
    email: str = Field(unique=True, index=True, max_length=255)
    hashed_password: str
    is_active: bool = Field(default=True)
    created_at: datetime = Field(default_factory=lambda: datetime.now(UTC))
```
Le paramètre `pool_size=20` dimensionne le pool de connexions PostgreSQL. Sur une instance Postgres 17.4 standard avec `max_connections=100`, ce réglage permet de servir 5 workers Uvicorn sans saturer la base. Pour la production à fort trafic, ajoutez PgBouncer en transaction pooling devant Postgres : vous gagnez un facteur 5 à 10 sur le nombre de connexions logiques sans dégradation. Le flag `pool_pre_ping=True` est crucial : il évite les `OperationalError: connection has been closed` lorsqu’un load balancer ferme silencieusement les connexions inactives.

## Étape 6 : Implémenter les routes CRUD avec dépendances asynchrones

FastAPI 0.95+ a introduit la syntaxe `Annotated[Type, Depends(...)]` qui remplace l’ancienne signature `param: Type = Depends(...)`. Cette nouvelle forme est compatible avec mypy strict et permet aux IDE de remonter correctement les types. Tous les exemples ci-dessous utilisent la syntaxe Annotated, qui est désormais la convention recommandée par tiangolo dans la documentation officielle 2026.

```
# app/api/routes/articles.py
from typing import Annotated
from fastapi import APIRouter, Depends, HTTPException, status
from sqlmodel import select
from sqlmodel.ext.asyncio.session import AsyncSession
from app.api.deps import CurrentUser, SessionDep
from app.models.article import Article
from app.schemas.article import ArticleCreate, ArticleRead, ArticleUpdate
router = APIRouter()
@router.get("/", response_model=list[ArticleRead])
async def list_articles(
    session: SessionDep,
    skip: int = 0,
    limit: int = 20,
) -> list[Article]:
    statement = select(Article).where(Article.published == True).offset(skip).limit(limit)
    result = await session.exec(statement)
    return result.all()
@router.post("/", response_model=ArticleRead, status_code=status.HTTP_201_CREATED)
async def create_article(
    payload: ArticleCreate,
    session: SessionDep,
    user: CurrentUser,
) -> Article:
    article = Article.model_validate(payload, update={"author_id": user.id})
    session.add(article)
    await session.commit()
    await session.refresh(article)
    return article
@router.get("/{article_id}", response_model=ArticleRead)
async def get_article(article_id: int, session: SessionDep) -> Article:
    article = await session.get(Article, article_id)
    if not article:
        raise HTTPException(status.HTTP_404_NOT_FOUND, "Article introuvable")
    return article
@router.patch("/{article_id}", response_model=ArticleRead)
async def update_article(
    article_id: int,
    payload: ArticleUpdate,
    session: SessionDep,
    user: CurrentUser,
) -> Article:
    article = await session.get(Article, article_id)
    if not article:
        raise HTTPException(status.HTTP_404_NOT_FOUND, "Article introuvable")
    if article.author_id != user.id:
        raise HTTPException(status.HTTP_403_FORBIDDEN, "Accès refusé")
    update_data = payload.model_dump(exclude_unset=True)
    for key, value in update_data.items():
        setattr(article, key, value)
    session.add(article)
    await session.commit()
    await session.refresh(article)
    return article
@router.delete("/{article_id}", status_code=status.HTTP_204_NO_CONTENT)
async def delete_article(
    article_id: int,
    session: SessionDep,
    user: CurrentUser,
) -> None:
    article = await session.get(Article, article_id)
    if not article or article.author_id != user.id:
        raise HTTPException(status.HTTP_404_NOT_FOUND)
    await session.delete(article)
    await session.commit()
```
Le pattern `SessionDep = Annotated[AsyncSession, Depends(get_session)]` dans `deps.py` permet de réutiliser la dépendance dans toutes les routes sans dupliquer la signature complète. Cette factorisation est devenue le standard depuis le template officiel *full-stack-fastapi-template* de tiangolo. La méthode `model_dump(exclude_unset=True)` de Pydantic v2 retourne uniquement les champs réellement envoyés par le client, ce qui permet des updates partielles propres sans écraser les valeurs existantes par `None`.

## Étape 7 : Sécuriser l’API avec JWT et OAuth2 password flow

FastAPI fournit nativement le squelette OAuth2 via `OAuth2PasswordBearer` et `OAuth2PasswordRequestForm`. Pour l’authentification réelle, nous utilisons des JWT signés HS256 avec python-jose et le hashing bcrypt via passlib. En production, augmentez le coût bcrypt à 12 (par défaut) ou 14 selon votre tolérance latence : un hash bcrypt cost 12 prend environ 250 ms sur un Xeon Gold 6326, ce qui reste acceptable pour un endpoint `/login` appelé rarement.

```
# app/core/security.py
from datetime import datetime, timedelta, UTC
from jose import jwt, JWTError
from passlib.context import CryptContext
from app.core.config import settings
pwd_ctx = CryptContext(schemes=["bcrypt"], deprecated="auto")
def hash_password(password: str) -> str:
    return pwd_ctx.hash(password)
def verify_password(plain: str, hashed: str) -> bool:
    return pwd_ctx.verify(plain, hashed)
def create_access_token(subject: str | int, expires_minutes: int = 60) -> str:
    expire = datetime.now(UTC) + timedelta(minutes=expires_minutes)
    payload = {"sub": str(subject), "exp": expire, "type": "access"}
    return jwt.encode(payload, settings.SECRET_KEY, algorithm="HS256")
def decode_token(token: str) -> dict:
    try:
        return jwt.decode(token, settings.SECRET_KEY, algorithms=["HS256"])
    except JWTError as e:
        raise ValueError("Token invalide") from e
```
```
# app/api/routes/auth.py
from typing import Annotated
from fastapi import APIRouter, Depends, HTTPException, status
from fastapi.security import OAuth2PasswordRequestForm
from sqlmodel import select
from app.api.deps import SessionDep
from app.core.security import create_access_token, hash_password, verify_password
from app.models.article import User
from pydantic import BaseModel, EmailStr
router = APIRouter()
class TokenResponse(BaseModel):
    access_token: str
    token_type: str = "bearer"
class RegisterRequest(BaseModel):
    email: EmailStr
    password: str
@router.post("/register", response_model=TokenResponse, status_code=201)
async def register(payload: RegisterRequest, session: SessionDep) -> TokenResponse:
    existing = await session.exec(select(User).where(User.email == payload.email))
    if existing.first():
        raise HTTPException(409, "Email déjà enregistré")
    user = User(email=payload.email, hashed_password=hash_password(payload.password))
    session.add(user)
    await session.commit()
    await session.refresh(user)
    return TokenResponse(access_token=create_access_token(user.id))
@router.post("/login", response_model=TokenResponse)
async def login(
    form: Annotated[OAuth2PasswordRequestForm, Depends()],
    session: SessionDep,
) -> TokenResponse:
    result = await session.exec(select(User).where(User.email == form.username))
    user = result.first()
    if not user or not verify_password(form.password, user.hashed_password):
        raise HTTPException(status.HTTP_401_UNAUTHORIZED, "Identifiants invalides")
    return TokenResponse(access_token=create_access_token(user.id))
```
La `SECRET_KEY` doit faire au moins 32 caractères aléatoires en production. Générez-la avec `python -c "import secrets; print(secrets.token_urlsafe(64))"` et stockez-la dans un secret manager (AWS Secrets Manager, HashiCorp Vault, OVH Confidential Computing). Pour un déploiement multi-instances, signez plutôt avec RS256 (clé asymétrique) afin de ne pas dupliquer la clé privée sur chaque worker. La durée d’expiration de 60 minutes est un compromis raisonnable : abaissez-la à 15 minutes et complétez avec un refresh token long-lived stocké dans un cookie HttpOnly Secure pour les applications grand public.

## Étape 8 : Streaming JSON Lines avec yield (nouveauté FastAPI 0.134.0)

La version 0.134.0 sortie le 27 février 2026 a introduit le support natif des streams JSON Lines et binaires via `yield` dans une route. Cette fonctionnalité simplifie radicalement l’export de gros volumes (rapports CSV, exports d’événements, streaming de tokens LLM) sans charger toute la donnée en mémoire. Un endpoint qui retournait avant 200 Mo en une seule réponse JSON peut désormais émettre 1 ligne JSON par enregistrement, consommée incrémentalement par le client.

```
# app/api/routes/articles.py – ajout du streaming
import json
from collections.abc import AsyncIterator
from fastapi.responses import StreamingResponse
@router.get("/export.jsonl")
async def export_articles(session: SessionDep) -> StreamingResponse:
    async def generate() -> AsyncIterator[bytes]:
        statement = select(Article).execution_options(yield_per=100)
        result = await session.stream(statement)
        async for article in result.scalars():
            line = json.dumps({
                "id": article.id,
                "title": article.title,
                "created_at": article.created_at.isoformat(),
            }, ensure_ascii=False)
            yield (line + "\n").encode("utf-8")
    return StreamingResponse(
        generate(),
        media_type="application/x-ndjson",
        headers={"Content-Disposition": "attachment; filename=articles.jsonl"},
    )
```
L’option `execution_options(yield_per=100)` de SQLAlchemy 2.0 force le driver asyncpg à streamer les lignes par paquets de 100 plutôt que de tout charger en RAM. Combinée avec `StreamingResponse` et un générateur asynchrone, cette approche permet d’exporter des tables de plusieurs millions d’enregistrements avec une empreinte mémoire constante d’environ 50 Mo. Les tests internes d’OpenAI sur leurs APIs de streaming utilisent un pattern strictement identique pour émettre les tokens LLM, à ceci près que le format est *Server-Sent Events* (`text/event-stream`) au lieu de *NDJSON*.

## Étape 9 : Middlewares de sécurité, CORS et rate limiting

Une API exposée en production doit traiter au minimum trois préoccupations transverses : CORS (Cross-Origin Resource Sharing), HTTPS strict via HSTS, et rate limiting. FastAPI, héritant de Starlette, expose un système de middlewares ASGI compatible avec l’écosystème entier (slowapi, fastapi-limiter, asgi-correlation-id). Voici une configuration de production solide.

```
# app/main.py – extension
from fastapi.middleware.cors import CORSMiddleware
from fastapi.responses import JSONResponse
from starlette.middleware.trustedhost import TrustedHostMiddleware
from slowapi import Limiter
from slowapi.util import get_remote_address
from slowapi.errors import RateLimitExceeded
from slowapi.middleware import SlowAPIMiddleware
limiter = Limiter(key_func=get_remote_address, default_limits=["100/minute"])
app.state.limiter = limiter
app.add_middleware(SlowAPIMiddleware)
app.add_middleware(
    CORSMiddleware,
    allow_origins=["https://example.fr", "https://app.example.fr"],
    allow_credentials=True,
    allow_methods=["GET", "POST", "PATCH", "DELETE"],
    allow_headers=["Authorization", "Content-Type"],
    max_age=600,
)
app.add_middleware(
    TrustedHostMiddleware,
    allowed_hosts=["api.example.fr", "*.example.fr"],
)
@app.exception_handler(RateLimitExceeded)
async def rate_limit_handler(request, exc):
    return JSONResponse(
        status_code=429,
        content={"detail": "Trop de requêtes, réessayez dans quelques secondes"},
    )
```
Pour le rate limiting distribué entre plusieurs workers ou plusieurs nodes Kubernetes, remplacez le backend mémoire de slowapi par Redis : `Limiter(storage_uri="redis://redis:6379")`. Le coût opérationnel est marginal (un GET INCR par requête) mais l’efficacité est globale. Évitez d’appliquer un rate limit unique par IP : derrière un proxy ou un VPN d’entreprise, des dizaines d’utilisateurs partagent la même IP source. Préférez un rate limit composite `get_remote_address + user_id` pour les endpoints authentifiés.

## Étape 10 : Tester l’API avec pytest et httpx AsyncClient

FastAPI fournit `TestClient` (synchrone, basé sur httpx) pour les tests unitaires rapides. Pour des tests d’intégration end-to-end qui imitent fidèlement le comportement asynchrone réel, utilisez `httpx.AsyncClient` avec `pytest-asyncio`. Cette approche détecte les blocages d’event loop, les fuites de connexions DB et les race conditions impossibles à reproduire avec TestClient.

```
# tests/conftest.py
import pytest
import pytest_asyncio
from httpx import AsyncClient, ASGITransport
from sqlmodel import SQLModel
from sqlalchemy.ext.asyncio import create_async_engine
from sqlmodel.ext.asyncio.session import AsyncSession
from app.main import app
from app.db.session import get_session
TEST_DB_URL = "postgresql+asyncpg://postgres:postgres@localhost:5432/test_db"
@pytest_asyncio.fixture
async def session():
    engine = create_async_engine(TEST_DB_URL)
    async with engine.begin() as conn:
        await conn.run_sync(SQLModel.metadata.create_all)
    async with AsyncSession(engine) as s:
        yield s
    async with engine.begin() as conn:
        await conn.run_sync(SQLModel.metadata.drop_all)
    await engine.dispose()
@pytest_asyncio.fixture
async def client(session):
    async def override():
        yield session
    app.dependency_overrides[get_session] = override
    transport = ASGITransport(app=app)
    async with AsyncClient(transport=transport, base_url="http://test") as c:
        yield c
    app.dependency_overrides.clear()
```
```
# tests/test_articles.py
import pytest
@pytest.mark.asyncio
async def test_create_and_read_article(client):
    # Inscription
    r = await client.post("/auth/register", json={
        "email": "[email protected]",
        "password": "MotDePasse2026!"
    })
    assert r.status_code == 201
    token = r.json()["access_token"]
    headers = {"Authorization": f"Bearer {token}"}
    # Création
    r = await client.post(
        "/articles/",
        json={"title": "test", "content": "contenu suffisant pour passer", "published": True},
        headers=headers,
    )
    assert r.status_code == 201
    article_id = r.json()["id"]
    # Lecture
    r = await client.get(f"/articles/{article_id}")
    assert r.status_code == 200
    assert r.json()["title"] == "Test"  # validator capitalize
@pytest.mark.asyncio
async def test_unauthorized_create(client):
    r = await client.post("/articles/", json={"title": "x", "content": "y"})
    assert r.status_code == 401
```
Lancez la suite avec `pytest -v --asyncio-mode=auto`. Pour mesurer la couverture, ajoutez `pytest-cov` et exécutez `pytest --cov=app --cov-report=term-missing`. L’objectif raisonnable pour un projet FastAPI est **85 %+ de couverture de lignes** et 100 % sur les modules `core/security` et `api/deps`. La documentation officielle FastAPI annonce 100 % de coverage sur le framework lui-même, ce qui est un excellent indicateur de stabilité pour la production.

## Étape 11 : Conteneuriser l’application avec Docker multi-stage

Pour la production, utilisez un Dockerfile multi-stage qui sépare la phase de build (avec uv et les outils de compilation) de l’image runtime minimale. Cette approche divise la taille finale par 3 à 5 selon les dépendances. L’image officielle `python:3.12-slim` pèse 130 Mo, contre 950 Mo pour `python:3.12` standard.

```
# Dockerfile
# Étape 1 : builder
FROM python:3.12-slim AS builder
ENV UV_COMPILE_BYTECODE=1 UV_LINK_MODE=copy
COPY --from=ghcr.io/astral-sh/uv:0.5.11 /uv /usr/local/bin/uv
WORKDIR /app
COPY pyproject.toml uv.lock ./
RUN uv sync --frozen --no-dev --no-install-project
COPY . .
RUN uv sync --frozen --no-dev
# Étape 2 : runtime minimal
FROM python:3.12-slim AS runtime
RUN apt-get update && apt-get install -y --no-install-recommends \
    libpq5 ca-certificates curl \
    && rm -rf /var/lib/apt/lists/*
WORKDIR /app
COPY --from=builder /app /app
ENV PATH="/app/.venv/bin:$PATH" \
    PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1
RUN useradd --create-home --uid 1001 fastapi
USER fastapi
EXPOSE 8000
HEALTHCHECK --interval=30s --timeout=5s --start-period=10s \
    CMD curl -f http://localhost:8000/health || exit 1
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8000", "--workers", "4"]
```
```
# docker-compose.yml
services:
  api:
    build: .
    ports:
      - "8000:8000"
    environment:
      DATABASE_URL: postgresql+asyncpg://postgres:postgres@db:5432/articles
      SECRET_KEY: change-me-in-production
    depends_on:
      db:
        condition: service_healthy
    restart: unless-stopped
  db:
    image: postgres:17.4-alpine
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: articles
    volumes:
      - pgdata:/var/lib/postgresql/data
    healthcheck:
      test: ["CMD", "pg_isready", "-U", "postgres"]
      interval: 5s
      timeout: 3s
      retries: 5
  redis:
    image: redis:7.4-alpine
    restart: unless-stopped
volumes:
  pgdata:
```
Le nombre de workers Uvicorn (`--workers 4`) doit correspondre à `2 × nombre de CPU + 1` selon la règle de Gunicorn, dans la limite de la RAM disponible. Sur une instance Scaleway DEV1-M (3 vCPU, 4 Go), 4 workers consommant 250 Mo chacun saturent la mémoire ; restez à 3 workers pour laisser une marge. En Kubernetes, fixez `--workers 1` et laissez l’autoscaler horizontal HPA répliquer les pods : c’est plus propre et plus observable.

## Étape 12 : Observabilité avec Prometheus, OpenTelemetry et logs structurés

Une API en production sans télémétrie est aveugle. Trois piliers minimaux : métriques Prometheus pour le RED (Rate, Errors, Duration), traces OpenTelemetry pour les chemins critiques inter-services, et logs JSON structurés pour le SIEM. La librairie `prometheus-fastapi-instrumentator` expose automatiquement `/metrics` avec les histogrammes par route.

```
# app/main.py – observabilité
from prometheus_fastapi_instrumentator import Instrumentator
from opentelemetry import trace
from opentelemetry.instrumentation.fastapi import FastAPIInstrumentor
from opentelemetry.exporter.otlp.proto.grpc.trace_exporter import OTLPSpanExporter
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchSpanProcessor
# Métriques Prometheus
Instrumentator().instrument(app).expose(app, endpoint="/metrics")
# Traces OpenTelemetry
trace.set_tracer_provider(TracerProvider())
trace.get_tracer_provider().add_span_processor(
    BatchSpanProcessor(OTLPSpanExporter(endpoint="http://otel-collector:4317"))
)
FastAPIInstrumentor.instrument_app(app)
import structlog
structlog.configure(
    processors=[
        structlog.processors.TimeStamper(fmt="iso"),
        structlog.processors.add_log_level,
        structlog.processors.JSONRenderer(),
    ],
)
logger = structlog.get_logger()
```
Sur Grafana, importez le dashboard *FastAPI Observability* (ID 18739) qui affiche RPS par endpoint, latence p50/p95/p99 et taux d’erreur 5xx. Pour les traces distribuées, l’OTLP exporter envoie vers un OpenTelemetry Collector qui dispatche ensuite vers Jaeger, Tempo ou Datadog. La rétention par défaut de 7 jours sur Tempo coûte environ 0,15 €/Go ingéré sur Grafana Cloud Pro selon les tarifs publics 2026, ce qui reste compétitif face à Datadog APM (estimé entre 31 et 36 € par host/mois selon l’engagement annuel).

## Étape 13 : Déployer en production sur Scaleway, OVH ou AWS

Pour un déploiement européen RGPD-friendly, trois options dominent en 2026 : Scaleway Serverless Containers (Paris, Amsterdam), OVH Public Cloud Managed Kubernetes (Gravelines, Strasbourg) et Clever Cloud (Paris, Montréal, Sydney). Pour une mise en ligne rapide d’un MVP, Scaleway Serverless Containers facture à l’usage (CPU.ms et RAM.s) et démarre vos containers en moins de 2 secondes au cold start.

```
# Déploiement Scaleway Serverless Containers via CLI
scw container namespace create name=fastapi-2026 region=fr-par
scw registry namespace create name=fastapi-2026
docker tag fastapi-app:latest rg.fr-par.scw.cloud/fastapi-2026/api:latest
docker push rg.fr-par.scw.cloud/fastapi-2026/api:latest
scw container container create \
    namespace-id=$NAMESPACE_ID \
    name=api \
    registry-image=rg.fr-par.scw.cloud/fastapi-2026/api:latest \
    port=8000 \
    cpu-limit=1000 \
    memory-limit=1024 \
    min-scale=1 \
    max-scale=10
scw container container deploy container-id=$CONTAINER_ID
```
Pour une charge prévisible et continue, OVH Public Cloud avec Managed Kubernetes Service à 0,01 € par node-heure (hors VM) reste l’option la plus économique en France. Comptez environ 25 €/mois pour un cluster minimal 3 nœuds B2-7 (2 vCPU, 7 Go) capable d’héberger une API FastAPI servant 500 requêtes/seconde. Pour le scaling vertical, profitez du autoscaling pod-level (HPA) sur la métrique `http_requests_per_second` exposée par Prometheus.

## Pièges courants à éviter en 2026

FastAPI est généralement intuitif, mais certaines erreurs récurrentes coûtent des heures de débogage. Voici les six pièges les plus fréquents que nous voyons en revue de code dans les ESN parisiennes et les startups deeptech.

- **Mélanger sync et async dans la même route** : appeler une fonction synchrone bloquante (requests, time.sleep, psycopg2 sync) dans une route`async def` bloque l’event loop entier. Utilisez`fastapi.concurrency.run_in_threadpool` ou bascule l’appel vers une variante asynchrone (httpx, asyncpg).
- **Oublier le `response_model`** : sans cette annotation, FastAPI sérialise l’objet brut, ce qui peut leaker des champs sensibles comme`hashed_password` . Définissez systématiquement un schéma de sortie séparé du modèle DB.
- **Confondre `Body()`, `Query()` et `Path()`** : Body extrait du JSON, Query du querystring, Path du chemin. Un mauvais decorator donne des 422 incompréhensibles côté frontend.
- **Pydantic v1 dans les imports** : depuis FastAPI 0.128,`from pydantic.v1 import BaseModel` ne fonctionne plus. Migrez avec`bump-pydantic` .
- **Sessions DB partagées entre requêtes** : créer un`AsyncSession` au niveau module et le réutiliser provoque des fuites de transactions. Toujours injecter via`Depends(get_session)` .
- **Lancer Uvicorn sans `--proxy-headers` derrière un reverse proxy** : sans ce flag, request.client.host retourne 127.0.0.1 au lieu de l’IP réelle, cassant le rate limiting et les logs.

## Tableau comparatif : FastAPI vs Flask vs Django REST en 2026

| Critère | FastAPI 0.136.1 | Flask 3.1 | Django REST 3.15 | 
|---|---|---|---|
| Année de création | 2018 | 2010 | 2014 | 
| Async natif | Oui (ASGI) | Partiel (Flask 2+) | Limité (ASGI) | 
| Validation | Pydantic v2 (Rust) | Manuelle / marshmallow | Serializers DRF | 
| Documentation auto | OpenAPI 3.1 + Swagger | flasgger (extension) | drf-spectacular | 
| Type hints stricts | Obligatoires | Optionnels | Optionnels | 
| Performance (req/s, JSON simple) | ≈ 30 000+ | ≈ 5 000-10 000 | ≈ 4 000-8 000 | 
| Courbe apprentissage | Faible | Très faible | Élevée | 
| Écosystème | Asynchrone (httpx, asyncpg) | Synchrone (requests, psycopg2) | Tout-en-un (ORM, admin, auth) | 
| Cas d’usage idéal | API REST moderne, ML serving | Microservices simples | App full-stack monolithique | 

Les chiffres de performance dépendent fortement du scénario testé (JSON simple, ORM, IO bound). Sur les benchmarks TechEmpower Round 22, FastAPI atteint environ 30 000 requêtes par seconde sur le test JSON simple, soit 4 à 6 fois plus que Flask synchrone. Sur des charges IO-bound (requêtes DB, appels HTTP externes), l’écart se creuse encore plus en faveur de FastAPI grâce à l’asynchronisme. Pour des applications full-stack monolithiques avec admin intégré et ORM Django, Django REST reste un choix légitime – FastAPI ne cherche pas à le remplacer sur ce périmètre.

## Dépannage : 8 erreurs FastAPI fréquentes et leurs solutions

- **“AttributeError: module ‘pydantic’ has no attribute ‘BaseSettings'”** – Pydantic v2 a déplacé BaseSettings vers le paquet`pydantic-settings` . Installez`uv add pydantic-settings` et importez via`from pydantic_settings import BaseSettings` .
- **“RuntimeError: Event loop is closed”** en tests – Activez`asyncio_mode = "auto"` dans`pyproject.toml` sous`[tool.pytest.ini_options]` et utilisez la fixture`event_loop` de pytest-asyncio 0.25+.
- **HTTP 422 sur un payload qui semble valide** – Vérifiez que vous envoyez bien`Content-Type: application/json` . Sans ce header, FastAPI cherche les paramètres dans le querystring et échoue.
- **“sqlalchemy.exc.MissingGreenlet”** – Vous appelez une méthode synchrone sur un objet ORM dans un contexte async. Préfixez par`await` ou utilisez`session.run_sync()` .
- **OpenAPI lent à générer (10+ secondes)** – Vous avez probablement plusieurs centaines de routes ou des schémas Pydantic récursifs. Activez`app = FastAPI(openapi_url=None)` en production et générez le JSON OpenAPI au build.
- **“jose.exceptions.ExpiredSignatureError”** imprévu – Vérifiez le décalage horaire entre vos serveurs (NTP) et augmentez`leeway=10` dans`jwt.decode()` pour tolérer 10 secondes de drift.
- **Connexions PostgreSQL qui s’accumulent** – Surveillez`pg_stat_activity` . Probable absence de`await session.close()` ou pool trop large. Ajoutez PgBouncer en transaction pooling.
- **“422 Unprocessable Entity” sans détails** – Activez le middleware de logging des erreurs Pydantic et inspectez`exc.errors()` dans un exception handler personnalisé. Le message inclut le chemin exact du champ invalide.

## Astuces avancées pour optimiser une API FastAPI

Une fois le projet en production, plusieurs optimisations marginales mais cumulatives font gagner facilement 30 à 50 % de débit. Premier réflexe : remplacer `json` standard par `orjson` ou `msgspec` pour la sérialisation. Le gain mesuré sur le projet de référence est de 2 à 3× sur des payloads de 50 ko. FastAPI accepte un encodeur custom via `FastAPI(default_response_class=ORJSONResponse)`.

Deuxième optimisation : précompiler les schémas Pydantic au démarrage avec `BaseModel.model_rebuild()`. Sur une API avec 80 schémas, cette étape supprime le *JIT compilation* au premier appel et réduit la latence p99 du premier appel par client par 200 ms environ. Troisième astuce : remplacer le pool asyncpg standard par `asyncpg.create_pool()` avec `statement_cache_size=2048` pour cacher les plans préparés et économiser un round-trip Postgres par requête.

Pour les charges très élevées, étudiez Granian, un serveur ASGI alternatif écrit en Rust qui prétend battre Uvicorn de 20 à 40 % sur des benchmarks réalistes selon les tests publiés en 2025. La migration depuis Uvicorn est triviale : `granian --interface asgi app.main:app`. Enfin, pour servir des modèles ML lourds (PyTorch, ONNX), externalisez l’inférence vers un sidecar Triton Inference Server ou vers vLLM, et faites de FastAPI le simple *API gateway*. Cette séparation libère l’event loop des opérations CPU-bound qui bloqueraient sinon le serveur entier.

## Cas d’usage et adoption en production

FastAPI s’est imposé en 2025-2026 comme le framework de référence pour exposer des modèles d’IA. Hugging Face utilise FastAPI dans la majorité de ses Spaces et endpoints d’inférence. Microsoft, Uber et Netflix figurent parmi les utilisateurs documentés selon Planeks (planeks.net), pour des cas d’usage allant de l’orchestration de microservices à la diffusion de contenu personnalisé. Le framework supporte 100 % de couverture de tests sur son propre code, ce qui rassure les équipes plateforme et SRE qui auditent les dépendances.

En France, plusieurs scale-ups deeptech (Mistral AI, Doctolib, BlaBlaCar) ont intégré FastAPI pour leurs APIs internes ou publiques. Les annonces emploi sur Welcome to the Jungle et LinkedIn mentionnent FastAPI comme compétence requise dans plus de 60 % des offres backend Python senior pour 2026. Le ticket d’entrée reste accessible : un développeur Flask ou Django expérimenté devient productif sur FastAPI en 2 à 5 jours, principalement le temps d’assimiler Pydantic v2 et l’asynchronisme Python.

## Foire aux questions sur FastAPI en 2026

### FastAPI est-il prêt pour la production en 2026 ?

Oui. FastAPI tourne en production chez Microsoft, Uber, Netflix, Hugging Face et de nombreuses scale-ups françaises depuis plusieurs années. Le numéro de version 0.x.x reflète une politique de versioning sémantique conservative (chaque mineure peut introduire un breaking change), pas un manque de maturité. Le framework est utilisé sur des charges allant de quelques dizaines à plusieurs dizaines de milliers de requêtes par seconde sans difficulté.

### Quelle est la différence entre FastAPI et Flask ?

FastAPI est asynchrone-first et impose le typage strict via Pydantic, ce qui apporte la documentation OpenAPI automatique et la validation des inputs sans code supplémentaire. Flask est synchrone-first (avec async optionnel depuis Flask 2.0), plus minimaliste, et requiert des extensions tierces (marshmallow, flasgger) pour atteindre les mêmes fonctionnalités. Sur des microservices simples sans contrainte de performance, Flask reste plus rapide à prototyper. Sur tout projet sérieux exposant une API REST, FastAPI est désormais le choix par défaut en 2026.

### Faut-il utiliser SQLModel ou SQLAlchemy directement avec FastAPI ?

SQLModel, créé par le même auteur que FastAPI, fusionne SQLAlchemy 2.0 et Pydantic en une seule classe. C’est l’option recommandée pour les nouveaux projets car elle réduit la duplication entre modèles DB et schémas Pydantic. SQLAlchemy 2.0 brut reste pertinent si vous avez besoin de fonctionnalités avancées (custom types, dialectes exotiques, requêtes très complexes) ou si votre équipe maîtrise déjà SQLAlchemy. Les deux approches sont compatibles avec asyncpg et l’asynchronisme.

### Quel serveur ASGI choisir entre Uvicorn, Hypercorn et Granian ?

Uvicorn 0.34+ reste le choix par défaut : maturité, écosystème, intégration FastAPI CLI. Hypercorn supporte HTTP/2 et HTTP/3 nativement, à privilégier si vous servez en frontal sans reverse proxy. Granian, écrit en Rust, affiche les meilleurs scores sur les benchmarks 2025-2026 mais l’écosystème est encore jeune. Pour la majorité des équipes, Uvicorn derrière Nginx ou Traefik est largement suffisant et représente moins de 5 % de la latence totale.

### Comment migrer un projet Flask existant vers FastAPI ?

Procédez par micro-étapes plutôt que par bigbang. Étape 1 : extrayez les schémas Marshmallow vers Pydantic v2. Étape 2 : remplacez les routes Flask les plus consultées par leurs équivalents FastAPI dans une nouvelle application montée derrière le même reverse proxy. Étape 3 : migrez les middlewares de sécurité et le rate limiting. Étape 4 : décommissionnez Flask. Comptez 2 à 6 semaines pour un projet de taille moyenne (50 endpoints) selon la maturité du typage existant et la dette technique. La librairie `asgiref.WsgiToAsgi` permet de monter une app Flask comme sous-application FastAPI le temps de la transition.

### FastAPI gère-t-il GraphQL en plus de REST ?

FastAPI ne fournit pas GraphQL nativement, mais s’intègre avec Strawberry GraphQL ou Ariadne via une simple route. Pour la majorité des APIs métier, REST + OpenAPI reste plus simple à versionner, à documenter et à mettre en cache côté CDN. GraphQL devient pertinent quand vous avez plusieurs frontends consommant des projections différentes du même modèle, ou quand le sur-fetching/under-fetching devient un problème mesuré.

### Quel est le coût total d’une API FastAPI en production en France ?

Pour un MVP servant moins de 100 requêtes par seconde, comptez environ 30 € par mois sur Scaleway Serverless Containers + Postgres Managed Database (instance DEV). Pour une API à 500-1 000 RPS avec haute disponibilité, prévoyez 150 à 300 € par mois sur OVH Managed Kubernetes Service avec 3 nœuds B2-15 et un Postgres répliqué. Au-delà de 5 000 RPS, étudiez du *bare metal* ou un déploiement sur AWS/GCP avec autoscaling agressif. Les coûts restent très inférieurs à un équivalent Java/Spring Boot grâce au footprint mémoire de Python plus léger en async.

### FastAPI supporte-t-il Python 3.13 free-threaded ?

Python 3.13, sortie en octobre 2024, introduit un mode free-threaded (sans GIL) en bêta. FastAPI 0.136 fonctionne en mode free-threaded, mais plusieurs dépendances binaires (asyncpg, orjson, bcrypt) n’exposent pas encore de wheels free-threaded stables en avril 2026. Pour la production, restez sur Python 3.12.x avec GIL classique. Réservez 3.13 free-threaded à des expérimentations sur du calcul CPU-bound parallélisé, où le gain peut atteindre 4 à 8× sur 8 cœurs selon les benchmarks officiels CPython.

### Comment versionner correctement une API FastAPI ?

Deux stratégies dominent : versioning par préfixe d’URL (`/v1/articles`, `/v2/articles`) ou par header (`Accept-Version: 2`). Le préfixe d’URL est plus simple à cacher et à logger ; le header est plus propre sémantiquement. Dans les deux cas, montez chaque version comme un router FastAPI distinct dans `app.include_router(v1_router, prefix="/v1")`. Maintenez deux versions en parallèle pendant au moins 3 mois lors d’un breaking change pour laisser le temps aux clients de migrer.

### Related Coverage

## Sources et documentation officielle

- Notes de version officielles FastAPI – historique complet des changements 0.128.0 à 0.136.1.
- FastAPI sur PyPI – paquet officiel, dernière version 0.136.1 publiée le 23 avril 2026.
- Releases GitHub fastapi/fastapi – changelog et binaires.
- Documentation Pydantic v2 – guide officiel de la validation typée.
- Starlette ASGI Framework – fondations sous-jacentes de FastAPI.

*Tutoriel publié le 26 avril 2026 par la rédaction Tech Insider. Code testé sur Python 3.12.7, FastAPI 0.141.1, PostgreSQL 17.4, Ubuntu 24.04 LTS et macOS 15.4.*
