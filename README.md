# MCP Meta Ads — Claude Code Skill

Skill Claude Code pour créer et gérer des campagnes publicitaires Meta Ads (Facebook & Instagram) via l'API Meta Graph, sans passer par le navigateur.

---

## Prérequis

- [Claude Code](https://claude.ai/code) installé
- Une app Meta Ads créée sur [developers.facebook.com](https://developers.facebook.com)
- Un compte publicitaire Meta (Ad Account)
- Node.js 18+ installé

---

## Installation

### 1. Copier le skill

Copie le fichier `skills/meta-campaign.md` dans ton dossier de commandes Claude Code :

**Windows :**
```
C:\Users\<ton-username>\.claude\commands\meta-campaign.md
```

**Mac / Linux :**
```
~/.claude/commands/meta-campaign.md
```

> Ensuite ouvre le fichier et remplace `~` par ton chemin réel si tu es sur Windows.

---

### 2. Configurer le MCP Meta

Ajoute la configuration suivante dans ton fichier Claude Desktop :

**Windows** : `C:\Users\<username>\AppData\Roaming\Claude\claude_desktop_config.json`
**Mac** : `~/Library/Application Support/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "meta-mcp": {
      "command": "node",
      "args": ["<chemin-vers>/meta-mcp/build/index.js"],
      "env": {
        "META_APP_ID": "TON_APP_ID",
        "META_APP_SECRET": "TON_APP_SECRET",
        "META_ACCESS_TOKEN": "TON_ACCESS_TOKEN"
      }
    }
  }
}
```

> ⚠️ Ne jamais committer ce fichier — il contient des credentials.

---

### 3. Créer le fichier clients

Crée le dossier et copie le template :

```bash
mkdir ~/.claude/meta-clients
cp clients-template.md ~/.claude/meta-clients/clients.md
```

---

## Utilisation

Dans Claude Code, tape simplement :

```
/meta-campaign
```

Le skill va :
1. Charger l'historique de tes clients
2. Te poser les questions nécessaires (client, vidéo, objectif, budget, audience)
3. Afficher un récapitulatif complet
4. Attendre ta confirmation **OUI** avant de lancer quoi que ce soit
5. Créer la campagne via l'API Meta Graph
6. Mettre à jour le profil client automatiquement

---

## Objectifs supportés

| Objectif | Type API | Description |
|----------|----------|-------------|
| **VUES** | `OUTCOME_AWARENESS` | Maximiser les vues vidéo (2 secondes) |
| **MESSAGES** | `OUTCOME_TRAFFIC` | Trafic vers WhatsApp via lien `wa.me` |

---

## Ciblage géographique

Le skill utilise les **region keys** de l'API Meta pour cibler des régions précises.

Exemples Maroc :
| Région | Key |
|--------|-----|
| Tanger-Tétouan | `2226` |
| Grand Casablanca | `2221` |
| Rabat-Salé | `2225` |

Pour trouver d'autres keys :
```
GET https://graph.facebook.com/v22.0/search?type=adgeolocation&q=<ville>&access_token=<token>
```

---

## Structure des fichiers

```
mcp-meta/
├── README.md                  ← Ce fichier
├── clients-template.md        ← Template vide pour ~/.claude/meta-clients/clients.md
└── skills/
    └── meta-campaign.md       ← Skill à copier dans ~/.claude/commands/
```

---

## Notes importantes

- Le fichier `clients.md` contient des données clients confidentielles → **ne jamais le committer**
- L'access token Meta expire — le renouveler régulièrement dans `claude_desktop_config.json`
- Pour les campagnes MESSAGES : le page linked doit avoir un **WhatsApp Business** pour utiliser la destination WhatsApp native. Sinon, utiliser un lien `wa.me` avec l'objectif TRAFIC.

---

## Workflow de création d'une campagne

```
/meta-campaign
    ↓
Questions client + vidéo + config
    ↓
Récapitulatif complet
    ↓
Tape OUI
    ↓
Campaign → AdSet → Creative → Ad
    ↓
Rapport + mise à jour clients.md
```
