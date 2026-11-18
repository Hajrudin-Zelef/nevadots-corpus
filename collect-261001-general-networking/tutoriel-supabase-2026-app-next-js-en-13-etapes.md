**Tutoriel Supabase 2026** – Avec plus de **101 948 étoiles GitHub**, **12 315 forks** et la version **@supabase/supabase-js 2.105.3** publiée sur npm – toujours la branche v2.x que la documentation officielle de la CLI Supabase, actualisée en août 2026, désigne comme SDK par défaut –, Supabase s’est imposé en France comme la principale alternative open-source à Firebase. La plateforme combine une base PostgreSQL gérée, une authentification multi-fournisseurs, du stockage S3-compatible, du temps réel via WebSockets et des Edge Functions Deno – le tout exposé par une API REST auto-générée. Ce guide pratique de 13 étapes vous montre comment construire une application complète Next.js 16 + Supabase, déployable en production, en moins de 90 minutes.

*Publié le 06 avril 2026 – Tutoriel testé sur Ubuntu 24.04 LTS, macOS 15 Sequoia et Windows 11 24H2 avec Node.js 22 LTS.*

## Pourquoi choisir Supabase en 2026 : un BaaS open-source dominant

Supabase n’est plus une promesse : c’est devenu en 2026 le **backend-as-a-service open-source** le plus adopté par les développeurs français et européens. Là où Firebase impose un modèle propriétaire et un verrouillage Google Cloud, Supabase repose sur une stack 100 % open-source – PostgreSQL 16, GoTrue pour l’authentification, Realtime en Elixir, Storage en Go, et PostgREST pour exposer automatiquement la base sous forme d’API REST. Ce socle technique permet à un développeur de cloner toute son infrastructure en local avec la CLI Supabase, de l’auto-héberger sur Hetzner ou Scaleway pour respecter le RGPD, ou de migrer vers le cloud managé sans changer une ligne de code applicatif.

L’écosystème a explosé : la plateforme dépasse les **101 948 étoiles GitHub** en mai 2026, avec **12 315 forks** actifs et un rythme de contributions qui place le projet parmi les 30 premiers dépôts mondiaux. Côté financement, Supabase a levé plus de 120 millions de dollars cumulés depuis sa création, dont une série B de 80 millions menée par Felicis Ventures, valorisant la société à environ 2 milliards de dollars selon les dernières fuites du marché secondaire. Pour les équipes françaises, l’argument décisif reste la maîtrise des données : trois régions européennes – Francfort, Paris et Dublin – permettent de garder le trafic utilisateur sur le sol UE, condition indispensable pour les secteurs santé, finance et secteur public.

Ce tutoriel construit une application réelle : une plateforme de gestion de tickets support avec authentification email + magic link, table PostgreSQL avec Row Level Security, upload de pièces jointes vers Supabase Storage, mises à jour en temps réel via Realtime, et une Edge Function Deno qui envoie un email de notification. Le code complet est compatible avec Next.js 16.2.4 et React 19.2.6, déployable sur Vercel ou auto-hébergé via Docker.

## Prérequis : versions exactes pour suivre ce tutoriel sans erreur

Avant d’écrire la première ligne de code, vérifiez que votre poste de développement dispose des outils suivants. Toutes les versions ont été validées en avril 2026 sur les trois systèmes d’exploitation majeurs. Une version trop ancienne de Node.js ou de Docker provoquera des erreurs cryptiques sur les Edge Functions et la CLI Supabase.

| Outil | Version minimale | Version testée | Commande de vérification | 
|---|---|---|---|
| Node.js | 20.0.0 LTS | 22.14.0 LTS | `node --version` | 
| npm | 10.0.0 | 10.9.2 | `npm --version` | 
| Docker Desktop | 4.30 | 4.38.0 | `docker --version` | 
| Supabase CLI | 1.220.0 | 2.6.8 | `supabase --version` | 
| Git | 2.40 | 2.45.2 | `git --version` | 
| @supabase/supabase-js | 2.100.0 | 2.105.3 | `npm view @supabase/supabase-js version` | 
| @supabase/ssr | 0.10.0 | 0.10.2 | `npm view @supabase/ssr version` | 
| Next.js | 15.0.0 | 16.2.4 | `npx next --version` | 
| TypeScript | 5.4.0 | 6.0.3 | `npx tsc --version` | 
| Deno (Edge Functions) | 2.0 | 2.4.1 | `deno --version` | 

Côté compte, créez gratuitement un projet sur supabase.com/dashboard. Le tier Free offre 500 MB de base PostgreSQL, 1 GB de stockage, 50 000 utilisateurs actifs mensuels (MAU) et 2 projets simultanés – largement suffisant pour ce tutoriel. Si votre application dépasse ces seuils, le passage au tier Pro à 25 $ par projet et par mois débloque 8 GB de base, 100 GB de stockage et 500 000 invocations Edge Functions incluses, avec 10 $ de crédits compute offerts chaque mois.

## Étape 1 : Installer la CLI Supabase et initialiser le projet local

La CLI Supabase est le couteau suisse du développeur : elle clone un environnement complet – Postgres 16, GoTrue, Storage, Realtime, Studio, Inbucket pour les emails de test – dans des conteneurs Docker locaux. Ce mode local accélère drastiquement le cycle de développement : pas de latence réseau, pas de risque de polluer la base de production, et la possibilité de versionner toutes les migrations dans Git.

