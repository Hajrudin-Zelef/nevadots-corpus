Express.js reste le framework Node.js le plus utilisé au monde pour créer des API REST, avec environ 69 000 étoiles sur GitHub et 82 494 010 téléchargements sur npm pour la seule semaine du 6 juin 2026, selon les statistiques npmcharts. Avec la sortie d’Express 5.2.1 — la version stable la plus récente, publiée le 1er décembre 2025 — et une adoption qui continue de grimper (TechnologyChecker recense 138 238 domaines actifs utilisant Express.js en août 2026, contre 90 377 seulement en juin 2026), le support natif des Promises et une sécurité renforcée contre les attaques ReDoS font entrer le framework dans une nouvelle ère. Ce tutoriel vous guide pas à pas pour construire une API REST complète avec Express.js 5, de l’installation à la mise en production, en 12 étapes concrètes.

Que vous soyez développeur backend débutant ou que vous migriez depuis Express 4, ce guide couvre tout : routage, middleware, validation, authentification JWT, gestion d’erreurs, tests et déploiement. Chaque étape inclut du code fonctionnel, des exemples de sortie et les pièges courants à éviter. Temps estimé : 60 minutes.

## Prérequis et Versions Requises pour Express.js 5

Avant de commencer, assurez-vous de disposer de l’environnement suivant. Express 5 abandonne le support des versions de Node.js antérieures à la v18, ce qui constitue un changement majeur par rapport à Express 4.

| Outil | Version minimale | Version recommandée | Rôle | 
|---|---|---|---|
| Node.js | 18.x | 22.x LTS | Runtime JavaScript | 
| npm | 9.x | 10.x | Gestionnaire de paquets | 
| Express.js | 5.0.0 | 5.2.1 | Framework HTTP | 
| MongoDB | 7.0 | 8.0 | Base de données NoSQL | 
| VS Code ou éditeur | – | Dernière version | Éditeur de code | 
| Postman ou cURL | – | Dernière version | Test des endpoints | 

Express 5.2.1, publié le 1er décembre 2025, reste la version stable la plus récente et bénéficie d’un support de sécurité actif sur la branche 5.x, un statut confirmé à la fois par CompatHub et par la page Wikipedia consacrée à Express.js, qui référencent toutes deux cette même date de sortie. Fait notable, la version 5.2.0 est sortie le même jour en tant que release stable distincte selon CompatHub, avant d’être rapidement dépassée par le correctif 5.2.1, qui corrige un changement cassant introduit dans la v5.2.0 concernant le parser de requêtes étendues. Le support natif des Promises et le routage mis à jour via path-to-regexp v8 sont les deux améliorations majeures de cette version, qui compte désormais environ 23 400 forks sur GitHub selon SoloDevStack — un signe de son adoption large. Selon le calendrier LTS officiel d’Express.js, la branche 5.x restera maintenue au moins jusqu’en avril 2026, tandis que l’équipe prévoit une version 6.x au plus tôt en janvier 2026.

Vérifiez vos versions installées avant de continuer :

```
node --version
# v22.x.x (minimum v18)
npm --version
# 10.x.x
# Vérifier que MongoDB est en cours d'exécution
mongosh --eval "db.version()"
```
**Piège n°1 :** Si vous utilisez Node.js 16 ou antérieur, Express 5 refusera de s’installer. Mettez à jour vers Node.js 18 ou supérieur avec `nvm install 22` avant de continuer.

## Étape 1 : Initialiser le Projet et Installer Express.js

Créez un nouveau répertoire pour votre projet et initialisez-le avec npm. Nous utilisons l’option `--yes` pour générer un `package.json` par défaut, puis nous installerons Express 5 ainsi que les dépendances nécessaires.

```
# Créer et initialiser le projet
mkdir express-api-rest && cd express-api-rest
npm init --yes
# Installer Express 5 et les dépendances
npm install express@5 mongoose dotenv cors helmet jsonwebtoken bcryptjs
npm install --save-dev nodemon jest supertest
# Structure du projet
mkdir -p src/{routes,controllers,models,middleware,config,utils}
touch src/app.js src/server.js .env .gitignore
```
Express 5 est désormais la version par défaut sur npm depuis la sortie de la v5.1.0 le 31 mars 2025, qui a marqué le début de la phase ACTIVE du support de la branche 5.x selon le blog officiel d’Express.js — une phase toujours en cours à l’été 2026, le calendrier LTS d’Express.js ne prévoyant aucun changement de phase avant avril 2026. Vous n’avez plus besoin de spécifier `@next` comme c’était le cas pendant la phase de développement. Le fichier `package.json` doit inclure `"type": "module"` pour activer les imports ES modules.

Ajoutez ces lignes à votre `package.json` :

```
{
  "type": "module",
  "scripts": {
    "start": "node src/server.js",
    "dev": "nodemon src/server.js",
    "test": "node --experimental-vm-modules node_modules/.bin/jest"
  }
}
```
Configurez le fichier `.env` avec les variables d’environnement :

```
PORT=3000
MONGODB_URI=mongodb://localhost:27017/express-api
JWT_SECRET=votre_cle_secrete_ici_changez_en_production
JWT_EXPIRES_IN=24h
NODE_ENV=development
```
**Piège n°2 :** N’oubliez pas d’ajouter `.env` et `node_modules/` à votre `.gitignore`. Exposer vos clés secrètes JWT dans un dépôt Git est une faille de sécurité critique.

## Étape 2 : Configurer le Serveur Express.js 5 de Base

La configuration du serveur Express.js 5 diffère de la v4 sur plusieurs points. Le support natif des Promises élimine le besoin d’envelopper chaque handler dans un try-catch. Les middleware qui retournent des Promises rejetées sont automatiquement interceptés par le routeur.

