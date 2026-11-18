**Publié le 27 avril 2026 · Niveau : Intermédiaire · Durée : 60 minutes**

Playwright s’impose en 2026 comme la référence des tests bout-en-bout (E2E) pour les applications web modernes. Après la sortie de **Playwright 1.59** début avril 2026, Microsoft a enchaîné les mises à jour jusqu’au patch **v1.62.1** publié le 30 juillet 2026 (relevé par DevUpdate.io), consolidant une avance technique déjà confortable face à Cypress et Selenium : pilotage cross-browser (Chromium 141, Firefox 142, WebKit 26), test runner natif `@playwright/test`, mode UI interactif, Trace Viewer, et désormais l’API `page.screencast` pour enregistrer et annoter des sessions complètes. Signe de cette accélération, les téléchargements hebdomadaires npm ont atteint 52 millions en mai 2026 selon TestDino, faisant de Playwright le framework de test le plus utilisé au monde. Ce tutoriel Playwright vous emmène de l’installation jusqu’au pipeline CI parallélisé, en 13 étapes structurées et testables localement.

Le projet GitHub microsoft/playwright, qui affichait déjà plus de 78 600 étoiles fin décembre 2025 selon Testdino, comptabilise désormais plus de **510 000 dépôts dépendants** sur GitHub au 30 juillet 2026 et dépasse aujourd’hui les **89 600 étoiles**, restant l’outil de test E2E le plus téléchargé sur npm. Concrètement, vous allez construire une suite de tests complète pour une application de démonstration : authentification, formulaires, API REST, captures de régression visuelle, mocking réseau, et reporting HTML. Le projet final est exécutable sur GitHub Actions avec sharding sur 4 runners, soit une suite qui passe de 8 minutes à moins de 2 minutes en parallèle.

## Pourquoi Playwright en 2026 : 9 900 recherches mensuelles et adoption industrielle

Le test E2E n’est plus optionnel. D’après les données de recherche DataForSEO pour la France (avril 2026), le mot-clé « playwright » génère **9 900 recherches mensuelles** avec une concurrence faible, contre 27 100 pour Cypress (concurrence élevée) et 1 000 pour Selenium. Cette tendance se confirme au niveau mondial : les téléchargements hebdomadaires npm du package ont oscillé entre 20 et 33 millions fin 2025 et début 2026 selon TestDino, avant de culminer à 52 millions dès mai 2026, soit une multiplication par plus de 100 en cinq ans. Cette croissance s’explique par trois facteurs structurels : la complexité croissante des SPA React et Vue 3, l’obligation réglementaire de tester l’accessibilité (directive EAA effective en juin 2025), et la migration massive vers le tooling JavaScript moderne avec Bun et Vite. Là où Selenium WebDriver exigeait des configurations laborieuses et où Cypress reste bloqué sur Chromium par défaut, Playwright propose une API unifiée pour Chromium, Firefox et WebKit, avec auto-waiting et assertions web-first.

L’adoption en entreprise progresse rapidement : Microsoft, GitHub, Stripe, Vercel, Shopify et Hugging Face utilisent Playwright en production, aux côtés de plus de 4 400 entreprises vérifiées recensées par TestDino en décembre 2025. Selon la State of JavaScript Survey 2025, Playwright a dépassé Cypress en taux de satisfaction (88 % contre 76 %), et son taux d’adoption chez les équipes QA a bondi de 12 % en 2023 à 45,1 % en 2025 selon Agamisoft (citant le State of Testing), un niveau confirmé à 45,1 % dès mai 2026 par TestDino. La courbe d’apprentissage est plus douce qu’avec Selenium : un développeur familier de JavaScript ou TypeScript devient productif en moins de trois jours. Mieux encore, le runner intégré `@playwright/test` élimine la dépendance à Jest ou Mocha pour orchestrer la parallélisation, les retries et le sharding.

Sur le plan économique, Playwright reste open source sous licence Apache 2.0, sans paywall ni dashboard SaaS imposé. Cypress Cloud commence à 75 $ par mois pour 100 000 tests enregistrés, tandis que BrowserStack Automate facture 169 $ par utilisateur. Playwright se contente d’un runner local, d’un rapport HTML auto-hébergé et de l’intégration native à GitHub Actions, Azure Pipelines, GitLab CI ou Jenkins. Pour un cabinet de conseil ou une équipe produit française, l’économie annuelle dépasse souvent les 8 000 € sur une suite de taille moyenne.

## Prérequis et versions exactes : Node.js 20, Playwright 1.59, navigateurs bundled

Avant de plonger dans le code, validez votre environnement. Playwright 1.59 (publiée le 1er avril 2026 selon les release notes officielles) a depuis été suivie par la v1.61.1, puis par le patch v1.62.1 le 30 juillet 2026 selon DevUpdate.io, portant le compteur à 165 releases cumulées répertoriées par Microsoft sur GitHub à cette date. Entre-temps, l’éditeur Checkly a lui-même embarqué Playwright v1.58.2 dans son Runtime 2026.04 publié le 31 mars 2026, signe que l’écosystème CI/CD suit désormais la cadence de release Microsoft de très près ; la version courante requiert Node.js 18 ou supérieur, mais nous recommandons fortement Node.js 20 LTS pour bénéficier du support natif de `--watch` et des performances V8 améliorées. Sur macOS, vérifiez que vous tournez sous macOS 13 (Ventura) minimum ; sur Linux, Ubuntu 22.04 ou Debian 12 sont les configurations testées par Microsoft.