```
# macOS (Homebrew)
brew install supabase/tap/supabase
# Linux (npm global)
npm install -g supabase
# Windows (Scoop)
scoop bucket add supabase https://github.com/supabase/scoop-bucket.git
scoop install supabase
# Vérifier l'installation
supabase --version
# Sortie attendue : 2.6.8
# Créer le dossier projet
mkdir tickets-support && cd tickets-support
supabase init
# La CLI génère :
# supabase/config.toml      → configuration locale
# supabase/migrations/      → migrations SQL versionnées
# supabase/seed.sql         → données de test
# supabase/.gitignore       → exclusions Git
```
La commande `supabase init` crée un dossier `supabase/` à la racine du projet. Le fichier `config.toml` contient les ports utilisés par chaque service local – par défaut 54321 pour l’API, 54322 pour Postgres, 54323 pour Studio. Si l’un de ces ports est déjà occupé sur votre poste, modifiez-les dans le fichier de configuration avant de démarrer la stack.

## Étape 2 : Démarrer la stack Supabase locale en Docker

Avec Docker Desktop lancé en arrière-plan, exécutez `supabase start`. Le premier lancement télécharge environ 2 GB d’images Docker – comptez entre 3 et 8 minutes selon votre connexion. Les lancements suivants prennent moins de 30 secondes grâce au cache Docker.

```
supabase start
# Sortie attendue (extrait)
Started supabase local development setup.
         API URL: http://127.0.0.1:54321
     GraphQL URL: http://127.0.0.1:54321/graphql/v1
  S3 Storage URL: http://127.0.0.1:54321/storage/v1/s3
          DB URL: postgresql://postgres:[email protected]:54322/postgres
      Studio URL: http://127.0.0.1:54323
    Inbucket URL: http://127.0.0.1:54324
      JWT secret: super-secret-jwt-token-with-at-least-32-characters-long
        anon key: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
service_role key: eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```
Notez précieusement les clés `anon key` et `service_role key`. La première sera exposée côté navigateur – elle respecte les politiques RLS. La seconde donne un accès root à la base : elle ne doit JAMAIS apparaître dans le code client. Ouvrez http://127.0.0.1:54323 pour accéder à Supabase Studio en local : l’interface graphique pour gérer tables, politiques RLS, utilisateurs, fonctions et logs.

## Étape 3 : Concevoir le schéma PostgreSQL avec Row Level Security

Supabase repose sur PostgreSQL 16, qui supporte plus de 150 extensions parmi lesquelles pgvector pour l’IA, pg_cron pour les tâches planifiées, postgis pour la géolocalisation, et pg_trgm pour la recherche fuzzy. Pour notre application de tickets, nous avons besoin de deux tables – `tickets` et `messages` – liées par une clé étrangère, avec Row Level Security activée pour garantir qu’un utilisateur ne voit que ses propres données.

Créez votre première migration via la CLI :

`supabase migration new create_tickets_schema`
Éditez le fichier généré dans `supabase/migrations/` avec le SQL suivant :

```
-- supabase/migrations/20260406120000_create_tickets_schema.sql
-- Table tickets
create table public.tickets (
  id uuid primary key default gen_random_uuid(),
  user_id uuid references auth.users(id) on delete cascade not null,
  title text not null check (length(title) between 3 and 200),
  description text,
  status text not null default 'open' check (status in ('open','pending','closed')),
  priority text not null default 'medium' check (priority in ('low','medium','high','urgent')),
  attachment_path text,
  created_at timestamptz default now() not null,
  updated_at timestamptz default now() not null
);
-- Table messages
create table public.messages (
  id uuid primary key default gen_random_uuid(),
  ticket_id uuid references public.tickets(id) on delete cascade not null,
  user_id uuid references auth.users(id) on delete cascade not null,
  body text not null,
  created_at timestamptz default now() not null
);
-- Indexes pour la performance
create index idx_tickets_user_id on public.tickets(user_id);
create index idx_tickets_status on public.tickets(status);
create index idx_messages_ticket_id on public.messages(ticket_id);
-- Activer Row Level Security
alter table public.tickets enable row level security;
alter table public.messages enable row level security;
-- Policies tickets : un utilisateur ne voit que ses propres tickets
create policy "Users can view own tickets" on public.tickets
  for select using (auth.uid() = user_id);
create policy "Users can insert own tickets" on public.tickets
  for insert with check (auth.uid() = user_id);
create policy "Users can update own tickets" on public.tickets
  for update using (auth.uid() = user_id);
create policy "Users can delete own tickets" on public.tickets
  for delete using (auth.uid() = user_id);
-- Policies messages : seulement messages de ses propres tickets
create policy "Users view messages of own tickets" on public.messages
  for select using (
    exists (
      select 1 from public.tickets
      where tickets.id = messages.ticket_id and tickets.user_id = auth.uid()
    )
  );
create policy "Users insert messages on own tickets" on public.messages
  for insert with check (
    auth.uid() = user_id and
    exists (
      select 1 from public.tickets
      where tickets.id = ticket_id and tickets.user_id = auth.uid()
    )
  );
-- Trigger updated_at automatique
create or replace function public.update_updated_at()
returns trigger as $$
begin
  new.updated_at = now();
  return new;
end;
$$ language plpgsql;
create trigger set_updated_at
  before update on public.tickets
  for each row execute function public.update_updated_at();
```
Appliquez la migration :

```
supabase db reset
# Sortie attendue
Resetting local database...
Recreating database...
Initialising schema...
Applying migration 20260406120000_create_tickets_schema.sql...
Seeding data supabase/seed.sql...
Finished supabase db reset on branch main.
```
La commande `supabase db reset` détruit et recrée toute la base locale, applique chaque migration dans l’ordre alphabétique, puis exécute le fichier `seed.sql` pour insérer des données de test. Cette commande est destructrice : ne l’utilisez jamais sur la production.

