# Commande /test-gen

Génère des tests pour le code spécifié.

## Usage

```
/test-gen [fichier] [options]
```

### Options
- `--type=[unit|integration|e2e]` - Type de tests (défaut: unit)
- `--framework=[vitest|jest|pytest|playwright]` - Framework de test
- `--coverage` - Générer des tests pour atteindre une couverture cible

## Comportement

1. Analyse le fichier source
2. Identifie les fonctions/méthodes à tester
3. Génère des cas de test couvrant :
   - Cas nominal (happy path)
   - Cas limites (edge cases)
   - Cas d'erreur
   - Valeurs nulles/undefined

## Structure des Tests Générés

### Tests Unitaires

```typescript
import { describe, it, expect, vi, beforeEach } from 'vitest'
import { createUser } from './user-service'

describe('createUser', () => {
  // Setup
  const mockDb = {
    users: {
      create: vi.fn(),
      findUnique: vi.fn(),
    },
  }

  beforeEach(() => {
    vi.clearAllMocks()
  })

  describe('cas nominal', () => {
    it('devrait créer un utilisateur avec des données valides', async () => {
      // Arrange
      const input = { email: 'test@example.com', password: 'SecureP@ss123' }
      mockDb.users.findUnique.mockResolvedValue(null)
      mockDb.users.create.mockResolvedValue({ id: '1', ...input })

      // Act
      const result = await createUser(mockDb)(input)

      // Assert
      expect(result).toHaveProperty('id')
      expect(result.email).toBe(input.email)
      expect(mockDb.users.create).toHaveBeenCalledWith(
        expect.objectContaining({ email: input.email })
      )
    })
  })

  describe('validation', () => {
    it('devrait rejeter un email invalide', async () => {
      const input = { email: 'invalid', password: 'SecureP@ss123' }

      await expect(createUser(mockDb)(input))
        .rejects.toThrow('Email invalide')
    })

    it('devrait rejeter un mot de passe trop court', async () => {
      const input = { email: 'test@example.com', password: '123' }

      await expect(createUser(mockDb)(input))
        .rejects.toThrow('8 caractères minimum')
    })
  })

  describe('cas d\'erreur', () => {
    it('devrait rejeter si l\'email existe déjà', async () => {
      const input = { email: 'existing@example.com', password: 'SecureP@ss123' }
      mockDb.users.findUnique.mockResolvedValue({ id: '1' })

      await expect(createUser(mockDb)(input))
        .rejects.toThrow('Email déjà utilisé')
    })

    it('devrait propager les erreurs de base de données', async () => {
      const input = { email: 'test@example.com', password: 'SecureP@ss123' }
      mockDb.users.findUnique.mockResolvedValue(null)
      mockDb.users.create.mockRejectedValue(new Error('DB Error'))

      await expect(createUser(mockDb)(input))
        .rejects.toThrow('DB Error')
    })
  })

  describe('cas limites', () => {
    it('devrait accepter un email avec des caractères spéciaux valides', async () => {
      const input = { email: 'test+tag@sub.example.com', password: 'SecureP@ss123' }
      mockDb.users.findUnique.mockResolvedValue(null)
      mockDb.users.create.mockResolvedValue({ id: '1', ...input })

      const result = await createUser(mockDb)(input)

      expect(result.email).toBe(input.email)
    })
  })
})
```

### Tests d'Intégration

```typescript
import { describe, it, expect, beforeAll, afterAll } from 'vitest'
import { createTestDb, cleanupTestDb } from '../test-utils/db'
import { createUserService } from './user-service'

describe('UserService Integration', () => {
  let db: TestDb
  let userService: UserService

  beforeAll(async () => {
    db = await createTestDb()
    userService = createUserService(db)
  })

  afterAll(async () => {
    await cleanupTestDb(db)
  })

  it('devrait créer et récupérer un utilisateur', async () => {
    // Créer
    const created = await userService.create({
      email: 'integration@test.com',
      password: 'SecureP@ss123',
    })

    // Récupérer
    const found = await userService.findById(created.id)

    expect(found).not.toBeNull()
    expect(found?.email).toBe('integration@test.com')
  })
})
```

### Tests E2E

```typescript
import { test, expect } from '@playwright/test'

test.describe('Création de compte', () => {
  test('devrait permettre de créer un compte', async ({ page }) => {
    await page.goto('/register')

    await page.getByLabel('Email').fill('e2e@test.com')
    await page.getByLabel('Mot de passe').fill('SecureP@ss123')
    await page.getByRole('button', { name: 'Créer mon compte' }).click()

    await expect(page).toHaveURL('/dashboard')
    await expect(page.getByText('Bienvenue')).toBeVisible()
  })

  test('devrait afficher une erreur pour un email existant', async ({ page }) => {
    // Pré-condition : créer un utilisateur
    await page.goto('/register')

    await page.getByLabel('Email').fill('existing@test.com')
    await page.getByLabel('Mot de passe').fill('SecureP@ss123')
    await page.getByRole('button', { name: 'Créer mon compte' }).click()

    await expect(page.getByText('Email déjà utilisé')).toBeVisible()
  })
})
```

## Format de Sortie

```markdown
## Tests Générés

**Fichier source** : src/services/user-service.ts
**Fichier de test** : src/services/user-service.test.ts
**Tests créés** : X

### Couverture Estimée

| Fonction | Lignes | Branches |
|----------|--------|----------|
| createUser | 100% | 85% |
| findById | 100% | 100% |

### Tests Créés

- `createUser`
  - [x] Cas nominal - création avec données valides
  - [x] Validation - email invalide
  - [x] Validation - mot de passe trop court
  - [x] Erreur - email existant
  - [x] Erreur - erreur BDD
  - [x] Limite - email avec caractères spéciaux

### Fichier Généré

\`\`\`typescript
[contenu du fichier de test]
\`\`\`
```

## Exemple

```
/test-gen src/services/order-service.ts --type=unit --framework=vitest
```

## Bonnes Pratiques

### Structure AAA
- **Arrange** : Préparer les données et mocks
- **Act** : Exécuter l'action à tester
- **Assert** : Vérifier le résultat

### Nommage
- `devrait [comportement attendu]`
- `devrait [action] quand [condition]`

### Isolation
- Chaque test indépendant des autres
- Nettoyage après chaque test
- Mocks réinitialisés entre les tests
