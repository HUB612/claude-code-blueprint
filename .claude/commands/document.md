# Commande /document

Génère ou met à jour la documentation pour le code spécifié.

## Usage

```
/document [cible] [options]
```

### Cibles
- `[fichier]` - Documente un fichier spécifique
- `api` - Génère la documentation API (OpenAPI)
- `readme` - Met à jour le README
- `adr` - Crée un Architecture Decision Record

### Options
- `--format=[md|jsdoc|openapi]` - Format de sortie
- `--lang=[fr|en]` - Langue de la documentation

## Types de Documentation

### Documentation de Code (JSDoc/TSDoc)

```typescript
/**
 * Crée un nouvel utilisateur dans le système.
 *
 * @param data - Les données de l'utilisateur à créer
 * @param data.email - L'adresse email (doit être unique)
 * @param data.password - Le mot de passe (min 8 caractères)
 * @returns L'utilisateur créé avec son ID
 * @throws {ValidationError} Si les données sont invalides
 * @throws {ConflictError} Si l'email existe déjà
 *
 * @example
 * ```typescript
 * const user = await createUser({
 *   email: 'john@example.com',
 *   password: 'SecureP@ss123'
 * })
 * console.log(user.id) // "550e8400-..."
 * ```
 */
export async function createUser(data: CreateUserInput): Promise<User> {
  // ...
}
```

### Documentation API (OpenAPI)

```yaml
paths:
  /users:
    post:
      summary: Créer un utilisateur
      description: |
        Crée un nouvel utilisateur dans le système.
        L'email doit être unique.
      tags:
        - Users
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateUser'
            example:
              email: "john@example.com"
              password: "SecureP@ss123"
      responses:
        '201':
          description: Utilisateur créé avec succès
```

### Architecture Decision Record (ADR)

```markdown
# ADR-XXX: [Titre de la Décision]

## Statut
[Proposé | Accepté | Déprécié | Remplacé par ADR-YYY]

## Contexte
[Quel est le problème ou le besoin qui motive cette décision ?]

## Décision
[Quelle est la décision prise ?]

## Conséquences

### Positives
- [Avantage 1]
- [Avantage 2]

### Négatives
- [Inconvénient 1]
- [Inconvénient 2]

## Alternatives Considérées

### [Alternative 1]
- **Description** : ...
- **Raison du rejet** : ...

### [Alternative 2]
- **Description** : ...
- **Raison du rejet** : ...
```

## Format de Sortie

### Pour un fichier

```markdown
## Documentation Générée

**Fichier** : src/services/user-service.ts
**Fonctions documentées** : X
**Types documentés** : Y

### Fonctions

#### `createUser(data: CreateUserInput): Promise<User>`
Crée un nouvel utilisateur...

[Documentation JSDoc générée]

### Types

#### `User`
Représente un utilisateur du système...

#### `CreateUserInput`
Données requises pour créer un utilisateur...
```

### Pour l'API

```markdown
## Documentation API Générée

**Endpoints documentés** : X
**Schémas documentés** : Y

**Fichier généré** : docs/api/openapi.yaml

### Aperçu des Endpoints

| Méthode | Path | Description |
|---------|------|-------------|
| POST | /users | Créer un utilisateur |
| GET | /users/{id} | Obtenir un utilisateur |
...
```

## Exemples

### Documenter un fichier

```
/document src/services/user-service.ts
```

### Générer la doc API

```
/document api --format=openapi
```

### Créer un ADR

```
/document adr "Choix de PostgreSQL comme base de données"
```

## Bonnes Pratiques de Documentation

### Ce qu'il faut documenter
- Fonctions publiques/exportées
- Types et interfaces complexes
- Comportements non évidents
- Cas limites et erreurs possibles
- Exemples d'utilisation

### Ce qu'il ne faut pas documenter
- Code évident (getters/setters simples)
- Implémentation interne
- Ce que le nom explique déjà

### Style
- Phrases complètes avec ponctuation
- Voix active
- Descriptions concises mais complètes
- Exemples de code fonctionnels
