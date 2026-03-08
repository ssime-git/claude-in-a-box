# claude-in-a-box

Un environnement isolé pour exécuter Claude Code via OpenRouter dans une VM Incus.

En 2026, laisser un agent IA exécuter du code directement sur sa machine principale est un risque réel.
Ce repo propose une approche simple : utiliser une VM isolée où Claude peut exécuter des commandes sans impacter le système hôte.


```txt
votre machine hôte
└── VM Incus
    ├── Claude Code
    ├── → OpenRouter
    └── workspace
```

⸻
# Pré-requis

Incus doit être installé sur la machine hôte. Pour plus de détails, consuter la [Documentation officielle](https://linuxcontainers.org/incus/docs/main/installing/)

Vous avez aussi besoin d’une clé API OpenRouter (`sk-or-v1-...``), disponible gratuitement [ici](https://openrouter.ai).

Une procédure d’installation complète pour Ubuntu 22.04 sera ajoutée plus tard.

⸻

# Démarrer la VM

1. Lister les instances : `incus list`
2. Démarrer la VM : `incus start ai-cli-vm`
3. Entrer dans la VM : `incus exec ai-cli-vm -- bash`


⸻

# Lancer Claude Code

Dans la VM : `source .env; claude`
⸻

# Copier des fichiers

Depuis l’hôte vers la VM : `incus file push file.py ai-cli-vm/root/workspace/`

Depuis la VM vers l’hôte : `incus file pull ai-cli-vm/root/workspace/output.py .`


⸻

# Pourquoi une VM

Un agent IA peut exécuter des commandes arbitraires.

L’exécuter dans une VM permet de garder une frontière claire entre l’environnement expérimental et la machine principale.

```txt
host system
└── virtual machine
    └── agent
```

Si la VM casse, l’hôte reste intact.