```
// src/app.js
import express from 'express';
import cors from 'cors';
import helmet from 'helmet';
import dotenv from 'dotenv';
dotenv.config();
const app = express();
// Middleware de sécurité
app.use(helmet());
app.use(cors({
  origin: process.env.CORS_ORIGIN || 'http://localhost:3000',
  methods: ['GET', 'POST', 'PUT', 'DELETE', 'PATCH'],
  allowedHeaders: ['Content-Type', 'Authorization']
}));
// Parser JSON et URL-encoded
app.use(express.json({ limit: '10mb' }));
app.use(express.urlencoded({ extended: true }));
// Route de santé
app.get('/api/health', (req, res) => {
  res.json({
    status: 'ok',
    timestamp: new Date().toISOString(),
    environment: process.env.NODE_ENV
  });
});
export default app;
```
```
// src/server.js
import app from './app.js';
import { connectDB } from './config/database.js';
const PORT = process.env.PORT || 3000;
const startServer = async () => {
  await connectDB();
  app.listen(PORT, () => {
    console.log(`Serveur Express.js démarré sur le port ${PORT}`);
    console.log(`Environnement : ${process.env.NODE_ENV}`);
    console.log(`Santé : http://localhost:${PORT}/api/health`);
  });
};
startServer();
```
Le middleware `helmet` ajoute automatiquement 11 en-têtes HTTP de sécurité, dont `Content-Security-Policy`, `X-Content-Type-Options` et `Strict-Transport-Security`. C’est une pratique recommandée pour toute API Express.js en production.

Lancez le serveur avec `npm run dev` et testez la route de santé :

```
curl http://localhost:3000/api/health
# Sortie attendue :
{
  "status": "ok",
  "timestamp": "2026-04-10T10:30:00.000Z",
  "environment": "development"
}
```
## Étape 3 : Connexion à MongoDB avec Mongoose

MongoDB est la base de données la plus couramment associée à Express.js dans la stack MERN (MongoDB, Express, React, Node). Mongoose fournit une couche d’abstraction ODM (Object Document Mapping) qui simplifie la validation et les requêtes.

```
// src/config/database.js
import mongoose from 'mongoose';
export const connectDB = async () => {
  try {
    const conn = await mongoose.connect(process.env.MONGODB_URI, {
      maxPoolSize: 10,
      serverSelectionTimeoutMS: 5000,
      socketTimeoutMS: 45000
    });
    console.log(`MongoDB connecté : ${conn.connection.host}`);
    mongoose.connection.on('error', (err) => {
      console.error('Erreur MongoDB :', err);
    });
    mongoose.connection.on('disconnected', () => {
      console.warn('MongoDB déconnecté. Tentative de reconnexion...');
    });
  } catch (error) {
    console.error(`Erreur de connexion MongoDB : ${error.message}`);
    process.exit(1);
  }
};
```
**Piège n°3 :** Les options `useNewUrlParser` et `useUnifiedTopology` ne sont plus nécessaires avec les versions récentes de Mongoose. Les inclure génère un avertissement de dépréciation. Supprimez-les si vous migrez depuis un ancien projet.

Le pool de connexions (`maxPoolSize: 10`) est configuré pour gérer jusqu’à 10 connexions simultanées. En production, augmentez cette valeur selon votre charge, typiquement entre 20 et 50 pour une API à trafic moyen.

## Étape 4 : Créer les Modèles de Données Mongoose

Nous allons créer une API de gestion de produits avec authentification. Commençons par les modèles `User` et `Product`. La validation au niveau du schéma Mongoose constitue la première ligne de défense contre les données invalides.

```
// src/models/User.js
import mongoose from 'mongoose';
import bcrypt from 'bcryptjs';
const userSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Le nom est requis'],
    trim: true,
    minlength: [2, 'Le nom doit contenir au moins 2 caractères'],
    maxlength: [50, 'Le nom ne peut pas dépasser 50 caractères']
  },
  email: {
    type: String,
    required: [true, 'L\'email est requis'],
    unique: true,
    lowercase: true,
    match: [/^\S+@\S+\.\S+$/, 'Format d\'email invalide']
  },
  password: {
    type: String,
    required: [true, 'Le mot de passe est requis'],
    minlength: [8, 'Le mot de passe doit contenir au moins 8 caractères'],
    select: false
  },
  role: {
    type: String,
    enum: ['user', 'admin'],
    default: 'user'
  }
}, {
  timestamps: true,
  toJSON: { virtuals: true },
  toObject: { virtuals: true }
});
// Hash du mot de passe avant sauvegarde
userSchema.pre('save', async function(next) {
  if (!this.isModified('password')) return next();
  this.password = await bcrypt.hash(this.password, 12);
  next();
});
// Méthode de comparaison du mot de passe
userSchema.methods.comparePassword = async function(candidatePassword) {
  return bcrypt.compare(candidatePassword, this.password);
};
export default mongoose.model('User', userSchema);
```
```
// src/models/Product.js
import mongoose from 'mongoose';
const productSchema = new mongoose.Schema({
  name: {
    type: String,
    required: [true, 'Le nom du produit est requis'],
    trim: true,
    maxlength: [100, 'Le nom ne peut pas dépasser 100 caractères']
  },
  description: {
    type: String,
    required: [true, 'La description est requise'],
    maxlength: [2000, 'La description ne peut pas dépasser 2000 caractères']
  },
  price: {
    type: Number,
    required: [true, 'Le prix est requis'],
    min: [0, 'Le prix ne peut pas être négatif']
  },
  category: {
    type: String,
    required: [true, 'La catégorie est requise'],
    enum: ['electronique', 'vetements', 'alimentation', 'sport', 'maison']
  },
  stock: {
    type: Number,
    required: true,
    min: [0, 'Le stock ne peut pas être négatif'],
    default: 0
  },
  createdBy: {
    type: mongoose.Schema.Types.ObjectId,
    ref: 'User',
    required: true
  }
}, {
  timestamps: true
});
// Index pour les recherches fréquentes
productSchema.index({ category: 1, price: 1 });
productSchema.index({ name: 'text', description: 'text' });
export default mongoose.model('Product', productSchema);
```
Les index composites sur `category` et `price` accélèrent les requêtes de filtrage. L’index texte sur `name` et `description` permet les recherches full-text intégrées à MongoDB sans moteur de recherche externe.

## Étape 5 : Construire les Contrôleurs CRUD

Dans Express 5, les contrôleurs asynchrones n’ont plus besoin de blocs try-catch explicites grâce au support natif des Promises. Si une Promise est rejetée, Express la transmet automatiquement au middleware d’erreur. C’est l’un des changements les plus appréciés de cette version.

```
// src/controllers/productController.js
import Product from '../models/Product.js';
// GET /api/products
export const getProducts = async (req, res) => {
  const { page = 1, limit = 10, category, sort, search } = req.query;
  const filter = {};
  if (category) filter.category = category;
  if (search) filter.$text = { $search: search };
  const sortOptions = {};
  if (sort === 'price_asc') sortOptions.price = 1;
  else if (sort === 'price_desc') sortOptions.price = -1;
  else sortOptions.createdAt = -1;
  const skip = (parseInt(page) - 1) * parseInt(limit);
  const [products, total] = await Promise.all([
    Product.find(filter)
      .sort(sortOptions)
      .skip(skip)
      .limit(parseInt(limit))
      .populate('createdBy', 'name email'),
    Product.countDocuments(filter)
  ]);
  res.json({
    success: true,
    data: products,
    pagination: {
      page: parseInt(page),
      limit: parseInt(limit),
      total,
      pages: Math.ceil(total / parseInt(limit))
    }
  });
};
// GET /api/products/:id
export const getProduct = async (req, res) => {
  const product = await Product.findById(req.params.id)
    .populate('createdBy', 'name email');
  if (!product) {
    res.status(404);
    throw new Error('Produit non trouvé');
  }
  res.json({ success: true, data: product });
};
// POST /api/products
export const createProduct = async (req, res) => {
  const product = await Product.create({
    ...req.body,
    createdBy: req.user._id
  });
  res.status(201).json({ success: true, data: product });
};
// PUT /api/products/:id
export const updateProduct = async (req, res) => {
  const product = await Product.findByIdAndUpdate(
    req.params.id,
    req.body,
    { new: true, runValidators: true }
  );
  if (!product) {
    res.status(404);
    throw new Error('Produit non trouvé');
  }
  res.json({ success: true, data: product });
};
// DELETE /api/products/:id
export const deleteProduct = async (req, res) => {
  const product = await Product.findByIdAndDelete(req.params.id);
  if (!product) {
    res.status(404);
    throw new Error('Produit non trouvé');
  }
  res.status(204).send();
};
```
Remarquez l’absence totale de blocs try-catch. C’est la puissance d’Express 5 : toute erreur levée (synchrone ou asynchrone) est automatiquement interceptée par le middleware de gestion d’erreurs que nous configurerons à l’étape 8. La pagination utilise `Promise.all` pour exécuter la requête et le comptage en parallèle, réduisant le temps de réponse.

**Piège n°4 :** Avec Express 4, oublier un try-catch dans un handler asynchrone provoquait un crash silencieux du serveur. Express 5 résout ce problème, mais vous devez vous assurer d’utiliser la v5 et non la v4 pour en bénéficier.

## Étape 6 : Implémenter l’Authentification JWT

L’authentification par JSON Web Token (JWT) est le standard pour les API REST stateless. Nous allons créer un système complet avec inscription, connexion et middleware de protection des routes.

```
// src/controllers/authController.js
import jwt from 'jsonwebtoken';
import User from '../models/User.js';
const generateToken = (userId) => {
  return jwt.sign({ id: userId }, process.env.JWT_SECRET, {
    expiresIn: process.env.JWT_EXPIRES_IN
  });
};
// POST /api/auth/register
export const register = async (req, res) => {
  const { name, email, password } = req.body;
  const existingUser = await User.findOne({ email });
  if (existingUser) {
    res.status(400);
    throw new Error('Un utilisateur avec cet email existe déjà');
  }
  const user = await User.create({ name, email, password });
  const token = generateToken(user._id);
  res.status(201).json({
    success: true,
    data: {
      user: { id: user._id, name: user.name, email: user.email, role: user.role },
      token
    }
  });
};
// POST /api/auth/login
export const login = async (req, res) => {
  const { email, password } = req.body;
  if (!email || !password) {
    res.status(400);
    throw new Error('Email et mot de passe requis');
  }
  const user = await User.findOne({ email }).select('+password');
  if (!user || !(await user.comparePassword(password))) {
    res.status(401);
    throw new Error('Email ou mot de passe incorrect');
  }
  const token = generateToken(user._id);
  res.json({
    success: true,
    data: {
      user: { id: user._id, name: user.name, email: user.email, role: user.role },
      token
    }
  });
};
// GET /api/auth/me
export const getMe = async (req, res) => {
  res.json({ success: true, data: req.user });
};
```
```
// src/middleware/auth.js
import jwt from 'jsonwebtoken';
import User from '../models/User.js';
export const protect = async (req, res, next) => {
  const authHeader = req.headers.authorization;
  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    res.status(401);
    throw new Error('Accès non autorisé. Token manquant.');
  }
  const token = authHeader.split(' ')[1];
  const decoded = jwt.verify(token, process.env.JWT_SECRET);
  const user = await User.findById(decoded.id);
  if (!user) {
    res.status(401);
    throw new Error('Utilisateur associé au token introuvable');
  }
  req.user = user;
  next();
};
export const authorize = (...roles) => {
  return (req, res, next) => {
    if (!roles.includes(req.user.role)) {
      res.status(403);
      throw new Error('Accès interdit pour ce rôle');
    }
    next();
  };
};
```
Le middleware `protect` vérifie la présence et la validité du token JWT dans l’en-tête `Authorization`. Le middleware `authorize` fournit un contrôle d’accès basé sur les rôles (RBAC). Ces deux middleware peuvent être chaînés sur n’importe quelle route Express.js.

**Piège n°5 :** Ne stockez jamais les tokens JWT dans le `localStorage` côté client — c’est vulnérable aux attaques XSS. Préférez les cookies HttpOnly avec les flags `Secure` et `SameSite` pour les applications en production.

## Étape 7 : Définir les Routes de l’API REST

Express.js utilise un système de routage modulaire qui permet d’organiser les endpoints par ressource. Chaque fichier de routes est autonome et peut être monté sur un préfixe différent dans l’application principale.

```
// src/routes/authRoutes.js
import { Router } from 'express';
import { register, login, getMe } from '../controllers/authController.js';
import { protect } from '../middleware/auth.js';
const router = Router();
router.post('/register', register);
router.post('/login', login);
router.get('/me', protect, getMe);
export default router;
```
```
// src/routes/productRoutes.js
import { Router } from 'express';
import {
  getProducts,
  getProduct,
  createProduct,
  updateProduct,
  deleteProduct
} from '../controllers/productController.js';
import { protect, authorize } from '../middleware/auth.js';
const router = Router();
router.route('/')
  .get(getProducts)
  .post(protect, createProduct);