## Étape 4 : Créer le projet Next.js 16 avec App Router et TypeScript

Next.js 16 introduit le compilateur Turbopack stable et un App Router optimisé pour les Server Components React 19. Cette combinaison réduit drastiquement la quantité de JavaScript envoyée au navigateur – un avantage décisif pour une application Supabase qui peut faire la majorité de ses requêtes côté serveur.

```
cd tickets-support
# Créer l'app Next.js dans un sous-dossier
npx create-next-app@latest web \
  --typescript \
  --tailwind \
  --eslint \
  --app \
  --src-dir \
  --turbopack \
  --import-alias "@/*"
cd web
# Installer les SDK Supabase
npm install @supabase/[email protected] @supabase/[email protected]
# Vérifier
npm list @supabase/supabase-js @supabase/ssr
```
Le package `@supabase/ssr` remplace l’ancien `@supabase/auth-helpers-nextjs` (déprécié depuis 2024) et gère correctement les cookies de session entre Server Components, Route Handlers, Server Actions et Client Components – un piège classique qui causait des fuites de session avant cette refonte.

## Étape 5 : Configurer les variables d’environnement et le client Supabase

Créez le fichier `web/.env.local` avec les clés issues de `supabase status` (ou de votre projet cloud) :

```
# web/.env.local
NEXT_PUBLIC_SUPABASE_URL=http://127.0.0.1:54321
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```
Ajoutez immédiatement `.env.local` à `.gitignore` – Next.js le fait par défaut, mais vérifiez. Créez ensuite trois utilitaires de client Supabase, un par contexte d’exécution :

```
// src/lib/supabase/client.ts
import { createBrowserClient } from '@supabase/ssr'
export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  )
}
```
```
// src/lib/supabase/server.ts
import { createServerClient } from '@supabase/ssr'
import { cookies } from 'next/headers'
export async function createClient() {
  const cookieStore = await cookies()
  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return cookieStore.getAll()
        },
        setAll(cookiesToSet) {
          try {
            cookiesToSet.forEach(({ name, value, options }) =>
              cookieStore.set(name, value, options)
            )
          } catch {
            // Ignoré dans Server Component, géré par middleware
          }
        },
      },
    }
  )
}
```
```
// src/lib/supabase/middleware.ts
import { createServerClient } from '@supabase/ssr'
import { NextResponse, type NextRequest } from 'next/server'
export async function updateSession(request: NextRequest) {
  let response = NextResponse.next({ request })
  const supabase = createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return request.cookies.getAll()
        },
        setAll(cookiesToSet) {
          cookiesToSet.forEach(({ name, value }) =>
            request.cookies.set(name, value)
          )
          response = NextResponse.next({ request })
          cookiesToSet.forEach(({ name, value, options }) =>
            response.cookies.set(name, value, options)
          )
        },
      },
    }
  )
  const { data: { user } } = await supabase.auth.getUser()
  // Redirection si non authentifié sur routes protégées
  if (!user && request.nextUrl.pathname.startsWith('/app')) {
    const url = request.nextUrl.clone()
    url.pathname = '/login'
    return NextResponse.redirect(url)
  }
  return response
}
```
Créez ensuite `src/middleware.ts` à la racine de `src/` qui appelle `updateSession()` sur chaque requête. Ce middleware rafraîchit automatiquement le JWT toutes les heures sans intervention manuelle – un point critique pour éviter les déconnexions intempestives.

## Étape 6 : Implémenter l’authentification avec Magic Link et OAuth

Supabase Auth, basé sur GoTrue (un fork open-source de Netlify Identity, dont la documentation d’auto-hébergement de février 2026 pin désormais l’image de production à la version 2.186.0), supporte plus de 20 fournisseurs OAuth – Google, GitHub, Apple, Discord, Azure AD, Facebook, Spotify, LinkedIn, GitLab, Bitbucket, Twitter/X, Notion, Slack, Twitch, Zoom, Figma, Keycloak, Kakao, Workos, et tout fournisseur OIDC ou SAML 2.0 personnalisé. Pour ce tutoriel, nous combinons l’authentification par **Magic Link** (mot de passe sans mot de passe via email) et OAuth GitHub.

```
// src/app/login/page.tsx
'use client'
import { useState } from 'react'
import { createClient } from '@/lib/supabase/client'
export default function LoginPage() {
  const [email, setEmail] = useState('')
  const [sent, setSent] = useState(false)
  const [error, setError] = useState<string | null>(null)
  const supabase = createClient()
  async function handleMagicLink(e: React.FormEvent) {
    e.preventDefault()
    setError(null)
    const { error } = await supabase.auth.signInWithOtp({
      email,
      options: {
        emailRedirectTo: `${location.origin}/auth/callback`,
      },
    })
    if (error) setError(error.message)
    else setSent(true)
  }
  async function handleGitHub() {
    await supabase.auth.signInWithOAuth({
      provider: 'github',
      options: { redirectTo: `${location.origin}/auth/callback` },
    })
  }
  if (sent) return <p>Vérifiez votre boîte mail !</p>
  return (
    <div className="max-w-sm mx-auto p-6">
      <h1 className="text-2xl font-bold mb-4">Connexion</h1>
      <form onSubmit={handleMagicLink} className="space-y-3">
        <input
          type="email"
          required
          value={email}
          onChange={(e) => setEmail(e.target.value)}
          placeholder="[email protected]"
          className="w-full px-3 py-2 border rounded"
        />
        <button type="submit" className="w-full bg-blue-600 text-white py-2 rounded">
          Envoyer le lien magique
        </button>
      </form>
      <button onClick={handleGitHub} className="w-full mt-3 bg-gray-900 text-white py-2 rounded">
        Connexion avec GitHub
      </button>
      {error && <p className="text-red-600 mt-2">{error}</p>}
    </div>
  )
}
```
Créez le route handler de callback OAuth qui échange le code contre une session :