| Composant | Version requise | Version recommandée 2026 | Commande de vérification | 
|---|---|---|---|
| Node.js | ≥ 18.0 | 20.18 LTS | `node --version` | 
| npm | ≥ 9 | 10.9 | `npm --version` | 
| Playwright | 1.59.1 | 1.59.1 (stable avril 2026) | `npx playwright --version` | 
| Chromium bundled | 141.0.7390 | 141.0.7390.37 | Installé via `npx playwright install` | 
| Firefox bundled | 142.0 | 142.0.1 | Idem | 
| WebKit bundled | 26.0 | 26.0 | Idem | 
| OS supportés | — | Ubuntu 22/24, macOS 13+, Windows 10+ | `uname -a` | 
| RAM minimum | 4 Go | 8 Go (16 Go pour CI parallèle) | `free -h` | 

Une remarque importante sur les binaires de navigateurs : Playwright télécharge ses propres builds, séparés des navigateurs installés sur la machine. Cette isolation garantit la reproductibilité entre développeurs et CI, mais consomme environ 580 Mo dans `~/.cache/ms-playwright`. Si vous travaillez en environnement contraint (laptop SSD 256 Go), prévoyez l’espace. Vous pouvez également pointer vers un Chrome existant via `channel: 'chrome'` dans la configuration, ce qui évite le téléchargement de Chromium.

Côté éditeur, l’extension VS Code « Playwright Test for VSCode » publiée par Microsoft est quasi indispensable. Elle ajoute l’exécution test-par-test depuis la gutter, le debugger intégré, le picker de sélecteurs et l’enregistrement Codegen sans quitter l’IDE. Elle est compatible avec WebStorm via le plugin Playwright officiel JetBrains.

## Étape 1 : Installation initiale et scaffolding du projet

Créez un nouveau dossier et initialisez le projet Playwright via la commande officielle d’installation. Cette commande génère la structure de répertoires, un fichier `playwright.config.ts` typé, deux tests d’exemple et le workflow GitHub Actions correspondant. Choisissez TypeScript lorsqu’elle vous le propose : la majorité de la documentation 2026 utilise TypeScript et l’auto-complétion sur les sélecteurs est précieuse.

```
mkdir tutoriel-playwright-2026 && cd tutoriel-playwright-2026
npm init -y
npm init playwright@latest
# Choisir : TypeScript, dossier ./tests, ajouter GitHub Actions = yes, installer navigateurs = yes
npx playwright --version
# Output: Version 1.59.1
ls -la
# .github/workflows/playwright.yml
# tests/example.spec.ts
# tests-examples/demo-todo-app.spec.ts
# playwright.config.ts
# package.json
```
L’installation télécharge environ 580 Mo de binaires. Sur une connexion fibre, comptez 90 secondes. Sur une connexion entreprise filtrée, configurez le proxy via les variables `HTTPS_PROXY` et `PLAYWRIGHT_DOWNLOAD_HOST`. La commande `npx playwright install --with-deps` installe également les dépendances système sur Linux (libxkbcommon, libnss3, libgbm1), étape souvent oubliée qui produit l’erreur classique « Host system is missing dependencies to run browsers ».

## Étape 2 : Configurer playwright.config.ts pour la production

Le fichier `playwright.config.ts` orchestre l’ensemble du comportement de la suite. Une configuration de débutant tient en 10 lignes, mais une configuration prête pour la production exige des choix explicites sur les retries, les timeouts, le reporting, la base URL et les projets multi-navigateurs. Voici un fichier de référence pour une équipe française en 2026, optimisé pour CI parallèle et debugging local efficace.

```
// playwright.config.ts
import { defineConfig, devices } from '@playwright/test';
export default defineConfig({
  testDir: './tests',
  fullyParallel: true,
  forbidOnly: !!process.env.CI,
  retries: process.env.CI ? 2 : 0,
  workers: process.env.CI ? 4 : undefined,
  reporter: [
    ['html', { open: 'never' }],
    ['list'],
    ['junit', { outputFile: 'results.xml' }],
  ],
  use: {
    baseURL: process.env.BASE_URL || 'http://localhost:3000',
    trace: 'retain-on-failure-and-retries',
    screenshot: 'only-on-failure',
    video: 'retain-on-failure',
    actionTimeout: 10_000,
    navigationTimeout: 30_000,
    locale: 'fr-FR',
    timezoneId: 'Europe/Paris',
  },
  projects: [
    { name: 'chromium', use: { ...devices['Desktop Chrome'] } },
    { name: 'firefox',  use: { ...devices['Desktop Firefox'] } },
    { name: 'webkit',   use: { ...devices['Desktop Safari'] } },
    { name: 'iphone-15', use: { ...devices['iPhone 15'] } },
    { name: 'pixel-8',   use: { ...devices['Pixel 7'] } },
  ],
  webServer: {
    command: 'npm run dev',
    url: 'http://localhost:3000',
    reuseExistingServer: !process.env.CI,
    timeout: 120_000,
  },
});
```
Trois choix méritent une explication. D’abord, `trace: 'retain-on-failure-and-retries'` est la nouvelle option introduite dans Playwright 1.59 : elle conserve toutes les traces des tentatives ratées, ce qui simplifie l’analyse d’un test flaky qui finit par passer au 3e essai. Ensuite, `locale: 'fr-FR'` et `timezoneId: 'Europe/Paris'` sont essentiels pour les tests de paiement Stripe ou Lemon Squeezy qui adaptent leur interface selon la locale. Enfin, `webServer` démarre automatiquement votre application avant les tests et la coupe à la fin, supprimant le besoin de jongler avec Docker Compose en local.

## Étape 3 : Écrire votre premier test E2E avec les locators web-first

Playwright recommande depuis la version 1.27 l’usage exclusif de locators basés sur les rôles ARIA et les attributs accessibles. Cette approche présente deux avantages : les sélecteurs survivent aux refontes CSS et ils valident simultanément l’accessibilité de l’application. La nouvelle méthode `locator.normalize()` de Playwright 1.59 réécrit automatiquement les locators vers ces patterns recommandés, ce qui facilite la migration d’anciens tests Selenium ou Cypress.

