# Commande /security-audit

Effectue un audit de sécurité du code ou du projet.

## Usage

```
/security-audit [scope]
```

### Scopes disponibles
- `full` - Audit complet du projet (défaut)
- `deps` - Audit des dépendances uniquement
- `code` - Audit du code source uniquement
- `config` - Audit des configurations
- `auth` - Focus sur l'authentification/autorisation
- `[fichier]` - Audit d'un fichier spécifique

## Points de Vérification

### OWASP Top 10

#### A01 - Contrôle d'Accès Défaillant
- [ ] Vérifications d'autorisation sur tous les endpoints
- [ ] Pas d'accès direct aux ressources par ID
- [ ] Principe du moindre privilège

#### A02 - Défaillances Cryptographiques
- [ ] Mots de passe hachés (Argon2, bcrypt)
- [ ] Pas de secrets en dur dans le code
- [ ] HTTPS forcé
- [ ] Tokens JWT sécurisés

#### A03 - Injection
- [ ] Requêtes SQL/NoSQL paramétrées
- [ ] Échappement des sorties HTML
- [ ] Pas d'exécution de commandes avec input utilisateur

#### A04 - Conception Non Sécurisée
- [ ] Validation des entrées
- [ ] Rate limiting
- [ ] Threat modeling considéré

#### A05 - Mauvaise Configuration
- [ ] Headers de sécurité (CORS, CSP, etc.)
- [ ] Mode debug désactivé en prod
- [ ] Ports et services minimaux exposés

#### A06 - Composants Vulnérables
- [ ] Dépendances à jour
- [ ] Pas de vulnérabilités connues
- [ ] Licences compatibles

#### A07 - Authentification Défaillante
- [ ] Sessions sécurisées
- [ ] Tokens avec expiration
- [ ] Protection contre brute force

#### A08 - Intégrité Logicielle
- [ ] CI/CD sécurisé
- [ ] Signatures vérifiées
- [ ] Dépendances de sources fiables

#### A09 - Logging et Monitoring
- [ ] Logs des événements de sécurité
- [ ] Pas de données sensibles dans les logs
- [ ] Alertes configurées

#### A10 - SSRF
- [ ] Validation des URLs externes
- [ ] Restrictions réseau

### Fichiers Sensibles

- [ ] `.env` non commité
- [ ] Pas de credentials dans le code
- [ ] Pas de clés privées dans le repo
- [ ] `.gitignore` approprié

### Configuration

- [ ] CORS restrictif
- [ ] CSP configuré
- [ ] Cookies sécurisés (HttpOnly, Secure, SameSite)

## Format de Sortie

```markdown
## Rapport d'Audit de Sécurité

**Date** : YYYY-MM-DD
**Scope** : [full|deps|code|config|auth|fichier]
**Niveau de risque global** : [Critique|Élevé|Moyen|Faible]

### Résumé

| Sévérité | Nombre |
|----------|--------|
| Critique | X |
| Élevée   | X |
| Moyenne  | X |
| Faible   | X |

### Vulnérabilités Critiques

#### [VULN-001] Titre
- **Localisation** : fichier:ligne
- **Description** : ...
- **Impact** : ...
- **Remédiation** : ...
- **Référence** : CWE-XXX / OWASP A0X

### Vulnérabilités Élevées
...

### Vulnérabilités Moyennes
...

### Points d'Attention
...

### Recommandations Prioritaires
1. ...
2. ...
3. ...

### Dépendances Vulnérables
| Package | Version | Vulnérabilité | Sévérité | Fix |
|---------|---------|---------------|----------|-----|
| ... | ... | ... | ... | ... |
```

## Exemple

```
/security-audit auth
```

Sortie :
```
## Rapport d'Audit de Sécurité

**Date** : 2024-01-15
**Scope** : auth
**Niveau de risque global** : Moyen

### Résumé

| Sévérité | Nombre |
|----------|--------|
| Critique | 0 |
| Élevée   | 1 |
| Moyenne  | 2 |
| Faible   | 1 |

### Vulnérabilités Élevées

#### [VULN-001] Token JWT sans expiration
- **Localisation** : src/auth/jwt.ts:23
- **Description** : Les tokens JWT sont créés sans option `expiresIn`
- **Impact** : Un token compromis reste valide indéfiniment
- **Remédiation** : Ajouter `expiresIn: '15m'` pour les access tokens
- **Référence** : CWE-613 / OWASP A07

### Recommandations Prioritaires
1. Ajouter une expiration aux tokens JWT
2. Implémenter le refresh token rotation
3. Ajouter un rate limiter sur /login
```
