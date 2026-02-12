# [Nom du Projet] - Guide de Développement

<!--
  INSTRUCTIONS : Ce fichier est un template à personnaliser pour votre projet.
  Remplacez les sections entre [crochets] par vos informations.
  Supprimez les commentaires HTML une fois le fichier configuré.
-->

## Présentation du Projet

<!-- Décrivez brièvement votre projet en 2-3 phrases -->
[Description courte du projet et de son objectif principal]

## Stack Technique

<!-- Listez les technologies principales de votre projet -->

- **Frontend** : [Framework frontend - ex: React, Vue, Svelte, Angular]
- **Backend** : [Framework backend - ex: Node.js/Express, Python/FastAPI, Go/Gin]
- **Base de données** : [SGBD - ex: PostgreSQL, MongoDB, SQLite]
- **UI** : [Bibliothèque UI - ex: shadcn, Material UI, Tailwind]
- **Tests** : [Frameworks de test - ex: Jest, Vitest, Pytest, Playwright]
- **Linting** : [Outils de qualité - ex: ESLint, Biome, Ruff]
- **CI/CD** : [Plateforme - ex: GitHub Actions, GitLab CI]

## Architecture

<!--
  Décrivez la structure de votre projet.
  Adaptez selon votre architecture (monorepo, microservices, Clean Architecture, etc.)
-->

```
src/
├── [dossier]/           # [Description]
│   ├── [sous-dossier]/  # [Description]
│   └── [sous-dossier]/  # [Description]
├── [dossier]/           # [Description]
└── [dossier]/           # [Description]
```

### Principes Architecturaux

<!-- Listez les principes que vous suivez -->

- [Principe 1 - ex: Séparation des responsabilités]
- [Principe 2 - ex: Injection de dépendances]
- [Principe 3 - ex: Tests unitaires pour la logique métier]

## Conventions de Code

### TypeScript / JavaScript

<!-- Adaptez selon votre langage principal -->

- Mode strict activé
- Types explicites sur les fonctions publiques
- Préférer `type` pour les structures de données, `interface` pour les contrats
- Préférer `unknown` à `any`

### Nommage

| Élément | Convention | Exemple |
|---------|------------|---------|
| Fichiers | kebab-case | `user-service.ts` |
| Composants | PascalCase | `UserProfile.svelte` |
| Fonctions | camelCase | `getUserById` |
| Constantes | SCREAMING_SNAKE_CASE | `MAX_RETRY_COUNT` |
| Types/Interfaces | PascalCase | `UserProfile` |

### Principes de Code Propre

- **Responsabilité unique** : une fonction = un objectif
- **Fonctions courtes** : < 20 lignes quand possible
- **Pas de nombres magiques** : utiliser des constantes nommées
- **Retours anticipés** : éviter les conditions imbriquées
- **Noms descriptifs** : préférer des noms clairs aux commentaires

### Exports

- Exports nommés uniquement : `export const MyComponent`
- Pas d'exports par défaut
- Fichiers barrel (`index.ts`) pour l'API publique de chaque module

## Workflow Git

### Nommage des Branches

```
feature/[issue-number]-[description-courte]
fix/[issue-number]-[description-courte]
hotfix/[description-courte]
refactor/[description-courte]
```

### Messages de Commit (Conventional Commits)

```
type(scope): description

Types: feat, fix, docs, style, refactor, test, chore, perf
Scopes: [liste des scopes de votre projet]
```

<!-- Définissez vos scopes selon vos modules/domaines -->
**Scopes du projet** : `core`, `api`, `ui`, `auth`, `db`, `tests`, `ci`

**Exemples** :
```
feat(auth): ajouter l'authentification OAuth2
fix(api): corriger la validation des entrées utilisateur
test(core): ajouter tests unitaires pour le service de calcul
docs(api): documenter les endpoints REST
```

### Processus de Pull Request

1. Créer une branche depuis `main`
2. Développer avec des commits atomiques
3. S'assurer que les tests passent
4. Pousser et créer une PR
5. Attendre la revue humaine avant merge

### Format du Titre de PR

```
[#issue] type(scope): description
```

Exemple : `[#42] feat(auth): ajouter la connexion Google`

## Tests

### Tests Unitaires

- Co-localisés avec le code source : `*.test.ts` ou `*.spec.ts`
- Tester la logique métier en priorité
- Mocker les dépendances externes (API, BDD)

### Tests E2E

- Situés dans `tests/e2e/` ou `e2e/`
- Tester les parcours utilisateur complets
- Utiliser des attributs `data-testid` pour les sélecteurs

### Couverture

- Seuil minimum : [80%]
- Vérifié en CI

## Portes de Qualité

Avant merge, vérifier :

- [ ] Lint passe sans erreur
- [ ] Tous les tests passent
- [ ] Couverture >= [80%]
- [ ] Build réussit
- [ ] Revue humaine approuvée

## Développement Local

```bash
# Installer les dépendances
[commande d'installation - ex: pnpm install]

# Lancer le serveur de développement
[commande de dev - ex: pnpm dev]

# Lancer les tests
[commande de test - ex: pnpm test]

# Lancer les tests E2E
[commande e2e - ex: pnpm test:e2e]

# Linter et formater
[commande lint - ex: pnpm lint]
```

### Services Docker (si applicable)

<!-- Listez vos services Docker si vous en avez -->

| Service | Port | Description |
|---------|------|-------------|
| [service] | [port] | [description] |

## Variables d'Environnement

<!-- Listez les variables d'environnement nécessaires (sans les valeurs sensibles) -->

```env
# Base de données
DATABASE_URL=

# API
API_URL=
API_KEY=

# Authentification (si applicable)
AUTH_SECRET=
```

## Labels de Domaine

<!-- Définissez les labels pour catégoriser les issues/PRs -->

- `domain:core` - Fonctionnalités principales
- `domain:api` - API et endpoints
- `domain:ui` - Interface utilisateur
- `domain:auth` - Authentification et autorisation
- `domain:infra` - Infrastructure et déploiement

## Ressources

<!-- Liens vers la documentation, design, etc. -->

- Documentation : [lien]
- Design/Figma : [lien]
- API docs : [lien]
