# Templates de Prompts

Collection de prompts réutilisables pour des tâches courantes.

## Catégories

- [Développement](#développement)
- [Revue de Code](#revue-de-code)
- [Debug](#debug)
- [Refactoring](#refactoring)
- [Documentation](#documentation)
- [Tests](#tests)
- [Architecture](#architecture)

---

## Développement

### Implémenter une feature

```
Je dois implémenter [description de la feature].

Contexte :
- Le projet utilise [stack technique]
- La feature doit [exigences fonctionnelles]
- Contraintes : [contraintes techniques ou métier]

Peux-tu :
1. Proposer une approche d'implémentation
2. Identifier les fichiers à créer/modifier
3. Écrire le code avec les tests associés
```

### Créer un endpoint API

```
Je dois créer un endpoint [METHOD] [/path] qui [description].

Spécifications :
- Entrée : [format des données]
- Sortie : [format de la réponse]
- Authentification : [oui/non, type]
- Validation : [règles de validation]

Génère :
1. La route avec validation
2. Les types TypeScript
3. Les tests unitaires
4. La documentation OpenAPI
```

### Créer un composant UI

```
Je dois créer un composant [nom] qui [description].

Comportement :
- Props : [liste des props]
- États : [états internes]
- Événements : [événements émis]

Contraintes :
- Accessible (WCAG AA)
- Responsive
- [autres contraintes]

Génère le composant avec ses tests.
```

---

## Revue de Code

### Revue générale

```
Fais une revue de code de ce fichier/cette PR.

Vérifie :
- Qualité et lisibilité du code
- Gestion des erreurs
- Sécurité
- Performance
- Tests suffisants
- Respect des conventions du projet

Format de sortie souhaité :
- Problèmes critiques
- Suggestions d'amélioration
- Points positifs
```

### Revue de sécurité

```
Analyse ce code du point de vue sécurité.

Cherche :
- Injections (SQL, XSS, commandes)
- Problèmes d'authentification/autorisation
- Données sensibles exposées
- Dépendances vulnérables
- Mauvaises pratiques cryptographiques

Pour chaque problème trouvé, propose une correction.
```

---

## Debug

### Analyser une erreur

```
J'ai cette erreur :

[coller l'erreur/stack trace]

Contexte :
- Le code qui cause l'erreur : [code]
- Ce que j'essaie de faire : [description]
- Ce que j'ai déjà essayé : [tentatives]

Aide-moi à :
1. Comprendre la cause
2. Trouver la solution
3. Éviter ce problème à l'avenir
```

### Problème de performance

```
Cette fonction/requête est lente :

[code ou requête]

Métriques actuelles : [temps, mémoire, etc.]
Objectif : [performance souhaitée]

Analyse les causes possibles et propose des optimisations.
```

---

## Refactoring

### Simplifier du code complexe

```
Ce code est difficile à maintenir :

[code]

Problèmes identifiés :
- [problème 1]
- [problème 2]

Refactore-le en :
- Gardant le même comportement
- Améliorant la lisibilité
- Facilitant les tests
```

### Extraire un module/service

```
Je veux extraire [fonctionnalité] du code suivant :

[code]

Crée un module/service séparé avec :
- Interface claire
- Tests
- Documentation
```

---

## Documentation

### Documenter une fonction

```
Génère la documentation JSDoc/TSDoc pour cette fonction :

[code de la fonction]

Inclus :
- Description
- Paramètres avec types
- Valeur de retour
- Exceptions possibles
- Exemple d'utilisation
```

### Créer un ADR

```
Je dois documenter une décision d'architecture :

Décision : [description]
Contexte : [pourquoi cette décision est nécessaire]
Alternatives considérées : [liste]

Génère un ADR complet avec :
- Contexte détaillé
- Décision et justification
- Conséquences positives et négatives
- Alternatives avec raisons du rejet
```

### Documenter une API

```
Génère la spécification OpenAPI pour ces endpoints :

[liste des endpoints avec leur comportement]

Inclus :
- Descriptions détaillées
- Schémas de requête/réponse
- Exemples
- Codes d'erreur possibles
```

---

## Tests

### Générer des tests unitaires

```
Génère des tests unitaires pour cette fonction :

[code]

Couvre :
- Cas nominal
- Cas limites
- Cas d'erreur
- Valeurs nulles/undefined

Framework : [vitest/jest/pytest]
```

### Générer des tests E2E

```
Génère des tests E2E pour ce parcours utilisateur :

1. [étape 1]
2. [étape 2]
3. [étape 3]

Scénarios à couvrir :
- Parcours réussi
- Erreurs de validation
- Cas d'erreur serveur

Framework : [Playwright/Cypress]
```

---

## Architecture

### Concevoir une architecture

```
Je dois concevoir l'architecture pour [description du projet/feature].

Contraintes :
- [contrainte 1]
- [contrainte 2]

Exigences non-fonctionnelles :
- Performance : [exigences]
- Scalabilité : [exigences]
- Sécurité : [exigences]

Propose :
1. Architecture globale
2. Composants principaux
3. Flux de données
4. Technologies recommandées
5. Points d'attention
```

### Évaluer une architecture

```
Évalue cette architecture :

[description ou diagramme]

Critères :
- Maintenabilité
- Scalabilité
- Testabilité
- Sécurité
- Performance

Identifie les forces, faiblesses et améliorations possibles.
```

---

## Conseils d'Utilisation

### Être spécifique

Plus le contexte est précis, meilleure sera la réponse.

### Inclure les contraintes

Mentionner les limitations techniques, de temps, ou de budget.

### Demander le format souhaité

Préciser si vous voulez du code, de la documentation, une liste, etc.

### Itérer

N'hésitez pas à demander des clarifications ou des modifications.