```
// src/app/auth/callback/route.ts
import { createClient } from '@/lib/supabase/server'
import { NextResponse } from 'next/server'
export async function GET(request: Request) {
  const { searchParams, origin } = new URL(request.url)
  const code = searchParams.get('code')
  const next = searchParams.get('next') ?? '/app'
  if (code) {
    const supabase = await createClient()
    const { error } = await supabase.auth.exchangeCodeForSession(code)
    if (!error) return NextResponse.redirect(`${origin}${next}`)
  }
  return NextResponse.redirect(`${origin}/login?error=auth_callback`)
}
```
En local, les emails de Magic Link arrivent dans **Inbucket** sur http://127.0.0.1:54324 – aucun envoi réel. Pour la production, configurez un fournisseur SMTP (Resend, Postmark, SendGrid) dans `config.toml` ou via le dashboard Supabase, sous peine de quota strict de 4 emails par heure sur le service par défaut.

## Étape 7 : Lister, créer et modifier des tickets via Server Actions

Les Server Actions de Next.js 16 simplifient drastiquement les mutations : plus besoin d’écrire un endpoint API REST séparé. La fonction tourne sur le serveur, peut accéder directement au client Supabase server-side, et invalide automatiquement le cache du Router via `revalidatePath()`.

```
// src/app/app/actions.ts
'use server'
import { createClient } from '@/lib/supabase/server'
import { revalidatePath } from 'next/cache'
import { redirect } from 'next/navigation'
export async function createTicket(formData: FormData) {
  const supabase = await createClient()
  const { data: { user } } = await supabase.auth.getUser()
  if (!user) redirect('/login')
  const title = String(formData.get('title') ?? '').trim()
  const description = String(formData.get('description') ?? '').trim()
  const priority = String(formData.get('priority') ?? 'medium')
  if (title.length < 3) return { error: 'Titre trop court (min 3 caractères)' }
  const { error } = await supabase.from('tickets').insert({
    user_id: user.id,
    title,
    description,
    priority,
  })
  if (error) return { error: error.message }
  revalidatePath('/app')
  return { success: true }
}
export async function updateTicketStatus(id: string, status: string) {
  const supabase = await createClient()
  const { error } = await supabase
    .from('tickets')
    .update({ status })
    .eq('id', id)
  if (error) return { error: error.message }
  revalidatePath('/app')
  revalidatePath(`/app/${id}`)
  return { success: true }
}
```
La page liste utilise un Server Component qui charge les tickets directement depuis PostgreSQL – Row Level Security garantit que seuls les tickets de l’utilisateur connecté sont retournés, sans aucune logique de filtrage côté application :

```
// src/app/app/page.tsx
import { createClient } from '@/lib/supabase/server'
import Link from 'next/link'
import { createTicket } from './actions'
export default async function TicketsPage() {
  const supabase = await createClient()
  const { data: tickets } = await supabase
    .from('tickets')
    .select('id, title, status, priority, created_at')
    .order('created_at', { ascending: false })
    .limit(50)
  return (
    <main className="max-w-3xl mx-auto p-6">
      <h1 className="text-3xl font-bold">Mes tickets</h1>
      <form action={createTicket} className="mt-6 space-y-2 border p-4 rounded">
        <input name="title" placeholder="Titre" required className="w-full border p-2 rounded" />
        <textarea name="description" placeholder="Description" className="w-full border p-2 rounded" />
        <select name="priority" className="w-full border p-2 rounded">
          <option value="low">Faible</option>
          <option value="medium" selected>Moyenne</option>
          <option value="high">Haute</option>
          <option value="urgent">Urgente</option>
        </select>
        <button className="bg-blue-600 text-white px-4 py-2 rounded">Créer</button>
      </form>
      <ul className="mt-6 space-y-2">
        {tickets?.map(t => (
          <li key={t.id} className="border p-3 rounded">
            <Link href={`/app/${t.id}`} className="font-semibold">{t.title}</Link>
            <span className="ml-2 text-sm text-gray-500">{t.status} · {t.priority}</span>
          </li>
        ))}
      </ul>
    </main>
  )
}
```
## Étape 8 : Uploader des pièces jointes vers Supabase Storage

Supabase Storage est compatible avec l’API S3 d’Amazon, avec un bucket dédié par usage et des politiques d’accès basées sur les mêmes règles RLS que la base. Le tier Free offre 1 GB de stockage et 2 GB de bande passante mensuelle ; le Pro monte à 100 GB de stockage et 250 GB de bande passante incluse.

Créez un bucket privé via une migration SQL :

