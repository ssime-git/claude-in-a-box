# Claude in a Box

Environnement isolé pour exécuter Claude Code via OpenRouter dans une machine virtuelle Incus.

## Motivation

En 2026, laisser un agent IA exécuter du code directement sur votre machine principale pose un risque réel de sécurité. Ce projet propose une approche simple mais efficace : utiliser une VM isolée où Claude peut exécuter des commandes sans impacter le système hôte.

```
Machine hôte
└── VM Incus (isolée)
    ├── Claude Code
    ├── OpenRouter API
    └── Workspace
```

Si la VM casse ou est compromise, le système hôte reste intact.


## Prérequis

- **Incus** installé sur la machine hôte
  - [Documentation officielle d'installation](https://linuxcontainers.org/incus/docs/main/installing/)
- **Clé API OpenRouter** (`sk-or-v1-...`)
  - Disponible gratuitement sur [openrouter.ai](https://openrouter.ai)

> **Note :** Une procédure d'installation détaillée pour Ubuntu 22.04 sera ajoutée ultérieurement.


## Démarrage rapide

### 1. Lancer la VM

```bash
# Lister les instances disponibles
incus list

# Démarrer la VM
incus start ai-cli-vm

# Accéder à la VM
incus exec ai-cli-vm -- bash
```

### 2. Configurer l'environnement

Dans la VM, charger les variables d'environnement et lancer Claude :

```bash
source .env
claude
```

### 3. Transférer des fichiers

**Depuis la machine hôte vers la VM :**
```bash
incus file push mon-script.py ai-cli-vm/root/workspace/
```

**Depuis la VM vers la machine hôte :**
```bash
incus file pull ai-cli-vm/root/workspace/resultat.py .
```


## Architecture

La séparation VM/hôte offre plusieurs avantages :

| Aspect | Bénéfice |
|--------|----------|
| **Sécurité** | Les commandes exécutées par l'agent restent isolées |
| **Stabilité** | Les plantages ou erreurs dans la VM n'affectent pas l'hôte |
| **Nettogage** | Suppression facile de la VM si quelque chose se brise |
| **Portabilité** | La VM peut être clonée ou sauvegardée |


## Troubleshooting

### La VM n'est pas accessible
```bash
incus list  # Vérifier l'état
incus info ai-cli-vm  # Détails de l'instance
```

### Problème de connexion OpenRouter
- Vérifier que la clé API est valide et présente dans `.env`
- Vérifier la connectivité réseau depuis la VM : `ping openrouter.ai`

### Erreurs d'exécution Claude
- Vérifier les logs : `incus logs ai-cli-vm`
- S'assurer que toutes les dépendances sont installées dans la VM


## Développement futur

- [ ] Script d'installation automatique pour Ubuntu 22.04
- [ ] Configuration Terraform/Incus pour déploiement rapide
- [ ] Documentation des modèles disponibles via OpenRouter
- [ ] Gestion améliorée des fichiers (sync bidirectionnel)

