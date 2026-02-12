# Commande /review

Effectue une revue de code du fichier ou de la PR actuelle.

## Usage

```
/review [fichier|PR]
```

## Comportement

### Sans argument
- Analyse les changements non commités dans le répertoire courant
- Vérifie les fichiers stagés (`git diff --staged`)

### Avec un fichier
- Analyse le fichier spécifié en détail

### Avec un numéro de PR
- Analyse tous les changements de la Pull Request

## Points de Vérification

### 1. Qualité du Code
- [ ] Lisibilité et clarté
- [ ] Nommage des variables et fonctions
- [ ] Complexité des fonctions
- [ ] Duplication de code
- [ ] Respect des conventions du projet

### 2. Logique et Fonctionnalité
- [ ] Cas limites gérés
- [ ] Gestion des erreurs
- [ ] Validation des entrées
- [ ] Comportement attendu

### 3. Sécurité
- [ ] Injection (SQL, XSS, commandes)
- [ ] Authentification/Autorisation
- [ ] Données sensibles exposées
- [ ] Validation des entrées utilisateur

### 4. Performance
- [ ] Requêtes N+1
- [ ] Opérations coûteuses dans les boucles
- [ ] Mémoire/Fuites potentielles
- [ ] Caching approprié

### 5. Tests
- [ ] Tests présents pour les nouveaux changements
- [ ] Couverture suffisante
- [ ] Cas de test pertinents

### 6. Documentation
- [ ] Commentaires où nécessaire
- [ ] JSDoc/TSDoc pour les fonctions publiques
- [ ] README mis à jour si applicable

## Format de Sortie

```markdown
## Résumé de la Revue

**Fichiers analysés** : X
**Problèmes trouvés** : Y (X critiques, Y majeurs, Z mineurs)

### Problèmes Critiques
- [fichier:ligne] Description du problème
  Suggestion : ...

### Problèmes Majeurs
- [fichier:ligne] Description
  Suggestion : ...

### Points Mineurs
- [fichier:ligne] Description
  Suggestion : ...

### Points Positifs
- Bonne utilisation de...
- Structure claire de...

### Recommandations Générales
- ...
```

## Exemple

```
/review src/services/user-service.ts
```

Sortie :
```
## Résumé de la Revue

**Fichiers analysés** : 1
**Problèmes trouvés** : 3 (0 critiques, 1 majeur, 2 mineurs)

### Problèmes Majeurs
- [user-service.ts:45] Pas de validation de l'email avant insertion
  Suggestion : Ajouter une validation avec Zod ou une regex

### Points Mineurs
- [user-service.ts:12] Variable `d` peu descriptive
  Suggestion : Renommer en `userData` ou `userDetails`
- [user-service.ts:78] Fonction de 35 lignes, pourrait être découpée
  Suggestion : Extraire la logique de validation

### Points Positifs
- Bonne gestion des erreurs avec try/catch
- Types TypeScript bien définis
```