```
-- supabase/migrations/20260406130000_storage_attachments.sql
insert into storage.buckets (id, name, public, file_size_limit, allowed_mime_types)
values (
  'ticket-attachments',
  'ticket-attachments',
  false,
  10485760,  -- 10 MB max par fichier
  array['image/png','image/jpeg','image/webp','application/pdf']
)
on conflict (id) do nothing;
-- Policy : un utilisateur ne lit que ses propres uploads
create policy "Users read own attachments" on storage.objects
  for select using (
    bucket_id = 'ticket-attachments' and
    (storage.foldername(name))[1] = auth.uid()::text
  );
create policy "Users upload to own folder" on storage.objects
  for insert with check (
    bucket_id = 'ticket-attachments' and
    (storage.foldername(name))[1] = auth.uid()::text
  );
```
Côté client, créez un composant d’upload qui place chaque fichier dans le dossier `{user_id}/{ticket_id}/` :

```
// src/components/UploadAttachment.tsx
'use client'
import { useState } from 'react'
import { createClient } from '@/lib/supabase/client'
export function UploadAttachment({ ticketId }: { ticketId: string }) {
  const [uploading, setUploading] = useState(false)
  const supabase = createClient()
  async function handleUpload(e: React.ChangeEvent<HTMLInputElement>) {
    const file = e.target.files?.[0]
    if (!file) return
    setUploading(true)
    const { data: { user } } = await supabase.auth.getUser()
    if (!user) return
    const path = `${user.id}/${ticketId}/${Date.now()}-${file.name}`
    const { error } = await supabase.storage
      .from('ticket-attachments')
      .upload(path, file, { cacheControl: '3600', upsert: false })
    if (error) alert(error.message)
    else {
      await supabase.from('tickets').update({ attachment_path: path }).eq('id', ticketId)
      location.reload()
    }
    setUploading(false)
  }
  return (
    <input
      type="file"
      accept="image/*,application/pdf"
      onChange={handleUpload}
      disabled={uploading}
    />
  )
}
```
Pour afficher la pièce jointe, générez une URL signée temporaire – jamais d’URL publique sur un bucket privé :

```
const { data } = await supabase.storage
  .from('ticket-attachments')
  .createSignedUrl(path, 3600)  // valide 1 heure
```
## Étape 9 : Activer la mise à jour temps réel via Realtime

Supabase Realtime, écrit en Elixir et basé sur Phoenix Channels, propage les changements PostgreSQL via WebSockets – utile pour afficher de nouveaux messages instantanément. Le tier Free supporte 200 connexions simultanées et 2 millions de messages par mois ; le tier Pro monte à 500 connexions et 5 millions de messages inclus.

Activez la publication Realtime sur les tables concernées :

```
-- supabase/migrations/20260406140000_enable_realtime.sql
alter publication supabase_realtime add table public.messages;
alter publication supabase_realtime add table public.tickets;
```
Côté React, abonnez-vous aux insertions sur la table `messages` :

```
// src/components/MessagesList.tsx
'use client'
import { useEffect, useState } from 'react'
import { createClient } from '@/lib/supabase/client'
type Message = { id: string; body: string; created_at: string; user_id: string }
export function MessagesList({ ticketId, initial }: { ticketId: string; initial: Message[] }) {
  const [messages, setMessages] = useState(initial)
  const supabase = createClient()
  useEffect(() => {
    const channel = supabase
      .channel(`messages:${ticketId}`)
      .on(
        'postgres_changes',
        {
          event: 'INSERT',
          schema: 'public',
          table: 'messages',
          filter: `ticket_id=eq.${ticketId}`,
        },
        (payload) => {
          setMessages((prev) => [...prev, payload.new as Message])
        }
      )
      .subscribe()
    return () => { supabase.removeChannel(channel) }
  }, [ticketId])
  return (
    <ul className="space-y-2">
      {messages.map((m) => (
        <li key={m.id} className="border p-2 rounded">
          <p>{m.body}</p>
          <time className="text-xs text-gray-500">{new Date(m.created_at).toLocaleString('fr-FR')}</time>
        </li>
      ))}
    </ul>
  )
}
```
Important : les politiques RLS s’appliquent aussi aux événements Realtime depuis la version 2024 – un utilisateur ne reçoit que les changements qu’il aurait pu lire via une requête classique. Vérifiez que vos policies SELECT sont correctes, sous peine de manquer des notifications légitimes.

## Étape 10 : Écrire une Edge Function Deno qui envoie un email

Les Edge Functions Supabase tournent sur Deno 2.4.1, déployées dans plus de 30 régions edge à travers le monde – latence moyenne sous 50 ms en Europe. Le tier Free inclut 500 000 invocations par mois ; le Pro 2 millions, avec un démarrage à froid sous 100 ms grâce à V8 Isolates.

`supabase functions new notify-new-ticket`
Éditez la fonction générée pour appeler l’API Resend :

```
// supabase/functions/notify-new-ticket/index.ts
import "jsr:@supabase/functions-js/edge-runtime.d.ts"
interface TicketWebhook {
  type: 'INSERT'
  table: 'tickets'
  record: {
    id: string
    title: string
    user_id: string
    priority: string
  }
}
Deno.serve(async (req: Request) => {
  if (req.method !== 'POST') {
    return new Response('Method not allowed', { status: 405 })
  }
  const payload: TicketWebhook = await req.json()
  const { title, priority, user_id } = payload.record
  const resendKey = Deno.env.get('RESEND_API_KEY')
  if (!resendKey) {
    return new Response('Missing RESEND_API_KEY', { status: 500 })
  }
  const res = await fetch('https://api.resend.com/emails', {
    method: 'POST',
    headers: {
      Authorization: `Bearer ${resendKey}`,
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({
      from: '[email protected]',
      to: '[email protected]',
      subject: `[${priority.toUpperCase()}] Nouveau ticket : ${title}`,
      text: `Un nouveau ticket a été créé par ${user_id}.\nTitre : ${title}`,
    }),
  })
  if (!res.ok) {
    return new Response('Resend API error', { status: 502 })
  }
  return new Response(JSON.stringify({ ok: true }), {
    headers: { 'Content-Type': 'application/json' },
  })
})
```
Déployez la fonction et configurez le secret :