router.route('/:id')
  .get(getProduct)
  .put(protect, updateProduct)
  .delete(protect, authorize('admin'), deleteProduct);
export default router;
```
Montez les routes dans votre application principale en ajoutant ces lignes dans `src/app.js` :

```
// Ajouter dans src/app.js après les middleware existants
import authRoutes from './routes/authRoutes.js';
import productRoutes from './routes/productRoutes.js';
app.use('/api/auth', authRoutes);
app.use('/api/products', productRoutes);
```
La méthode `router.route()` permet de chaîner les handlers HTTP sur un même chemin, ce qui réduit la duplication et améliore la lisibilité. Les routes publiques (GET) ne nécessitent pas d’authentification, tandis que la suppression requiert le rôle admin.

| Méthode | Endpoint | Auth requise | Rôle | Description | 
|---|---|---|---|---|
| POST | /api/auth/register | Non | – | Inscription utilisateur | 
| POST | /api/auth/login | Non | – | Connexion et obtention du token | 
| GET | /api/auth/me | Oui | Tous | Profil de l’utilisateur connecté | 
| GET | /api/products | Non | – | Liste des produits avec pagination | 
| GET | /api/products/:id | Non | – | Détail d’un produit | 
| POST | /api/products | Oui | Tous | Créer un produit | 
| PUT | /api/products/:id | Oui | Tous | Mettre à jour un produit | 
| DELETE | /api/products/:id | Oui | Admin | Supprimer un produit | 

## Étape 8 : Gestion Centralisée des Erreurs Express.js 5

Express 5 révolutionne la gestion des erreurs avec un flux prévisible pour les erreurs synchrones et asynchrones. Le middleware d’erreur centralisé capture toutes les exceptions et fournit des réponses JSON standardisées, évitant les fuites d’informations sensibles.

```
// src/middleware/errorHandler.js
export const notFound = (req, res, next) => {
  res.status(404);
  throw new Error(`Route non trouvée : ${req.originalUrl}`);
};
export const errorHandler = (err, req, res, next) => {
  let statusCode = res.statusCode === 200 ? 500 : res.statusCode;
  let message = err.message;
  // Erreur de cast Mongoose (ID invalide)
  if (err.name === 'CastError' && err.kind === 'ObjectId') {
    statusCode = 400;
    message = 'Identifiant de ressource invalide';
  }
  // Erreur de duplication MongoDB
  if (err.code === 11000) {
    statusCode = 400;
    const field = Object.keys(err.keyValue)[0];
    message = `La valeur du champ '${field}' existe déjà`;
  }
  // Erreur de validation Mongoose
  if (err.name === 'ValidationError') {
    statusCode = 400;
    message = Object.values(err.errors).map(e => e.message).join(', ');
  }
  // Erreur JWT
  if (err.name === 'JsonWebTokenError') {
    statusCode = 401;
    message = 'Token invalide';
  }
  if (err.name === 'TokenExpiredError') {
    statusCode = 401;
    message = 'Token expiré';
  }
  res.status(statusCode).json({
    success: false,
    message,
    ...(process.env.NODE_ENV === 'development' && { stack: err.stack })
  });
};
```
Ajoutez les middleware d’erreur en dernière position dans `src/app.js` :

```
// En bas de src/app.js, APRÈS les routes
import { notFound, errorHandler } from './middleware/errorHandler.js';
app.use(notFound);
app.use(errorHandler);
```
En mode développement, la stack trace complète est incluse dans la réponse pour faciliter le débogage. En production, seul le message d’erreur est retourné. Le middleware gère automatiquement cinq types d’erreurs courants : CastError MongoDB, duplication de clé unique, validation Mongoose, et les deux erreurs JWT.

Exemple de réponse d’erreur :

```
# Requête avec un ID invalide
curl http://localhost:3000/api/products/abc123
# Réponse :
{
  "success": false,
  "message": "Identifiant de ressource invalide"
}
```
## Étape 9 : Ajouter la Validation des Données

La validation côté serveur est indispensable pour toute API REST en production. Nous utilisons express-validator pour définir des règles de validation déclaratives sur chaque route. Installez d’abord le paquet :

`npm install express-validator````
// src/middleware/validators.js
import { body, validationResult } from 'express-validator';
export const handleValidation = (req, res, next) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    res.status(400);
    throw new Error(
      errors.array().map(e => e.msg).join(', ')
    );
  }
  next();
};
export const validateRegister = [
  body('name')
    .trim()
    .notEmpty().withMessage('Le nom est requis')
    .isLength({ min: 2, max: 50 }).withMessage('Le nom doit contenir entre 2 et 50 caractères'),
  body('email')
    .trim()
    .notEmpty().withMessage('L\'email est requis')
    .isEmail().withMessage('Format d\'email invalide')
    .normalizeEmail(),
  body('password')
    .notEmpty().withMessage('Le mot de passe est requis')
    .isLength({ min: 8 }).withMessage('Le mot de passe doit contenir au moins 8 caractères')
    .matches(/\d/).withMessage('Le mot de passe doit contenir au moins un chiffre')
    .matches(/[A-Z]/).withMessage('Le mot de passe doit contenir au moins une majuscule'),
  handleValidation
];
export const validateProduct = [
  body('name')
    .trim()
    .notEmpty().withMessage('Le nom du produit est requis')
    .isLength({ max: 100 }).withMessage('Le nom ne peut pas dépasser 100 caractères'),
  body('description')
    .trim()
    .notEmpty().withMessage('La description est requise'),
  body('price')
    .isFloat({ min: 0 }).withMessage('Le prix doit être un nombre positif'),
  body('category')
    .isIn(['electronique', 'vetements', 'alimentation', 'sport', 'maison'])
    .withMessage('Catégorie invalide'),
  body('stock')
    .optional()
    .isInt({ min: 0 }).withMessage('Le stock doit être un entier positif'),
  handleValidation
];
```
Intégrez les validateurs dans les routes :

```
// Modifier src/routes/authRoutes.js
import { validateRegister } from '../middleware/validators.js';
router.post('/register', validateRegister, register);
// Modifier src/routes/productRoutes.js
import { validateProduct } from '../middleware/validators.js';
router.route('/')
  .get(getProducts)
  .post(protect, validateProduct, createProduct);
