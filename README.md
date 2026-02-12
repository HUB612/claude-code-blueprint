# Claude Code Blueprint

Un ensemble de fichiers et bonnes pratiques pour utiliser Claude Code efficacement dans vos projets de développement.

## Contenu

```
.
├── CLAUDE.md                    # Template de configuration projet
├── .claude/
│   ├── settings.json            # Configuration des agents et workflow
│   ├── agents/                  # Agents spécialisés
│   │   ├── orchestrator.md      # Coordinateur d'équipe
│   │   ├── backend.md           # Spécialiste backend
│   │   ├── frontend.md          # Spécialiste frontend
│   │   ├── qa.md                # Spécialiste qualité/tests
│   │   ├── docs.md              # Spécialiste documentation
│   │   ├── security.md          # Spécialiste sécurité
│   │   ├── devops.md            # Spécialiste DevOps/CI-CD
│   │   ├── database.md          # Spécialiste base de données
│   │   ├── api-design.md        # Spécialiste conception d'API
│   │   └── refactoring.md       # Spécialiste refactoring
│   ├── commands/                # Commandes personnalisées (skills)
│   │   ├── review.md            # Revue de code
│   │   ├── security-audit.md    # Audit de sécurité
│   │   ├── optimize.md          # Optimisation de performance
│   │   ├── document.md          # Génération de documentation
│   │   └── test-gen.md          # Génération de tests
│   ├── hooks/                   # Git hooks
│   │   └── README.md            # Guide de configuration
│   └── prompts/                 # Templates de prompts
│       └── README.md            # Catalogue de prompts
└── README.md                    # Ce fichier
```

## Installation

1. **Copier les fichiers** dans votre projet :

```bash
# Cloner le blueprint
git clone https://github.com/HUB612/claude-code-blueprint.git

# Copier dans votre projet
cp -r claude-code-blueprint/.claude votre-projet/
cp claude-code-blueprint/CLAUDE.md votre-projet/
```

2. **Personnaliser CLAUDE.md** selon votre projet :
   - Remplacer les sections entre `[crochets]`
   - Adapter la stack technique
   - Définir vos conventions

3. **Configurer settings.json** :
   - Ajuster le nom du projet
   - Sélectionner les agents pertinents
   - Définir vos scopes de commit

## Utilisation des Agents

Les agents sont des rôles spécialisés que Claude peut adopter selon le contexte :

| Agent | Rôle | Quand l'utiliser |
|-------|------|------------------|
| `orchestrator` | Coordination | Projets multi-agents, planification |
| `backend` | Code serveur | APIs, logique métier, BDD |
| `frontend` | Interface | Composants, pages, UX |
| `qa` | Tests | Tests unitaires, E2E, CI |
| `docs` | Documentation | API docs, guides, ADRs |
| `security` | Sécurité | Audits, vulnérabilités, auth |
| `devops` | Infrastructure | CI/CD, Docker, déploiement |
| `database` | Base de données | Schémas, migrations, requêtes |
| `api-design` | Conception API | REST, GraphQL, OpenAPI |
| `refactoring` | Amélioration | Refactoring, dette technique |

## Commandes Personnalisées

Les commandes (skills) sont des actions prédéfinies :

```
/review          # Revue de code du fichier ou de la PR
/security-audit  # Analyse de sécurité
/optimize        # Suggestions d'optimisation
/document        # Génération de documentation
/test-gen        # Génération de tests
```

## Bonnes Pratiques

### 1. Contexte du projet

Toujours maintenir un `CLAUDE.md` à jour avec :
- La stack technique actuelle
- Les conventions de l'équipe
- La structure du projet

### 2. Commits

Utiliser les Conventional Commits :
```
feat(scope): nouvelle fonctionnalité
fix(scope): correction de bug
docs(scope): documentation
test(scope): ajout/modification de tests
refactor(scope): refactoring sans changement fonctionnel
```

### 3. Pull Requests

- Lier à une issue quand applicable
- Décrire les changements et leur motivation
- Inclure les étapes de test

### 4. Sécurité

- Ne jamais commiter de secrets
- Utiliser des variables d'environnement
- Faire auditer le code sensible

## Contribuer

Les contributions sont bienvenues ! Voir les [issues ouvertes](https://github.com/HUB612/claude-code-blueprint/issues) pour les améliorations planifiées.

## Licence

MIT