```
# Servir en local
supabase functions serve notify-new-ticket --env-file ./supabase/functions/.env
# Déployer en production
supabase secrets set RESEND_API_KEY=re_votre_cle_ici
supabase functions deploy notify-new-ticket --no-verify-jwt
```
Branchez ensuite un Database Webhook dans le dashboard Supabase : *Database → Webhooks → Create*, table `tickets`, événement `INSERT`, target l’URL de votre fonction. À chaque création de ticket, l’email partira automatiquement.

## Étape 11 : Lier le projet local au cloud et déployer en production

Connectez votre projet local à votre projet cloud Supabase et appliquez toutes les migrations en une commande :

```
# Récupérer la liste des projets
supabase projects list
# Lier le projet (remplacez REF par votre Project Ref)
supabase link --project-ref abcdefghijklmnopqrst
# Pousser toutes les migrations locales vers la prod
supabase db push
# Sortie attendue
Connecting to remote database...
Applying migration 20260406120000_create_tickets_schema.sql...
Applying migration 20260406130000_storage_attachments.sql...
Applying migration 20260406140000_enable_realtime.sql...
Finished supabase db push.
```
Mettez à jour `.env.local` avec les clés de production récupérées dans *Project Settings → API*, puis déployez le front Next.js sur Vercel :

```
cd web
npm install -g vercel
vercel --prod
# Configurez les variables d'environnement dans le dashboard Vercel :
# NEXT_PUBLIC_SUPABASE_URL
# NEXT_PUBLIC_SUPABASE_ANON_KEY
# SUPABASE_SERVICE_ROLE_KEY (pour usages server-only)
```
Pensez à ajouter votre domaine Vercel à la liste des **Redirect URLs** autorisées dans *Authentication → URL Configuration* du dashboard Supabase, sous peine d’échec de tous les callbacks OAuth en production.

## Étape 12 : Branching pour les pull requests et workflows CI/CD

Le **Branching** Supabase, désormais inclus sur tous les plans depuis mai 2026, crée automatiquement une base PostgreSQL éphémère pour chaque pull request GitHub. Les migrations s’appliquent à la branche, les tests d’intégration tournent sur des données isolées, et la branche est détruite à la fusion ou à la fermeture de la PR.

Activez le Branching dans le dashboard, puis ajoutez ce workflow GitHub Actions :

```
# .github/workflows/supabase.yml
name: Supabase CI
on:
  pull_request:
    paths: ['supabase/**', 'web/**']
  push:
    branches: [main]
jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: supabase/setup-cli@v1
        with:
          version: 2.6.8
      - run: supabase db lint
      - run: supabase test db
  deploy:
    needs: test
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: supabase/setup-cli@v1
      - run: supabase link --project-ref ${{ secrets.SUPABASE_PROJECT_REF }}
      - run: supabase db push
      - run: supabase functions deploy --project-ref ${{ secrets.SUPABASE_PROJECT_REF }}
        env:
          SUPABASE_ACCESS_TOKEN: ${{ secrets.SUPABASE_ACCESS_TOKEN }}
```
Ce pipeline garantit qu’aucune migration cassée n’atteint la production : `supabase db lint` détecte les SQL non-portables, `supabase test db` exécute pgTAP sur les fonctions stockées et politiques RLS.

## Étape 13 : Observabilité, logs et performance en production

Le dashboard Supabase intègre, depuis la généralisation de l’éditeur SQL inline dans Supabase Studio entre le 10 et le 24 février 2025, des logs détaillés inspectables avec un panneau latéral, des rapports SQL personnalisables via SQL Snippets, et un AI Assistant capable de réécrire les requêtes lentes. La vue **Reports** agrège les métriques par endpoint API, latence p95, taille moyenne des réponses et nombre d’erreurs 4xx/5xx – l’équivalent d’un Datadog basique sans coût supplémentaire.

Pour le monitoring approfondi, exportez les logs vers Logflare, Datadog ou Grafana Loki via le connecteur natif. La métrique critique à surveiller reste le **compute utilization** de l’instance Postgres – au-delà de 80 % soutenu, planifiez le passage à un compute add-on (Small : 25 $/mois, Large : 110 $/mois, 16XL : 3 730 $/mois). Le tier Free utilise des shared compute units : leur limite stricte explique l’erreur `compute usage exceeded` que rencontrent les projets en croissance trop rapide.

| Plan | Prix mensuel | Database | Storage | MAU inclus | Edge Functions | Realtime connexions | 
|---|---|---|---|---|---|---|
| Free | 0 $ | 500 MB | 1 GB | 50 000 | 500 000 invocations | 200 | 
| Pro | 25 $/projet | 8 GB | 100 GB | 100 000 (puis 0,00325 $/MAU) | 2 millions invocations | 500 | 
| Team | 599 $/projet | 100 GB+ (custom) | 500 GB+ | 100 000 (puis 0,00325 $/MAU) | 2 millions invocations + SSO | 10 000+ | 
| Enterprise | Sur devis | Illimitée | Illimité | Illimité | Custom | Custom | 

## Pièges courants à éviter avec Supabase en production

Au-delà du tutoriel pas-à-pas, voici les erreurs les plus fréquentes que rencontrent les équipes françaises lors des premiers déploiements Supabase. Chacune a été observée plusieurs fois sur des projets clients en 2025-2026.