```
**Piège n°6 :** La validation doit toujours intervenir AVANT le contrôleur dans la chaîne de middleware. Placer la validation après le contrôleur est inutile car les données auront déjà été traitées.

## Étape 10 : Ajouter le Rate Limiting et la Sécurité

En mars 2026, trois vulnérabilités de sécurité ont été corrigées dans path-to-regexp, la bibliothèque de routage utilisée par Express.js, dont CVE-2026-4867 (critique) liée à une croissance exponentielle des regex. Le rate limiting et les mesures de sécurité avancées sont essentiels pour protéger votre API.

`npm install express-rate-limit hpp express-mongo-sanitize````
// Ajouter dans src/app.js
import rateLimit from 'express-rate-limit';
import mongoSanitize from 'express-mongo-sanitize';
import hpp from 'hpp';
// Rate limiting global
const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // 100 requêtes par IP
  standardHeaders: true,
  legacyHeaders: false,
  message: {
    success: false,
    message: 'Trop de requêtes. Réessayez dans 15 minutes.'
  }
});
// Rate limiting strict pour l'authentification
const authLimiter = rateLimit({
  windowMs: 15 * 60 * 1000,
  max: 5, // 5 tentatives de connexion par IP
  message: {
    success: false,
    message: 'Trop de tentatives de connexion. Réessayez dans 15 minutes.'
  }
});
app.use(limiter);
app.use('/api/auth/login', authLimiter);
app.use(mongoSanitize()); // Prévient les injections NoSQL
app.use(hpp()); // Prévient la pollution des paramètres HTTP
```
Le rate limiting à deux niveaux protège l’API contre les attaques par force brute. Le limiteur global autorise 100 requêtes par fenêtre de 15 minutes, tandis que le limiteur d’authentification restreint à 5 tentatives. `express-mongo-sanitize` supprime les opérateurs MongoDB (`$gt`, `$ne`) des entrées utilisateur pour prévenir les injections NoSQL.

| Couche de sécurité | Paquet | Protection | Impact performance | 
|---|---|---|---|
| En-têtes HTTP | helmet | 11 en-têtes de sécurité (CSP, HSTS, etc.) | Négligeable | 
| Rate limiting | express-rate-limit | Brute force, DDoS basique | Faible (stockage mémoire) | 
| Injection NoSQL | express-mongo-sanitize | Opérateurs MongoDB malveillants | Négligeable | 
| Pollution HTTP | hpp | Paramètres de requête dupliqués | Négligeable | 
| CORS | cors | Requêtes cross-origin non autorisées | Négligeable | 
| JWT | jsonwebtoken | Authentification stateless | Faible (vérification crypto) | 

## Étape 11 : Écrire les Tests avec Jest et Supertest

Les tests automatisés garantissent que votre API fonctionne correctement et permettent de détecter les régressions. Jest combiné à Supertest offre un framework de test complet pour les API Express.js.

```
// tests/product.test.js
import request from 'supertest';
import mongoose from 'mongoose';
import app from '../src/app.js';
import User from '../src/models/User.js';
import Product from '../src/models/Product.js';
let token;
let productId;
beforeAll(async () => {
  await mongoose.connect(process.env.MONGODB_URI_TEST ||
    'mongodb://localhost:27017/express-api-test');
  // Créer un utilisateur de test
  await User.deleteMany({});
  await Product.deleteMany({});
  const res = await request(app)
    .post('/api/auth/register')
    .send({
      name: 'Test User',
      email: '[email protected]',
      password: 'Password123'
    });
  token = res.body.data.token;
});
afterAll(async () => {
  await mongoose.connection.dropDatabase();
  await mongoose.connection.close();
});
describe('POST /api/products', () => {
  it('doit créer un produit avec un token valide', async () => {
    const res = await request(app)
      .post('/api/products')
      .set('Authorization', `Bearer ${token}`)
      .send({
        name: 'Laptop Pro',
        description: 'Un ordinateur portable performant',
        price: 1299.99,
        category: 'electronique',
        stock: 50
      });
    expect(res.status).toBe(201);
    expect(res.body.success).toBe(true);
    expect(res.body.data.name).toBe('Laptop Pro');
    productId = res.body.data._id;
  });
  it('doit refuser la création sans authentification', async () => {
    const res = await request(app)
      .post('/api/products')
      .send({
        name: 'Produit Test',
        description: 'Description test',
        price: 10,
        category: 'electronique'
      });
    expect(res.status).toBe(401);
  });
  it('doit refuser un produit avec un prix négatif', async () => {
    const res = await request(app)
      .post('/api/products')
      .set('Authorization', `Bearer ${token}`)
      .send({
        name: 'Produit',
        description: 'Description',
        price: -5,
        category: 'electronique'
      });
    expect(res.status).toBe(400);
  });
});
describe('GET /api/products', () => {
  it('doit retourner la liste paginée des produits', async () => {
    const res = await request(app)
      .get('/api/products?page=1&limit=10');
    expect(res.status).toBe(200);
    expect(res.body.success).toBe(true);
    expect(res.body.pagination).toBeDefined();
    expect(Array.isArray(res.body.data)).toBe(true);
  });
  it('doit filtrer par catégorie', async () => {
    const res = await request(app)
      .get('/api/products?category=electronique');
    expect(res.status).toBe(200);
    res.body.data.forEach(p => {
      expect(p.category).toBe('electronique');
    });
  });
});
```
Lancez les tests avec `npm test`. Sortie attendue :

```
$ npm test
PASS  tests/product.test.js
  POST /api/products
    ✓ doit créer un produit avec un token valide (156 ms)
    ✓ doit refuser la création sans authentification (23 ms)
    ✓ doit refuser un produit avec un prix négatif (34 ms)
  GET /api/products
    ✓ doit retourner la liste paginée des produits (45 ms)
    ✓ doit filtrer par catégorie (28 ms)