```
// tests/login.spec.ts
import { test, expect } from '@playwright/test';
test.describe('Authentification utilisateur', () => {
  test.beforeEach(async ({ page }) => {
    await page.goto('/connexion');
  });
  test('connexion reussie avec identifiants valides', async ({ page }) => {
    await page.getByLabel('Adresse e-mail').fill('[email protected]');
    await page.getByLabel('Mot de passe').fill('MotDePasseSecur1!');
    await page.getByRole('button', { name: 'Se connecter' }).click();
    await expect(page).toHaveURL(/\/tableau-de-bord/);
    await expect(page.getByRole('heading', { level: 1 })).toHaveText('Bienvenue');
    await expect(page.getByTestId('user-menu')).toBeVisible();
  });
  test('message d erreur sur identifiants invalides', async ({ page }) => {
    await page.getByLabel('Adresse e-mail').fill('[email protected]');
    await page.getByLabel('Mot de passe').fill('mauvais');
    await page.getByRole('button', { name: 'Se connecter' }).click();
    const alert = page.getByRole('alert');
    await expect(alert).toContainText('Identifiants invalides');
    await expect(page).toHaveURL(/\/connexion/);
  });
});
```
Exécutez ce test avec `npx playwright test tests/login.spec.ts --headed` pour voir le navigateur en action. Notez l’absence totale de `waitFor` ou `sleep` : Playwright attend automatiquement que chaque élément soit actionable avant de cliquer ou de remplir, et chaque assertion `expect()` est web-first, c’est-à-dire qu’elle retentent jusqu’au timeout (5 secondes par défaut). Cette différence avec Selenium élimine 80 % des causes classiques de tests flaky.

## Étape 4 : Générer des tests avec Codegen et l’enregistreur

L’outil Codegen est probablement la fonctionnalité la plus sous-estimée de Playwright. Lancez `npx playwright codegen http://localhost:3000` et une fenêtre Chromium s’ouvre, accompagnée d’un Inspector. Toutes vos interactions sont enregistrées sous forme de code TypeScript prêt à coller dans un fichier de test. Playwright 1.59 a amélioré le générateur : il insère désormais automatiquement les assertions `toBeVisible()` appropriées et propose les locators `getByRole` en priorité.

```
# Lancer Codegen sur une URL
npx playwright codegen https://votreapp.fr
# Lancer Codegen en mode mobile (iPhone 15)
npx playwright codegen --device="iPhone 15" https://votreapp.fr
# Lancer Codegen avec une locale et un timezone specifiques
npx playwright codegen --lang=fr-FR --timezone="Europe/Paris" https://votreapp.fr
# Enregistrer directement dans un fichier
npx playwright codegen -o tests/onboarding.spec.ts https://votreapp.fr
```
Le pattern recommandé en équipe est le suivant : un développeur enregistre un parcours utilisateur complet via Codegen, exporte vers un fichier `.spec.ts`, puis refactorise les assertions et extrait les Page Objects. Cette approche divise par trois le temps d’écriture initial d’une suite de tests. Pour un onboarding e-commerce avec 12 étapes (panier, paiement, confirmation), comptez 8 minutes d’enregistrement contre 45 minutes d’écriture manuelle.

## Étape 5 : Page Object Model et fixtures pour code réutilisable

Dès que votre suite dépasse 20 tests, l’inflation des sélecteurs devient un problème. Le Page Object Model encapsule les locators et les actions dans des classes dédiées, suivant le principe de responsabilité unique. Playwright propose en complément un système de **fixtures** typé qui dépasse de loin les fixtures Pytest ou Mocha : chaque test reçoit ses dépendances par injection, avec lifecycle automatique.

```
// pages/LoginPage.ts
import { Page, Locator, expect } from '@playwright/test';
export class LoginPage {
  readonly page: Page;
  readonly emailInput: Locator;
  readonly passwordInput: Locator;
  readonly submitButton: Locator;
  readonly errorAlert: Locator;
  constructor(page: Page) {
    this.page = page;
    this.emailInput = page.getByLabel('Adresse e-mail');
    this.passwordInput = page.getByLabel('Mot de passe');
    this.submitButton = page.getByRole('button', { name: 'Se connecter' });
    this.errorAlert = page.getByRole('alert');
  }
  async goto() {
    await this.page.goto('/connexion');
  }
  async login(email: string, password: string) {
    await this.emailInput.fill(email);
    await this.passwordInput.fill(password);
    await this.submitButton.click();
  }
  async expectErrorMessage(message: string) {
    await expect(this.errorAlert).toContainText(message);
  }
}
// fixtures.ts
import { test as base } from '@playwright/test';
import { LoginPage } from './pages/LoginPage';
type MesFixtures = { loginPage: LoginPage };
export const test = base.extend<MesFixtures>({
  loginPage: async ({ page }, use) => {
    const lp = new LoginPage(page);
    await lp.goto();
    await use(lp);
  },
});
export { expect } from '@playwright/test';
```
Avec cette structure, un test devient extrêmement lisible : `await loginPage.login('[email protected]', 'pwd')`. Si demain la page de connexion change de markup, vous modifiez un seul fichier au lieu de 40 tests. Les fixtures supportent également le scope `worker` pour partager une ressource coûteuse (instance Postgres, container Redis) entre tous les tests d’un worker, exactement comme avec Pytest.

## Étape 6 : Authentification persistante avec storageState

Se reconnecter avant chaque test est lent et fragile. Le pattern recommandé par la documentation officielle Playwright Auth consiste à se connecter une fois dans un fichier de setup global, puis à sauvegarder l’état du navigateur (cookies, localStorage, sessionStorage) dans un fichier JSON. Tous les tests suivants chargent cet état et démarrent déjà authentifiés.

