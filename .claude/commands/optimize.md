# Commande /optimize

Analyse et suggère des optimisations de performance.

## Usage

```
/optimize [cible]
```

### Cibles disponibles
- `[fichier]` - Analyse un fichier spécifique
- `queries` - Focus sur les requêtes de base de données
- `bundle` - Analyse de la taille du bundle
- `runtime` - Optimisations runtime
- `memory` - Analyse de l'utilisation mémoire

## Points d'Analyse

### Performance Base de Données

- [ ] Requêtes N+1
- [ ] Index manquants
- [ ] Requêtes non optimisées
- [ ] Pagination inefficace
- [ ] SELECT * évitables

### Performance Frontend

- [ ] Bundle size
- [ ] Code splitting
- [ ] Lazy loading
- [ ] Images non optimisées
- [ ] CSS non utilisé
- [ ] Renders inutiles

### Performance Backend

- [ ] Opérations bloquantes
- [ ] Caching manquant
- [ ] Connexions non poolées
- [ ] Sérialisation coûteuse
- [ ] Calculs redondants

### Mémoire

- [ ] Fuites potentielles
- [ ] Collections non bornées
- [ ] Closures retenant des références
- [ ] Streams non fermés

## Format de Sortie

```markdown
## Rapport d'Optimisation

**Cible** : [fichier/scope]
**Impact potentiel** : [Critique|Élevé|Moyen|Faible]

### Résumé des Opportunités

| Catégorie | Opportunités | Impact Estimé |
|-----------|--------------|---------------|
| Base de données | X | Élevé |
| Frontend | X | Moyen |
| Backend | X | Faible |

### Optimisations Recommandées

#### [OPT-001] Titre - Impact: Élevé
**Localisation** : fichier:ligne

**Problème actuel** :
```code
// Code actuel
```

**Solution proposée** :
```code
// Code optimisé
```

**Gain estimé** : Description du gain

---

### Métriques Suggérées
- Mesurer X avant/après
- Monitorer Y

### Actions Prioritaires
1. ...
2. ...
```

## Exemples d'Optimisations

### Requête N+1

```typescript
// ❌ Problème : N+1 queries
const users = await db.users.findMany()
for (const user of users) {
  user.posts = await db.posts.findMany({ where: { userId: user.id } })
}

// ✅ Solution : Include/Join
const users = await db.users.findMany({
  include: { posts: true }
})
```

### Pagination Inefficace

```typescript
// ❌ Problème : OFFSET lent sur grandes tables
const users = await db.users.findMany({
  skip: page * limit,
  take: limit
})

// ✅ Solution : Cursor-based pagination
const users = await db.users.findMany({
  cursor: { id: lastId },
  take: limit,
  skip: 1 // Skip the cursor
})
```

### Calcul Redondant

```typescript
// ❌ Problème : Calcul dans la boucle
items.map(item => ({
  ...item,
  tax: item.price * getTaxRate() // Appel répété
}))

// ✅ Solution : Calcul avant la boucle
const taxRate = getTaxRate()
items.map(item => ({
  ...item,
  tax: item.price * taxRate
}))
```

### Mémoire : Array en croissance

```typescript
// ❌ Problème : Accumulation en mémoire
const allResults = []
for await (const batch of fetchBatches()) {
  allResults.push(...batch)
}
process(allResults)

// ✅ Solution : Streaming
for await (const batch of fetchBatches()) {
  await processBatch(batch)
}
```

## Exemple

```
/optimize src/services/order-service.ts
```

Sortie :
```
## Rapport d'Optimisation

**Cible** : src/services/order-service.ts
**Impact potentiel** : Élevé

### Résumé des Opportunités

| Catégorie | Opportunités | Impact Estimé |
|-----------|--------------|---------------|
| Base de données | 2 | Élevé |
| Backend | 1 | Moyen |

### Optimisations Recommandées

#### [OPT-001] Requête N+1 dans getOrdersWithItems - Impact: Élevé
**Localisation** : order-service.ts:45

**Problème actuel** :
```typescript
const orders = await db.orders.findMany()
for (const order of orders) {
  order.items = await db.orderItems.findMany({ orderId: order.id })
}
```

**Solution proposée** :
```typescript
const orders = await db.orders.findMany({
  include: { items: true }
})
```

**Gain estimé** : Réduction de N requêtes à 1 requête

### Actions Prioritaires
1. Corriger la requête N+1 (ligne 45)
2. Ajouter un index sur orderItems.orderId
```