Test Suites: 1 passed, 1 total
Tests:       5 passed, 5 total
Time:        2.341 s
```
## Étape 12 : Préparer le Déploiement en Production

Le déploiement d’une API Express.js en production nécessite plusieurs ajustements : compression des réponses, logging structuré, gestion du processus et configuration HTTPS. Voici la configuration finale de production.

`npm install compression morgan````
// Ajouter dans src/app.js pour la production
import compression from 'compression';
import morgan from 'morgan';
// Logging des requêtes HTTP
if (process.env.NODE_ENV === 'development') {
  app.use(morgan('dev'));
} else {
  app.use(morgan('combined'));
}
// Compression gzip des réponses
app.use(compression());
// Endpoint de readiness pour les orchestrateurs
app.get('/api/ready', async (req, res) => {
  const dbState = mongoose.connection.readyState;
  if (dbState === 1) {
    res.json({ status: 'ready', db: 'connected' });
  } else {
    res.status(503).json({ status: 'not ready', db: 'disconnected' });
  }
});
```
Pour le déploiement avec Docker, créez un `Dockerfile` optimisé :

```
# Dockerfile
FROM node:22-alpine AS base
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production
COPY src/ ./src/
ENV NODE_ENV=production
EXPOSE 3000
USER node
HEALTHCHECK --interval=30s --timeout=3s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:3000/api/health || exit 1
CMD ["node", "src/server.js"]
```
```
# docker-compose.yml
services:
  api:
    build: .
    ports:
      - "3000:3000"
    environment:
      - PORT=3000
      - MONGODB_URI=mongodb://mongo:27017/express-api
      - JWT_SECRET=${JWT_SECRET}
      - NODE_ENV=production
    depends_on:
      mongo:
        condition: service_healthy
    restart: unless-stopped
  mongo:
    image: mongo:8
    volumes:
      - mongo_data:/data/db
    healthcheck:
      test: ["CMD", "mongosh", "--eval", "db.adminCommand('ping')"]
      interval: 10s
      timeout: 5s
      retries: 5
