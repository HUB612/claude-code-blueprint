# Git Hooks

Configuration des git hooks pour maintenir la qualité du code.

## Installation

### Avec Husky (recommandé)

```bash
# Installer Husky
npm install -D husky

# Initialiser
npx husky init

# Les hooks seront dans .husky/
```

### Sans Husky

Copier les scripts dans `.git/hooks/` et les rendre exécutables :

```bash
chmod +x .git/hooks/pre-commit
chmod +x .git/hooks/commit-msg
```

## Hooks Disponibles

### pre-commit

Exécuté avant chaque commit. Vérifie :
- Lint du code
- Formatage
- Tests des fichiers modifiés

```bash
#!/bin/sh
# .husky/pre-commit

# Lint et format des fichiers stagés
npx lint-staged

# Vérifier qu'aucun secret n'est commité
npx secretlint "**/*"
```

### commit-msg

Valide le format des messages de commit (Conventional Commits).

```bash
#!/bin/sh
# .husky/commit-msg

npx commitlint --edit "$1"
```

### pre-push

Exécuté avant un push. Vérifie :
- Tous les tests passent
- Build réussit

```bash
#!/bin/sh
# .husky/pre-push

# Lancer les tests
npm test

# Vérifier que le build fonctionne
npm run build
```

## Configuration lint-staged

```javascript
// lint-staged.config.js
export default {
  // TypeScript/JavaScript
  '*.{ts,tsx,js,jsx}': [
    'eslint --fix',
    'prettier --write',
  ],

  // Styles
  '*.{css,scss}': [
    'stylelint --fix',
    'prettier --write',
  ],

  // Autres fichiers
  '*.{json,md,yml,yaml}': [
    'prettier --write',
  ],

  // Tests associés aux fichiers modifiés
  '*.{ts,tsx}': () => 'npm run test:related',
}
```

## Configuration commitlint

```javascript
// commitlint.config.js
export default {
  extends: ['@commitlint/config-conventional'],
  rules: {
    // Types autorisés
    'type-enum': [
      2,
      'always',
      [
        'feat',     // Nouvelle fonctionnalité
        'fix',      // Correction de bug
        'docs',     // Documentation
        'style',    // Formatage (pas de changement de code)
        'refactor', // Refactoring
        'perf',     // Optimisation de performance
        'test',     // Ajout/modification de tests
        'chore',    // Maintenance
        'ci',       // CI/CD
        'revert',   // Revert d'un commit
      ],
    ],

    // Scopes autorisés (adapter selon votre projet)
    'scope-enum': [
      2,
      'always',
      [
        'core',
        'api',
        'ui',
        'auth',
        'db',
        'tests',
        'ci',
        'docs',
      ],
    ],

    // Longueur du sujet
    'subject-max-length': [2, 'always', 72],

    // Pas de point à la fin
    'subject-full-stop': [2, 'never', '.'],

    // Minuscule pour le sujet
    'subject-case': [2, 'always', 'lower-case'],
  },
}
```

## Bonnes Pratiques

### Garder les hooks rapides

- Pre-commit : < 10 secondes
- Commit-msg : < 1 seconde
- Pre-push : peut être plus long

### Permettre de contourner en cas d'urgence

```bash
# Contourner pre-commit (à utiliser avec parcimonie)
git commit --no-verify -m "fix: correction urgente"
```

### Documenter les hooks

Expliquer pourquoi chaque hook existe et ce qu'il vérifie.

## Dépannage

### Hook qui échoue sans raison claire

```bash
# Exécuter manuellement pour voir les erreurs
.husky/pre-commit
```

### lint-staged ne trouve pas les fichiers

Vérifier que les patterns glob sont corrects et que les fichiers sont bien stagés.

### commitlint rejette un message valide

Vérifier la configuration et les scopes autorisés.