- **Exposer la service_role key côté client** : cette clé contourne RLS. Elle ne doit JAMAIS apparaître dans`NEXT_PUBLIC_*` , dans le code envoyé au navigateur, ni dans un dépôt public. Utilisez-la uniquement dans des Server Actions, Edge Functions ou jobs cron côté serveur.
- **Oublier d’activer RLS sur une table** : par défaut, sans RLS, la clé anonyme donne accès complet à la table – fuite de données garantie. Activez systématiquement`alter table … enable row level security` et créez au moins une policy.
- **Confondre `auth.users` et table publique** : ne stockez jamais de profil utilisateur directement dans`auth.users` . Créez une table`public.profiles` avec une clé étrangère vers`auth.users(id)` et un trigger sur`auth.users` pour la peupler automatiquement.
- **Migrations non versionnées** : modifier le schéma directement dans Studio ne génère pas de migration. Utilisez`supabase db diff` pour capturer les changements ou éditez systématiquement les fichiers SQL.
- **Connexions Postgres saturées** : le tier Free limite à 60 connexions directes. Pour Next.js Serverless, utilisez le pooler PgBouncer sur le port 6543 (mode Transaction), pas le port 5432 direct.
- **Realtime sans publication activée** : oublier`alter publication supabase_realtime add table …` donne un canal silencieux qui ne reçoit jamais d’événement.
- **Bucket Storage public par erreur** : un bucket marqué`public: true` expose tous les fichiers via URL devinable. Préférez`public: false` + URL signées.
- **Magic Link rate limit** : Supabase bloque à 4 emails/heure par IP en local. En production, configurez impérativement un SMTP externe.

## Dépannage : 8 erreurs Supabase et comment les résoudre

| Erreur | Cause probable | Solution | 
|---|---|---|
| `Cannot read properties of undefined (reading 'cookies')` | Mauvais client Supabase (browser dans Server Component) | Importer `createClient` depuis`@/lib/supabase/server` | 
| `JWT expired` | Middleware non configuré ou non exécuté sur la route | Vérifier `matcher` dans`middleware.ts` | 
| `new row violates row-level security policy` | Policy INSERT manquante ou condition incorrecte | Ajouter `create policy … for insert with check (auth.uid() = user_id)` | 
| `relation "tickets" does not exist` | Migration non appliquée sur l’environnement courant | `supabase db reset` en local ou`supabase db push` en prod | 
| `Auth session missing!` | Cookies non transmis (CORS ou domaine cookie incorrect) | Vérifier les Redirect URLs dans le dashboard Supabase | 
| `Connection refused on port 54322` | Stack Supabase non démarrée | Lancer `supabase start` et attendre la fin du pull | 
| `Storage bucket not found` | Bucket non créé sur l’environnement cible | Appliquer la migration storage ou créer le bucket via Studio | 
| `Failed to fetch realtime channel` | WebSocket bloqué par firewall ou table non publiée | Vérifier publication + autoriser `wss://` sur le proxy | 

## Astuces avancées pour passer à l’échelle

### Optimiser les requêtes avec PostgREST embedding

L’API REST auto-générée par PostgREST, épinglé en version 14.5 dans les images de production Supabase depuis la mise à jour d’auto-hébergement de février 2026 (déployée mondialement depuis décembre 2025), supporte le *resource embedding* : récupérez tickets et messages associés en une seule requête HTTP au lieu de N+1.

```
const { data } = await supabase
  .from('tickets')
  .select('id, title, status, messages(id, body, created_at)')
  .eq('id', ticketId)
  .single()
```
### Cache HTTP avec révalidation

Sur Next.js 16, ajoutez `export const revalidate = 60` en tête d’un Server Component pour mettre en cache les réponses Supabase pendant 60 secondes au CDN – diminution massive des coûts compute.

### pgvector pour la recherche sémantique IA

L’extension pgvector, passée de la version 0.28.1 à 0.53.0 lors de la mise à jour des images Supabase de février 2026, est activable en une commande SQL : `create extension vector;`. Elle permet de stocker des embeddings (OpenAI, Mistral, Cohere) dans une colonne `vector(1536)` et d’effectuer des recherches par cosine distance avec un index HNSW. Combinée à RLS, c’est une alternative crédible à Pinecone ou Weaviate, avec un coût quasi nul en dessous de 1 million de vecteurs.

## Comparaison Supabase vs Firebase : la décision en 2026

| Critère | Supabase | Firebase | 
|---|---|---|
| Base de données | PostgreSQL 16 (relationnelle, SQL standard) | Firestore (NoSQL document) | 
| Modèle de licence | Open-source (Apache 2.0) | Propriétaire Google Cloud | 
| Self-hosting | Oui, Docker complet | Non possible | 
| Régions UE | Francfort, Paris, Dublin | europe-west1, europe-west3 | 
| Tarification entrée | Free puis 25 $/mois Pro | Spark gratuit puis Blaze pay-as-you-go | 
| Vendor lock-in | Faible (PostgreSQL standard) | Élevé (API propriétaire Firestore) | 
| SDKs officiels | JS, Swift, Kotlin, Flutter, Python | JS, Swift, Kotlin, Flutter, C++, Unity | 
| Conformité RGPD | Native, DPA standard | DPA Google + transferts US | 

Pour une analyse complète des deux plateformes, consultez notre comparatif détaillé Supabase vs Firebase 2026 ci-dessous dans la section Related Coverage. La tendance lourde en 2026 : les startups françaises levant en série A migrent massivement de Firestore vers Supabase pour la souveraineté des données et la sortie de l’écosystème Google Cloud.