```
// auth.setup.ts
import { test as setup, expect } from '@playwright/test';
const authFile = 'playwright/.auth/user.json';
setup('authentification', async ({ page }) => {
  await page.goto('/connexion');
  await page.getByLabel('Adresse e-mail').fill(process.env.TEST_EMAIL!);
  await page.getByLabel('Mot de passe').fill(process.env.TEST_PASSWORD!);
  await page.getByRole('button', { name: 'Se connecter' }).click();
  await page.waitForURL('/tableau-de-bord');
  await expect(page.getByTestId('user-menu')).toBeVisible();
  await page.context().storageState({ path: authFile });
});
// playwright.config.ts (extrait)
export default defineConfig({
  projects: [
    { name: 'setup', testMatch: /.*\.setup\.ts/ },
    {
      name: 'chromium',
      use: { ...devices['Desktop Chrome'], storageState: 'playwright/.auth/user.json' },
      dependencies: ['setup'],
    },
  ],
});
```
Cette stratégie réduit le temps total d’une suite de 50 tests d’environ 6 minutes à 90 secondes. Pour les applications avec OAuth (Google, GitHub, Microsoft), Playwright supporte également l’injection directe de tokens via `page.context().addCookies()`, ce qui évite le passage par le flow OAuth complet. Pour les SSO d’entreprise complexes (SAML, OIDC avec Keycloak), reportez-vous à notre tutoriel Keycloak 26.6 SSO/OIDC qui couvre l’intégration côté serveur d’identité.

## Étape 7 : Tester l’API REST avec request fixture

Beaucoup d’équipes ne réalisent pas que Playwright n’est pas qu’un outil UI : la fixture `request` permet d’écrire des tests d’API REST sans démarrer aucun navigateur. C’est extrêmement rapide (10 à 30 ms par test) et particulièrement utile pour tester un backend Fastify, Express ou NestJS avant le déploiement. Les mêmes assertions et le même test runner sont utilisés, ce qui unifie la stack de tests.

```
// tests/api/articles.spec.ts
import { test, expect } from '@playwright/test';
const API_BASE = process.env.API_URL || 'http://localhost:4000';
test.describe('API REST Articles', () => {
  let articleId: string;
  let authToken: string;
  test.beforeAll(async ({ request }) => {
    const res = await request.post(`${API_BASE}/auth/login`, {
      data: { email: '[email protected]', password: 'admin' },
    });
    expect(res.status()).toBe(200);
    authToken = (await res.json()).token;
  });
  test('POST /articles cree un article', async ({ request }) => {
    const res = await request.post(`${API_BASE}/articles`, {
      headers: { Authorization: `Bearer ${authToken}` },
      data: { titre: 'Mon article 2026', contenu: 'Lorem ipsum...' },
    });
    expect(res.status()).toBe(201);
    const body = await res.json();
    expect(body).toHaveProperty('id');
    expect(body.titre).toBe('Mon article 2026');
    articleId = body.id;
  });
  test('GET /articles/:id renvoie l article', async ({ request }) => {
    const res = await request.get(`${API_BASE}/articles/${articleId}`);
    expect(res.status()).toBe(200);
    const body = await res.json();
    expect(body.titre).toBe('Mon article 2026');
  });
  test.afterAll(async ({ request }) => {
    await request.delete(`${API_BASE}/articles/${articleId}`, {
      headers: { Authorization: `Bearer ${authToken}` },
    });
  });
});
```
Pour un backend construit avec notre tutoriel Fastify 5.8 API REST TypeScript ou un NestJS 11 avec Prisma, vous pouvez exécuter la suite API sur chaque pull request en 12 secondes là où une suite UI complète prendrait 4 minutes. C’est l’approche recommandée pour les boucles de feedback rapides en développement.

## Étape 8 : Mocking réseau et interception de requêtes

Tester un parcours utilisateur qui dépend d’une API tierce (Stripe, SendGrid, Mapbox) sans appeler ces services en production est un défi classique. Playwright propose `page.route()` pour intercepter toute requête sortante et la remplacer par une réponse contrôlée. Cette technique transforme des tests E2E imprévisibles en tests parfaitement déterministes.

```
// tests/checkout.spec.ts
import { test, expect } from '@playwright/test';
test('paiement Stripe avec mock', async ({ page }) => {
  // Intercepter l'appel Stripe et renvoyer succes
  await page.route('**/v1/payment_intents**', async (route) => {
    await route.fulfill({
      status: 200,
      contentType: 'application/json',
      body: JSON.stringify({
        id: 'pi_3OxXxXxXxXxXxXxX',
        status: 'succeeded',
        amount: 4999,
        currency: 'eur',
      }),
    });
  });
  // Bloquer Google Analytics pour des tests propres
  await page.route('**/google-analytics.com/**', (route) => route.abort());
  await page.goto('/panier');
  await page.getByRole('button', { name: 'Payer 49,99 EUR' }).click();
  await expect(page.getByText('Paiement confirme')).toBeVisible();
});
// Simuler une erreur reseau
test('gestion erreur API', async ({ page }) => {
  await page.route('**/api/checkout**', (route) =>
    route.fulfill({ status: 503, body: 'Service Unavailable' })
  );
  await page.goto('/panier');
  await page.getByRole('button', { name: 'Payer' }).click();
  await expect(page.getByRole('alert')).toContainText('Service temporairement indisponible');
});
```
Playwright 1.59 conserve les requêtes interceptées dans le Trace Viewer avec un nouveau toggle pretty-print pour les bodies JSON et form-data. Vous pouvez désormais débugger un mock défaillant en visualisant la requête réelle et la réponse renvoyée côte à côte, sans recourir à `console.log`. Pour les payloads volumineux (GraphQL, JSON-RPC), cette amélioration divise le temps d’investigation par cinq.

## Étape 9 : Tests de régression visuelle avec toHaveScreenshot

Un bouton dont la couleur passe accidentellement de bleu à gris, une typographie qui change de Inter à Times après un déploiement de polices, un layout qui casse sur Firefox uniquement : ces régressions visuelles échappent à toutes les assertions classiques. La méthode `toHaveScreenshot()` de Playwright capture une image, la compare à une baseline et signale toute différence supérieure à un seuil configurable.