volumes:
  mongo_data:
```
Lancez l’application complète avec `docker compose up -d`. Le healthcheck intégré garantit que Docker redémarrera automatiquement le conteneur si l’API ne répond plus.

## Express.js 5 vs Express 4 : Les Différences Clés

La migration d’Express 4 vers Express 5 apporte des changements significatifs. Voici un résumé des différences les plus importantes pour les développeurs qui maintiennent des projets existants.

| Fonctionnalité | Express 4 | Express 5 | 
|---|---|---|
| Support des Promises | Non natif (try-catch requis) | Natif (rejet automatique capturé) | 
| Node.js minimum | 0.10+ | 18+ | 
| Routage (path-to-regexp) | v1.x (regex non sécurisé) | v8.x (protection ReDoS) | 
| app.del() | Disponible (alias) | Supprimé (utiliser app.delete()) | 
| req.host | Retourne hostname:port | Retourne uniquement hostname | 
| req.query | Setter disponible | Getter uniquement | 
| res.json(obj, status) | Signature à deux arguments | Supprimée (utiliser res.status().json()) | 
| Gestion d’erreurs async | Crash silencieux possible | Erreurs interceptées automatiquement | 

Le passage à path-to-regexp v8 est le changement de sécurité le plus important, d’autant plus critique qu’Express.js équipe désormais 138 238 domaines actifs en août 2026 selon TechnologyChecker, contre 90 377 seulement deux mois plus tôt en juin 2026 — une surface d’exposition en forte croissance. Les sous-expressions regex sont supprimées pour éliminer les vecteurs d’attaques ReDoS. Si vos routes Express 4 utilisent des patterns regex complexes, vous devrez les adapter lors de la migration vers Express 5.

## Dépannage : 8 Problèmes Courants et Solutions

Voici les problèmes les plus fréquemment rencontrés lors du développement avec Express.js 5, accompagnés de leurs solutions détaillées.

### 1. ERR_MODULE_NOT_FOUND : Cannot find module

**Cause :** Avec les ES modules, les extensions de fichier sont obligatoires dans les imports. `import app from './app'` ne fonctionne pas — vous devez écrire `import app from './app.js'`.

**Solution :** Ajoutez `.js` à tous vos imports locaux. Vérifiez que `"type": "module"` est présent dans votre `package.json`.

### 2. MongoServerError : E11000 duplicate key

**Cause :** Tentative d’insertion d’un document avec une valeur unique déjà existante (typiquement l’email). Cela arrive souvent pendant les tests quand la base n’est pas nettoyée entre les exécutions.

**Solution :** Utilisez `Model.deleteMany({})` dans le `beforeAll` de vos tests. En production, renvoyez un message d’erreur clair grâce au middleware errorHandler (code 11000).

### 3. TypeError : app.del is not a function

**Cause :** Express 5 a supprimé l’alias `app.del()`. Ce changement breaking affecte le code migré depuis Express 4.

**Solution :** Remplacez toutes les occurrences de `app.del()` par `app.delete()`.

### 4. Error : Route path must not include “?” (Express 5)

**Cause :** Express 5 utilise path-to-regexp v8 qui interdit les sous-expressions regex comme `/users/:id?` pour les paramètres optionnels.

**Solution :** Utilisez la syntaxe `{:id}` pour les paramètres optionnels : `/users{/:id}`.

### 5. JsonWebTokenError : jwt malformed

**Cause :** Le token envoyé dans l’en-tête Authorization n’est pas un JWT valide. Cela arrive quand le préfixe “Bearer ” est absent ou dupliqué.

**Solution :** Vérifiez le format exact : `Authorization: Bearer eyJhbGciOi...`. Utilisez `authHeader.split(' ')[1]` pour extraire le token.

### 6. CORS : Access-Control-Allow-Origin manquant

**Cause :** Le middleware CORS n’est pas configuré ou l’origine du frontend n’est pas dans la whitelist.

**Solution :** Configurez le middleware `cors` avec l’origine exacte de votre frontend. N’utilisez jamais `origin: '*'` en production avec des cookies ou des credentials.

### 7. EADDRINUSE : Port 3000 déjà utilisé

**Cause :** Un autre processus utilise déjà le port 3000. Cela arrive souvent quand nodemon redémarre pendant qu’une ancienne instance est encore active.

**Solution :** Identifiez le processus avec `lsof -i :3000` (Linux/Mac) ou `netstat -ano | findstr :3000` (Windows), puis terminez-le. Alternativement, changez le port dans le fichier `.env`.

### 8. Timeout de connexion MongoDB au démarrage

**Cause :** Le serveur MongoDB n’est pas démarré ou l’URI de connexion est incorrect. Avec Docker, le conteneur MongoDB peut ne pas être prêt au moment où l’API tente de se connecter.

**Solution :** Vérifiez que MongoDB est en cours d’exécution avec `mongosh --eval "db.version()"`. Avec Docker Compose, utilisez `depends_on` avec un healthcheck comme montré dans l’étape 12.

## Astuces Avancées pour Express.js 5 en Production

Une fois votre API fonctionnelle, ces techniques avancées amélioreront ses performances, sa maintenabilité et sa robustesse en production.

**Mise en cache avec Redis.** Pour les endpoints très sollicités, ajoutez une couche de cache Redis. Un middleware de cache Express peut réduire le temps de réponse de 200 ms à moins de 5 ms pour les données qui changent rarement.

**Pagination par curseur.** La pagination par offset (`skip/limit`) devient lente sur les grandes collections MongoDB car le serveur doit parcourir tous les documents précédents. La pagination par curseur utilise un `_id` de référence et est O(1) quelle que soit la page, idéale pour les collections de plus de 100 000 documents.

**Logging structuré avec Pino.** Remplacez Morgan par Pino pour un logging JSON structuré en production. Pino est jusqu’à 5 fois plus rapide que Winston et produit des logs directement exploitables par des outils comme Elasticsearch ou Grafana Loki.

**Graceful shutdown.** Implémentez un arrêt propre du serveur pour terminer les requêtes en cours avant de fermer les connexions MongoDB. C’est essentiel pour les déploiements Kubernetes avec des rolling updates.

```
// Ajouter dans src/server.js
const gracefulShutdown = async (signal) => {
  console.log(`${signal} reçu. Arrêt gracieux en cours...`);
  server.close(async () => {
    await mongoose.connection.close();
    console.log('Connexions fermées. Processus terminé.');
    process.exit(0);
  });
  // Forcer l'arrêt après 10 secondes
  setTimeout(() => {
    console.error('Arrêt forcé après timeout');
    process.exit(1);
  }, 10000);
};
process.on('SIGTERM', () => gracefulShutdown('SIGTERM'));
process.on('SIGINT', () => gracefulShutdown('SIGINT'));
```
**Versioning de l’API.** Préfixez vos routes avec un numéro de version (`/api/v1/products`) dès le départ. Cela permet d’introduire des changements breaking dans une nouvelle version (`/api/v2/products`) sans casser les clients existants.

## Structure Complète du Projet Express.js

Voici l’arborescence finale du projet complet tel que construit dans ce tutoriel. Cette structure suit les conventions de l’industrie pour une API REST Express.js maintenable et scalable.

```
express-api-rest/
├── src/
│   ├── config/
│   │   └── database.js          # Connexion MongoDB
│   ├── controllers/
│   │   ├── authController.js    # Inscription, connexion, profil
│   │   └── productController.js # CRUD produits
│   ├── middleware/
│   │   ├── auth.js              # JWT protect & authorize
│   │   ├── errorHandler.js      # Gestion centralisée des erreurs
│   │   └── validators.js        # Validation express-validator
│   ├── models/
│   │   ├── User.js              # Schéma utilisateur + hash bcrypt
│   │   └── Product.js           # Schéma produit + index
│   ├── routes/
│   │   ├── authRoutes.js        # Routes /api/auth
│   │   └── productRoutes.js     # Routes /api/products
│   ├── app.js                   # Configuration Express
│   └── server.js                # Point d'entrée + graceful shutdown
├── tests/
│   └── product.test.js          # Tests Jest + Supertest
├── .env                         # Variables d'environnement
├── .gitignore                   # Fichiers exclus du versioning
├── Dockerfile                   # Image Docker optimisée
├── docker-compose.yml           # Orchestration API + MongoDB
└── package.json                 # Dépendances et scripts
```
Ce projet contient environ 500 lignes de code réparties sur 12 fichiers. Chaque composant a une responsabilité unique : les modèles gèrent la structure des données, les contrôleurs la logique métier, les middleware les préoccupations transversales (authentification, validation, erreurs), et les routes le mapping HTTP.

## Couverture Connexe

### Articles Connexes sur tech-insider.org

Pour approfondir vos compétences en développement backend et DevOps, consultez ces guides complémentaires :

- Tutoriel FastAPI Python 2026 : Créer une API REST Complète — Comparez l’approche Python avec Express.js pour le développement d’API REST.
- Tutoriel Docker pour Débutants — Maîtrisez la conteneurisation pour déployer votre API Express.js en production.
- Tutoriel Docker Compose : Applications Multi-Conteneurs — Orchestrez Express.js et MongoDB avec Docker Compose.
- Tutoriel GraphQL avec Node.js et Apollo Server — Explorez une alternative REST avec GraphQL sur Node.js.
- Tutoriel Prometheus Grafana : Monitoring Complet — Surveillez les performances de votre API Express.js en production.
- Tutoriel GitHub Actions CI/CD — Automatisez les tests et le déploiement de votre API.

## FAQ : Questions Fréquentes sur Express.js

### Express.js est-il toujours pertinent en 2026 ?

Express.js reste le framework Node.js le plus utilisé au monde, avec environ 69 000 étoiles et 23 400 forks sur GitHub ainsi que plus de 102,4 millions de téléchargements hebdomadaires sur npm en février 2026 selon SoloDevStack. Avec la sortie d’Express 5, le framework bénéficie du support natif des Promises, d’une sécurité renforcée contre les attaques ReDoS via path-to-regexp v8, et d’un calendrier LTS officiel. Son écosystème de middleware et sa communauté active en font un choix solide pour les API REST en production.

### Quelle est la différence entre Express.js et Fastify ?

Fastify est généralement plus rapide qu’Express.js en termes de requêtes par seconde grâce à son architecture basée sur les schémas JSON. Cependant, Express.js dispose d’un écosystème de middleware beaucoup plus large et d’une communauté plus vaste. Pour les projets nécessitant des performances brutes, Fastify est préférable. Pour les projets nécessitant flexibilité et compatibilité, Express.js reste le choix pragmatique.

### Dois-je migrer d’Express 4 vers Express 5 ?

Express 4 continue de recevoir des correctifs de sécurité (dernière version 4.22.1), mais Express 5.0.0 a été officiellement publiée en janvier 2025 — dix ans après la sortie d’Express 4 — selon InfoQ, qui a également rapporté à cette occasion l’abandon du support de Node.js antérieur à la v18. La migration vers Express 5 est recommandée pour les nouveaux projets et les projets existants qui bénéficieront du support natif des Promises et de la protection ReDoS. Testez votre code avec Express 5 dans un environnement de staging avant de migrer la production.

### Comment gérer le CORS en production avec Express.js ?

Configurez le middleware `cors` avec une liste blanche d’origines autorisées. N’utilisez jamais `origin: '*'` en production si votre API utilise des cookies ou des credentials. Utilisez une variable d’environnement pour définir les origines autorisées : `CORS_ORIGIN=https://monapp.fr`.