## FAQ : Supabase pour les développeurs francophones

### Supabase est-il vraiment gratuit pour un projet de production ?

Oui, le tier Free est exploitable en production pour de petits projets : 50 000 utilisateurs actifs mensuels, 500 MB de base PostgreSQL, 1 GB de stockage, 2 GB de bande passante. Limitation principale : les projets sont mis en pause après 7 jours d’inactivité (aucun trafic), et il faut les réveiller manuellement depuis le dashboard. Pour de la production sérieuse, comptez sur le tier Pro à 25 $ par mois et par projet.

### Peut-on auto-héberger Supabase pour respecter la souveraineté des données ?

Oui, intégralement. Le dépôt github.com/supabase/supabase fournit un docker-compose complet avec tous les services. La documentation officielle couvre Hetzner, Scaleway, AWS, GCP et Azure. Les serveurs français comme OVHcloud Public Cloud ou Scaleway Elements sont parfaitement supportés. Comptez 8 à 16 GB de RAM minimum pour une stack production.

### Comment migrer une base existante PostgreSQL vers Supabase ?

Supabase fournit un outil `supabase db dump` et accepte les imports `pg_dump` standard. Pour une base de plus de 50 GB, utilisez la fonctionnalité **Migration via WAL** du dashboard avec un downtime minimal. Pensez à recréer les politiques RLS – elles ne sont pas exportées par défaut dans les dumps PostgreSQL.

### Quelles sont les alternatives à Supabase en 2026 ?

Les principales alternatives sont Firebase (Google), Appwrite (open-source self-hosted), PocketBase (lite Go), Convex (TypeScript-first), Pockethost et Nhost. Supabase reste leader sur le créneau open-source + PostgreSQL grâce à son écosystème npm très mature et la qualité de sa documentation, traduite partiellement en français depuis 2025.

### Supabase est-il compatible RGPD pour des données médicales ou financières ?

Supabase Cloud est certifié SOC 2 Type II et HIPAA-ready sur les tiers Team et Enterprise. Pour la France, le tier Enterprise propose un Data Processing Addendum (DPA) signable avec hébergement exclusif sur la région Paris (eu-west-3). Pour les données ultra-sensibles, l’auto-hébergement sur OVHcloud SecNumCloud reste l’option recommandée.

### Quelle est la différence entre @supabase/supabase-js et @supabase/ssr ?

Le SDK `@supabase/supabase-js` 2.105.3 est le client universel utilisable dans n’importe quel environnement JavaScript (browser, Node, Deno, Cloudflare Workers). Le package `@supabase/ssr` 0.10.2 est une couche d’abstraction spécifiquement pour Next.js, SvelteKit, Remix et Astro qui gère la synchronisation des cookies de session entre serveur et client – indispensable pour le rendu côté serveur authentifié.

### Comment gérer les coûts Supabase à grande échelle ?

Trois leviers : (1) activez **Spend Caps** dans le dashboard pour bloquer toute facturation au-delà d’un seuil ; (2) utilisez les Edge Functions plutôt que des appels DB directs depuis le client pour réduire le compute Postgres ; (3) externalisez les fichiers volumineux vers un CDN tiers (Cloudflare R2, Bunny CDN) tout en gardant les métadonnées dans Supabase Storage. Une équipe française typique reste sous 100 $/mois jusqu’à 100 000 utilisateurs actifs.

### Supabase remplace-t-il vraiment un backend Node.js ou Django ?

Pour 80 % des cas d’usage CRUD avec authentification, oui. La combinaison PostgREST + RLS + Edge Functions couvre l’essentiel. Pour des logiques métier complexes (workflows multi-étapes, intégrations B2B, calculs lourds), gardez un backend dédié en Node.js, Python ou Go qui consomme Supabase via la service_role key. Cette approche hybride est désormais standard chez les scaleups françaises.

## Conclusion : Supabase, le standard du backend français en 2026

Au terme de ce tutoriel, votre application de gestion de tickets dispose d’une authentification complète Magic Link + OAuth GitHub, d’une base PostgreSQL 16 sécurisée par Row Level Security, de stockage de pièces jointes via S3 compatible, de mises à jour temps réel WebSocket, d’une Edge Function Deno qui envoie des notifications email, et d’un pipeline GitHub Actions qui déploie tout automatiquement avec branching par PR. Vous avez écrit moins de 600 lignes de code TypeScript pour une application qui aurait demandé entre 3 000 et 5 000 lignes avec un backend custom Node.js + Express + Passport + multer + Socket.io.

Le pari de Supabase – combiner la robustesse du SQL relationnel et la productivité d’un BaaS moderne – est gagné en 2026. Avec 101 948 étoiles GitHub, une communauté hyper-active – illustrée par le tutoriel vidéo Supabase de 5 heures publié par freeCodeCamp en décembre 2025 et par les plus de 100 tutoriels pas-à-pas Supabase publiés par RapidDev depuis mars 2026 –, des prix prédictibles et un modèle 100 % open-source, c’est aujourd’hui le choix par défaut pour démarrer une application web ou mobile en France et en Europe. Pour aller plus loin, explorez les fonctionnalités Vault (gestion de secrets dans Postgres), Auth Hooks (logique custom sur signup), et l’intégration native Stripe pour les abonnements.

### Related Coverage

**Sources externes** : Documentation officielle Supabase · Tarifs Supabase · Dépôt GitHub Supabase · Documentation Next.js · Documentation PostgreSQL · Deno runtime