```
// tests/visuel.spec.ts
import { test, expect } from '@playwright/test';
test('regression visuelle page d accueil', async ({ page }) => {
  await page.goto('/');
  // Masquer les elements dynamiques (compteurs, dates)
  await expect(page).toHaveScreenshot('homepage.png', {
    fullPage: true,
    mask: [page.getByTestId('compteur-temps-reel'), page.locator('time')],
    maxDiffPixels: 50,
    threshold: 0.2,
  });
});
test('composant bouton CTA', async ({ page }) => {
  await page.goto('/');
  const cta = page.getByRole('button', { name: 'Commencer gratuitement' });
  await expect(cta).toHaveScreenshot('cta-bouton.png', {
    animations: 'disabled',
  });
});
```
Lors de la première exécution (`--update-snapshots`), Playwright génère les images de référence dans `tests/visuel.spec.ts-snapshots/`. Versionnez-les avec Git et activez Git LFS si vous dépassez les 100 Mo cumulés. Pour éviter les faux positifs entre macOS et Linux (rendu de polices différent), vos baselines doivent provenir du même OS que la CI : exécutez `--update-snapshots` dans un container Docker Linux pour garantir la cohérence.

## Étape 10 : Trace Viewer et UI Mode pour le débogage

Quand un test échoue en CI sans raison apparente, le Trace Viewer est votre meilleur allié. Il enregistre chaque action, capture le DOM à chaque étape, journalise les requêtes réseau, les logs console et les sources des assertions. Avec le nouveau mode `retain-on-failure-and-retries` de Playwright 1.59, vous récupérez les traces de chaque tentative ratée, ce qui révèle les patterns de tests flaky.

```
# Executer avec traces forcees en local
npx playwright test --trace on
# Visualiser une trace generee
npx playwright show-trace test-results/login-spec-ts-chromium/trace.zip
# Mode UI interactif (recommande pour le developpement)
npx playwright test --ui
# Debug pas-a-pas avec inspecteur
PWDEBUG=1 npx playwright test tests/login.spec.ts
# Lancer un test specifique en headed mode avec slow motion
npx playwright test login.spec.ts --headed --slow-mo=500
```
L’UI Mode (`--ui`) est la fonctionnalité préférée des équipes Playwright depuis sa stabilisation en 1.42. Elle ouvre une fenêtre dédiée affichant tous vos tests, leur arbre d’exécution, leur historique de runs, leurs traces, et permet de relancer un test isolément en un clic. Combinée avec l’extension VS Code, l’UI Mode élimine quasi totalement le besoin de `console.log` dans les tests.

## Étape 11 : Émulation mobile, accessibilité et tests cross-browser

Playwright émule plus de 100 devices via `devices['iPhone 15']`, `devices['Pixel 7']`, ou `devices['Galaxy S9+']`. Chaque device configure user-agent, viewport, device-scale-factor, support tactile, géolocalisation et permissions. La nouveauté de Playwright 1.59 est `page.ariaSnapshot()` qui capture l’arbre d’accessibilité pour valider la conformité WCAG 2.2 et la directive EAA européenne entrée en vigueur en juin 2025.

```
// tests/mobile.spec.ts
import { test, expect, devices } from '@playwright/test';
test.use({ ...devices['iPhone 15'], locale: 'fr-FR' });
test('navigation mobile iPhone 15', async ({ page }) => {
  await page.goto('/');
  await expect(page.getByRole('button', { name: 'Menu' })).toBeVisible();
  await page.getByRole('button', { name: 'Menu' }).tap();
  await expect(page.getByRole('navigation')).toBeVisible();
});
// Accessibilite avec ariaSnapshot (Playwright 1.59+)
test('arbre d accessibilite conforme', async ({ page }) => {
  await page.goto('/');
  const snapshot = await page.locator('main').ariaSnapshot();
  expect(snapshot).toContain('heading "Accueil" [level=1]');
  expect(snapshot).toContain('button "Commencer gratuitement"');
});
// Geolocalisation simulee Paris
test('geolocalisation Paris', async ({ browser }) => {
  const context = await browser.newContext({
    geolocation: { latitude: 48.8566, longitude: 2.3522 },
    permissions: ['geolocation'],
    locale: 'fr-FR',
  });
  const page = await context.newPage();
  await page.goto('/agences-proches');
  await expect(page.getByText('Paris')).toBeVisible();
});
```
Pour l’audit d’accessibilité complet, combinez Playwright avec `@axe-core/playwright` qui exécute le moteur Axe sur chaque page testée et signale les violations WCAG. Une intégration standard ajoute 80 ms par test mais permet de bloquer un déploiement si le score d’accessibilité chute. Plusieurs sites publics français (impots.gouv.fr, ameli.fr) suivent désormais cette approche en production.

## Étape 12 : CI parallèle avec GitHub Actions et sharding sur 4 runners

Une suite de 200 tests E2E qui prend 8 minutes en local devient inutilisable sur chaque pull request si la CI ne parallélise pas. Le sharding officiel de Playwright divise la suite en N parties exécutées sur N runners distincts, puis fusionne les rapports HTML. Sur 4 runners GitHub-hosted, vous passez de 8 minutes à environ 2 minutes 15 secondes.