### Quelle base de données utiliser avec Express.js ?

MongoDB avec Mongoose est le choix le plus courant dans l’écosystème Node.js (stack MERN/MEAN). Pour les données relationnelles, PostgreSQL avec Prisma ou Drizzle ORM est recommandé. Le choix dépend de la structure de vos données : MongoDB pour les documents flexibles, PostgreSQL pour les relations complexes et les transactions ACID.

### Comment sécuriser une API Express.js en production ?

Appliquez ces couches de sécurité : helmet pour les en-têtes HTTP, rate limiting pour prévenir les attaques par force brute, express-mongo-sanitize contre les injections NoSQL, validation stricte des entrées avec express-validator, authentification JWT avec des tokens courts, et HTTPS obligatoire. Maintenez Express.js et ses dépendances à jour, notamment path-to-regexp qui a reçu trois correctifs de sécurité en mars 2026.

### Express.js supporte-t-il TypeScript ?

Express.js 5 fonctionne avec TypeScript via les types `@types/express`. Vous pouvez écrire vos contrôleurs et middleware en TypeScript et compiler vers JavaScript avec `tsc` ou utiliser `tsx` pour l’exécution directe en développement. La définition de types personnalisés pour `req.user` via la déclaration de module améliore significativement l’expérience développeur.

### Comment déployer Express.js sur le cloud ?

Les options les plus courantes incluent : Docker + Kubernetes pour un contrôle total, AWS Elastic Beanstalk ou Google Cloud Run pour un déploiement simplifié, Railway ou Render pour les projets plus petits. Quelle que soit la plateforme, assurez-vous d’implémenter le healthcheck (`/api/health`), le graceful shutdown, et les variables d’environnement pour la configuration. Consultez la documentation MDN sur Express.js et le standard OpenAPI pour documenter votre API avec Swagger.