# MCP Meta Ads — Claude Code Skill

Skill Claude Code pour créer, analyser et gérer des campagnes publicitaires Meta Ads (Facebook & Instagram) via l'API Meta Graph, sans passer par le navigateur.

---

## Prérequis

- [Claude Code](https://claude.ai/code) installé
- Une app Meta Ads créée sur [developers.facebook.com](https://developers.facebook.com) (mode développement recommandé pour usage interne)
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
        "META_APP_SECRET": "TON_APP_SECRET"
      }
    }
  }
}
```

> ⚠️ Ne jamais committer ce fichier — il contient des credentials.

---

### 3. Authentification

Le skill supporte deux modes d'authentification :

**Token utilisateur (OAuth — 60 jours) :**
```
/meta-campaign → meta_login → copier l'URL → authentifier → meta_whoami
```

**System User Token (permanent — recommandé pour équipes) :**
1. Créer un Utilisateur Système dans Meta Business Suite
2. Affecter le compte publicitaire avec Contrôle Total
3. Générer un token sans expiration
4. `meta_set_token` avec le token généré

---

### 4. Créer le fichier clients

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
1. **Demander quel compte Facebook** utiliser (évite les confusions entre comptes)
2. **Charger l'historique** du client depuis `clients.md`
3. **Poser toutes les questions** (type de campagne, contenu, budget, audience)
4. **Afficher un récapitulatif complet**
5. **Attendre ta confirmation "OUI"** avant de lancer
6. **Créer la campagne** via l'API Meta Graph
7. **Mettre à jour** le profil client automatiquement

---

## Objectifs supportés

| Objectif | Type API | Description |
|----------|----------|-------------|
| **MESSAGES** | `OUTCOME_ENGAGEMENT` / `OUTCOME_TRAFFIC` | Conversations WhatsApp / Messenger / Instagram Direct |
| **VUES** | `OUTCOME_AWARENESS` | Vues vidéo ThruPlay ou 2 secondes |
| **VENTES** | `OUTCOME_SALES` | Conversions site web via Pixel Meta |

---

## Philosophie Andromeda (Meta 2025–2026)

Le skill applique les nouvelles meilleures pratiques Meta :

| Aspect | Recommandation |
|--------|----------------|
| Structure | 1–2 campagnes max, structure simplifiée |
| Audience | Broad targeting ou Advantage+ — l'IA cible automatiquement |
| Budget | CBO (Campaign Budget Optimization) par défaut |
| Créatifs | **3 à 6 ads par adset** — rotation automatique par l'IA |
| Placements | Advantage+ Placements (Reels, Stories, Feed, Messenger) |
| Rôle advertiser | Stratégique : qualité des créatifs et du funnel |

---

## KPIs cibles (Maroc)

### Campagnes MESSAGES
| KPI | Objectif |
|-----|----------|
| Coût par message | 0.30 $ – 1.50 $ |
| CTR | > 2% |
| Fréquence | 2–3 max |

### Campagnes VENTES
| KPI | Objectif |
|-----|----------|
| CPA (coût/achat) | 2 – 4 $ |
| ROAS | > 2x |
| CTR | > 2% |
| CPM | 1 – 3 $ |

### Campagnes VUES
| KPI | Objectif |
|-----|----------|
| CPM | < 1 $ |
| ThruPlay rate | > 15% |

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

## Gestion multi-comptes

Le skill demande systématiquement **quel compte Facebook utiliser** avant chaque campagne, ce qui évite les erreurs sur des comptes clients différents. Il appelle `meta_get_ad_accounts` pour lister les comptes disponibles si nécessaire.

---

## Notes techniques

- **App en mode dev** : seuls les admins/testeurs déclarés peuvent s'authentifier. Parfait pour un usage interne d'équipe.
- **App en mode live** : nécessite App Review (Tech Provider) pour accès avancé. Non recommandé pour usage interne.
- **WhatsApp compte personnel** : utiliser `OUTCOME_TRAFFIC` + lien `wa.me/212XXXXXXXXX` (pas d'objectif MESSAGES natif possible).
- **Token System User** : permanent, pas de renouvellement, idéal pour équipes community management.
- **Token OAuth utilisateur** : valide 60 jours, renouveler via `meta_login`.

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

## Workflow de création d'une campagne

```
/meta-campaign
    ↓
Sélection compte Facebook
    ↓
Questions client + contenu + objectif + budget + audience
    ↓
Récapitulatif complet
    ↓
Tape OUI
    ↓
Campaign → AdSet → 3–6 Ads créatifs
    ↓
Rapport + mise à jour clients.md
```