```
# .github/workflows/playwright.yml
name: Playwright Tests
on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
jobs:
  test:
    timeout-minutes: 30
    runs-on: ubuntu-22.04
    strategy:
      fail-fast: false
      matrix:
        shard: [1, 2, 3, 4]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: 'npm'
      - run: npm ci
      - run: npx playwright install --with-deps chromium
      - run: npx playwright test --shard=${{ matrix.shard }}/4
      - uses: actions/upload-artifact@v4
        if: always()
        with:
          name: blob-report-${{ matrix.shard }}
          path: blob-report
          retention-days: 7
  merge-reports:
    if: always()
    needs: test
    runs-on: ubuntu-22.04
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with: { node-version: 20, cache: 'npm' }
      - run: npm ci
      - uses: actions/download-artifact@v4
        with:
          path: all-blob-reports
          pattern: blob-report-*
          merge-multiple: true
      - run: npx playwright merge-reports --reporter html ./all-blob-reports
      - uses: actions/upload-artifact@v4
        with:
          name: playwright-html-report
          path: playwright-report
          retention-days: 14
```
L’astuce du `blob` reporter, introduit en Playwright 1.37, sérialise les résultats de chaque shard dans un format intermédiaire, puis le job `merge-reports` les agrège en un rapport HTML unique téléchargeable comme artifact. Pour un pipeline complet incluant lint, tests unitaires Vitest et tests E2E, consultez notre guide GitHub Actions CI/CD 2026 qui détaille les caches optimisés et les stratégies de réutilisation des workflows.

## Étape 13 : Component testing pour React et Vue 3

Au-delà du E2E classique, Playwright propose depuis la version 1.22 le component testing pour React, Vue, Svelte et Solid. Chaque composant est monté dans un vrai navigateur (pas dans JSDOM comme Vitest), ce qui élimine la classe entière de bugs liée aux divergences DOM. Pour une bibliothèque de design system, c’est souvent la couche de tests qui détecte les régressions visuelles avant la production.

```
// tests/Button.spec.tsx
import { test, expect } from '@playwright/experimental-ct-react';
import { Button } from '../src/components/Button';
test('rendu du bouton CTA', async ({ mount }) => {
  const component = await mount(
    <Button variant="primary" onClick={() => console.log('clicked')}>
      Commencer maintenant
    </Button>
  );
  await expect(component).toHaveText('Commencer maintenant');
  await expect(component).toHaveCSS('background-color', 'rgb(37, 99, 235)');
});
test('bouton desactive', async ({ mount }) => {
  const component = await mount(<Button disabled>Indisponible</Button>);
  await expect(component).toBeDisabled();
  await component.click({ force: true });
  // Aucune action ne devrait etre declenchee
});
```
Le component testing reste marqué experimental en 1.59, mais il est utilisé en production par GitHub, Microsoft et Shopify. Sa stabilisation est annoncée pour Playwright 2.0 prévu fin 2026. Pour tester des composants plus simples sans navigateur, restez sur Vitest 4.1 qui reste deux à trois fois plus rapide pour les tests unitaires purs.

## Playwright vs Cypress vs Selenium : tableau comparatif 2026

Le marché du test E2E compte trois acteurs majeurs en 2026. Voici une comparaison factuelle basée sur les versions stables actuelles et les métriques publiques GitHub, npm et State of JavaScript Survey 2025.

| Critère | Playwright 1.59 | Cypress 14.x | Selenium 4.x | 
|---|---|---|---|
| Étoiles GitHub (avril 2026) | 89 635 | ~47 000 | ~30 000 | 
| Navigateurs supportés | Chromium, Firefox, WebKit | Chromium, Firefox, Edge, Electron | Tous (via WebDriver) | 
| Test runner natif | Oui (@playwright/test) | Oui (intégré) | Non (Mocha, Pytest…) | 
| Auto-waiting | Oui (web-first) | Oui (limité) | Non (waits manuels) | 
| Parallélisation locale | Native, illimitée | Payante (Cypress Cloud) | Via Grid manuel | 
| API testing | Oui (request fixture) | Oui (cy.request) | Non natif | 
| Mobile emulation | 100+ devices preset | Viewport seulement | Via Appium | 
| Component testing | React, Vue, Svelte, Solid | React, Vue, Angular, Svelte | Non | 
| Trace/replay viewer | Oui (Trace Viewer) | Oui (Test Replay payant) | Non | 
| Licence | Apache 2.0 | MIT (cloud payant) | Apache 2.0 | 
| Coût annuel équipe 10 dev | 0 EUR | ~9 000 EUR (Cloud) | 0 EUR (auto-hébergé) | 
| Vitesse run 100 tests | ~45 sec | ~110 sec | ~180 sec | 
| Satisfaction StateOfJS 2025 | 88 % | 76 % | 54 % | 

Playwright domine sur le rapport vitesse/coût/cross-browser. Cypress conserve un avantage marketing et un écosystème de plugins plus mature dans certaines niches (testing-library, accessibilité), mais l’écart se resserre depuis 2024. Selenium reste pertinent pour les contextes legacy ou les besoins multi-langages (Java, C#, Ruby) où Playwright officiel n’existe qu’en JavaScript, Python, .NET et Java.

## Pièges courants et erreurs à éviter

Voici les 8 erreurs les plus fréquentes rencontrées en mission audit chez des équipes françaises en 2026. Les éviter dès la conception de votre suite épargne des semaines de refactoring.

- **Sélecteurs CSS fragiles** : utiliser`.btn-primary` ou`div > div:nth-child(3)` au lieu de`getByRole('button', { name: '...' })` . Solution : exécuter`locator.normalize()` sur l’ancien code et migrer progressivement.
- **Sleep et waitForTimeout** : ces appels créent des tests lents et flaky. Remplacer par`expect(locator).toBeVisible()` qui retentent jusqu’au timeout.
- **Tests dépendants entre eux** : un test qui dépend de l’état laissé par le précédent casse en parallèle. Chaque test doit créer ses propres données et les nettoyer.
- **Storage state non rafraîchi** : si votre token JWT expire en 1 heure et que vous régénérez`user.json` quotidiennement, vos tests échouent le matin. Régénérez avant chaque run CI.
- **Baselines visuelles cross-OS** : une baseline macOS échoue systématiquement en CI Linux à cause du rendu de polices. Régénérez en Docker Linux.
- **Trop de retries** :`retries: 5` masque les vrais bugs. Garder à 2 maximum et investiguer chaque flake.
- **Timeouts insuffisants en CI** : un test qui passe en 2 sec local peut prendre 8 sec en CI partagée. Augmenter`actionTimeout` à 15 secondes pour CI.
- **Pas de cleanup en afterAll** : créer 1 000 articles de test sans les supprimer pollue la base de données et finit par casser le seed.

## Dépannage : 8 erreurs Playwright fréquentes et solutions

| Erreur | Cause probable | Solution | 
|---|---|---|
| `Host system is missing dependencies` | Bibliothèques Linux manquantes | `npx playwright install --with-deps` | 
| `Test timeout of 30000ms exceeded` | Auto-wait dépasse le délai | Augmenter `timeout` dans la config ou via`test.setTimeout()` | 
| `Browser closed unexpectedly` | OOM kill, RAM insuffisante en CI | Passer à un runner 4 vCPU/16 Go ou réduire workers | 
| `strict mode violation` | Locator matche plusieurs éléments | Préciser avec `.first()` ,`.nth(0)` ou meilleur sélecteur | 
| `page.goto: net::ERR_CONNECTION_REFUSED` | webServer pas encore démarré | Augmenter `webServer.timeout` à 180 sec | 
| `expect(received).toHaveScreenshot` failed | Baseline différente (OS, polices) | Régénérer en Docker Linux : `--update-snapshots` | 
| `Cannot find module @playwright/test` | Installation incomplète | `rm -rf node_modules && npm ci` | 
| `TimeoutError: locator.click` | Élément masqué par overlay/modal | Fermer modal d’abord ou utiliser `force: true` | 

## Astuces avancées pour 2026 : sharding intelligent, traces sélectives

Trois techniques permettent de pousser Playwright au-delà du tutoriel standard. La première est le **sharding intelligent** : au lieu de répartir aléatoirement les tests, utilisez `--shard` en combinaison avec un fichier de durées historiques pour équilibrer les runners. Un test de 90 secondes ne doit pas se retrouver isolé sur un runner pendant que 3 autres terminent en 20 secondes. Le plugin communautaire `playwright-balance` automatise cet équilibrage à partir de `results.json` de la run précédente.

La deuxième technique est l’usage de **tags dans les noms de tests** pour exécution sélective. Marquez vos tests critiques avec `@smoke` et lancez uniquement la suite smoke avant chaque déploiement : `npx playwright test --grep @smoke`. La suite complète tourne la nuit. Cette segmentation divise par 5 le temps de feedback sur les pull requests sans sacrifier la couverture nocturne.

La troisième est l’**intégration avec Docker Compose** pour reproduire l’environnement de production en CI. Démarrez PostgreSQL, Redis, votre backend et un mailcatcher (MailHog) via `docker compose up` avant les tests E2E. Cette approche, détaillée dans notre tutoriel Docker Compose Stack Production, permet de tester les flows complets (inscription, e-mail de confirmation, première connexion) sans dépendre de services externes payants.

## Exemples de sortie : rapport HTML et JUnit

Voici à quoi ressemble une exécution complète de la suite de ce tutoriel sur un projet Next.js 15 hébergé en local. Vous obtenez trois sorties simultanées grâce à la configuration du reporter dans `playwright.config.ts`.

```
$ npx playwright test
Running 47 tests using 4 workers
  ok  1 [chromium] tests/login.spec.ts:8:3 connexion reussie (1.2s)
  ok  2 [chromium] tests/login.spec.ts:22:3 message d erreur (0.8s)
  ok  3 [chromium] tests/checkout.spec.ts:5:3 paiement Stripe (2.1s)
  ok  4 [firefox]  tests/login.spec.ts:8:3 connexion reussie (1.4s)
  ok  5 [webkit]   tests/login.spec.ts:8:3 connexion reussie (1.9s)
  KO  6 [chromium] tests/visuel.spec.ts:8:3 regression visuelle (3.2s)
       Snapshot comparison failed:
         50 pixels (ratio 0.001 of all image pixels) are different.
       Expected: tests/visuel.spec.ts-snapshots/homepage-chromium-linux.png
       Received: test-results/visuel-spec-ts-regression-visuelle/homepage-actual.png
       Diff:     test-results/visuel-spec-ts-regression-visuelle/homepage-diff.png
  46 passed, 1 failed (1m 18s)
To open last HTML report run:
  npx playwright show-report
```
Le rapport HTML auto-hébergé (`npx playwright show-report`) ouvre une interface web sur le port 9323 avec filtres par projet, statut, durée, tags. Chaque test échoué offre un lien vers sa trace, ses captures, son log console et sa vidéo enregistrée. Le rapport JUnit (`results.xml`) est consommé directement par GitLab, Jenkins ou Azure DevOps pour afficher les résultats dans l’UI native du serveur CI.

## Projet complet : structure de fichiers et arborescence finale

Au terme des 13 étapes, votre projet doit ressembler à l’arborescence suivante. Cette structure est utilisée en production par plusieurs équipes françaises de la French Tech et reste maintenable même au-delà de 500 tests.

```
tutoriel-playwright-2026/
.github/
  workflows/
    playwright.yml          # CI parallele sharding 4
playwright/
  .auth/
    user.json               # Storage state (gitignored)
pages/                      # Page Object Model
  LoginPage.ts
  DashboardPage.ts
  CheckoutPage.ts
fixtures/
  index.ts                  # Fixtures TypeScript typees
tests/
  auth.setup.ts             # Setup global d authentification
  login.spec.ts
  checkout.spec.ts
  visuel.spec.ts            # Regression visuelle
  mobile.spec.ts            # iPhone, Pixel
  api/
    articles.spec.ts        # Tests API REST
    users.spec.ts
  components/
    Button.spec.tsx         # Component testing
test-results/               # Gitignored
playwright-report/          # Gitignored
playwright.config.ts
package.json
tsconfig.json
.gitignore
```
Le `package.json` final exporte cinq scripts clés : `test` (suite complète), `test:smoke` (tag @smoke uniquement), `test:ui` (mode UI interactif), `test:update-snapshots` (régénérer baselines visuelles), `test:report` (ouvrir le dernier rapport HTML). Cette convention de nommage est devenue standard dans la communauté Playwright française et facilite l’onboarding des nouveaux développeurs.

## Questions fréquentes sur Playwright en 2026

### Playwright remplace-t-il complètement Cypress en 2026 ?

Pour les nouveaux projets, oui dans la majorité des cas. Playwright offre des fonctionnalités supérieures (WebKit, sharding gratuit, API testing native) sans paywall. Cypress garde un avantage pour les équipes déjà investies dans son écosystème ou utilisant Cypress Component Testing avec Angular, où Playwright n’a pas encore d’équivalent direct.

### Quelle version de Node.js choisir pour Playwright 1.59 ?

Node.js 20 LTS est l’option recommandée en avril 2026. Node.js 18 reste supporté mais arrivera en fin de vie en avril 2025 (déjà dépassé). Node.js 22 fonctionne aussi mais reste « current » jusqu’à octobre 2026. Pour la stabilité CI, restez sur 20.

### Combien de temps pour exécuter 200 tests Playwright sur GitHub Actions ?

Sans sharding, environ 8 à 12 minutes sur ubuntu-22.04. Avec sharding sur 4 runners, environ 2 minutes 15 secondes plus 30 secondes de merge des rapports. Le coût additionnel reste nul sur les minutes incluses GitHub gratuites pour les repos publics (et 0,008 $/min pour repos privés).

### Peut-on tester une PWA ou une app Electron avec Playwright ?

Oui. Pour les PWA, utilisez les configurations device standards et activez le mode offline via `context.setOffline(true)`. Pour Electron, Playwright propose `_electron` dans son API expérimentale qui pilote directement l’application packagée. C’est ce que VS Code utilise pour ses propres tests.

### Comment intégrer Playwright avec un backend Django ou Symfony ?

Playwright est agnostique du backend. Pour Django, démarrez votre serveur via la configuration `webServer` dans `playwright.config.ts` avec `command: 'python manage.py runserver'`. Pour Symfony, c’est `symfony serve --no-tls`. Notre tutoriel Django 5.2 LTS couvre l’intégration côté Python et notre guide API Platform 4.2 traite Symfony.

### Playwright fonctionne-t-il sur ARM (Apple Silicon, AWS Graviton) ?

Oui depuis la version 1.40. Les binaires Chromium et WebKit sont compilés en ARM64 natif pour macOS Apple Silicon et Linux ARM64. AWS Graviton fonctionne nativement, ce qui réduit le coût CI d’environ 20 % par rapport aux runners x86 chez plusieurs cloud providers.

### Quel est l’impact RGPD des tests Playwright en CI ?

Aucun si vous utilisez des données de test fictives et n’envoyez pas de traces vers un service tiers. Les traces Playwright restent dans GitHub Actions artifacts par défaut. Si vous activez un service comme Currents.dev ou DeploySentinel, vérifiez leur conformité RGPD et hébergement européen. Playwright lui-même ne télémetre rien depuis l’installation, sauf si vous activez explicitement `PLAYWRIGHT_TELEMETRY=1`.

### Comment migrer une suite Selenium existante vers Playwright ?

Procédez progressivement test par test, plutôt qu’en big bang. Playwright et Selenium peuvent coexister dans le même projet. Commencez par les nouveaux tests sur Playwright, puis migrez les tests les plus flaky de Selenium. La fonction `locator.normalize()` de Playwright 1.59 aide à convertir les sélecteurs XPath Selenium vers du `getByRole` recommandé.

## Pour aller plus loin

### Related Coverage

- Tutoriel Vitest 4.1 : Tests JavaScript en 13 Étapes — Complément idéal pour tests unitaires et composants en JSDOM
- Tutoriel Pytest Python 2026 : Tests Automatisés en 13 Étapes — L’équivalent Python pour vos backends FastAPI ou Django
- Tutoriel Selenium Python 2026 : Automatisation Web en 13 Étapes — Comparaison technique et migration progressive
- Tutoriel GitHub Actions 2026 : CI/CD Complet — Le pipeline parallélisé qui complète votre setup Playwright
- Tutoriel Next.js : App Full-Stack en 13 Étapes — Le framework idéal à tester avec Playwright en 2026
- Tutoriel Docker Compose 2026 : Stack Production — Reproduire votre environnement de production pour les tests
- Tutoriel Fastify 5.8 : API REST Node.js — Pour le backend que vous testez via Playwright API

## Conclusion : Playwright, le choix par défaut pour les tests E2E en 2026

En 13 étapes, vous avez construit une suite de tests E2E professionnelle couvrant authentification persistante, API REST, mocking réseau, régression visuelle, émulation mobile, accessibilité ARIA et CI parallélisée sur 4 shards. Avec Playwright 1.59 et son écosystème open source, vous obtenez l’équivalent d’un produit SaaS facturé 9 000 € par an, sans aucune dépendance externe ni paywall. Cette autonomie technique est précieuse en 2026 où les équipes produits cherchent à reprendre le contrôle de leur stack de tests face à la consolidation du marché du SaaS de testing.

Le prochain jalon majeur est Playwright 2.0 attendu fin 2026 avec stabilisation du component testing et nouvelle API d’inspection AI-assisté. En attendant, votre suite actuelle reste 100 % compatible avec les futures versions grâce à la politique de stabilité long-terme de Microsoft. Pour suivre les sorties, abonnez-vous au flux des releases GitHub Playwright et au changelog officiel.

Bonne mise en pratique, et n’hésitez pas à partager votre configuration sur le canal Discord Playwright France si vous rencontrez des défis spécifiques avec votre stack.