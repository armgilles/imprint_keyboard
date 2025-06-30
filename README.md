# Configuration ZMK Imprint - Migration depuis Glove80

Ce repository contient une configuration ZMK complète pour le clavier Imprint, adaptée depuis une configuration personnalisée Glove80. Cette migration préserve le workflow et les habitudes de frappe tout en optimisant pour le layout Imprint.

## Fonctionnalités

### Layers
- **Layer 0**: QWERTY base avec mod-tap (home row mods)
- **Layer 1**: Navigation/curseur (flèches, page up/down, home/end)
- **Layer 2**: Pavé numérique et fonctions numériques
- **Layer 3**: Symboles et ponctuation avancée
- **Layer 4**: System (Bluetooth, RGB, Windows toggle)
- **Layer 5**: World (caractères français, accents)
- **Layer 6**: Mouse (trackball, clics, scroll)
- **Layer 7**: Windows (adaptation raccourcis Windows)

### Bluetooth et connectivité
- Support 5 profils Bluetooth (slots 0-4)
- Connexion USB forcée
- Clear all connections (BT_CLR_ALL)
- Feedback RGB pour état des connexions

### RGB et feedback visuel
- Contrôles RGB complets (couleur, luminosité, effets)
- Indicateurs visuels pour connexions Bluetooth
- Feedback RGB pour mode Windows (violet)
- RGB toggle pour confirmation d'actions

### Ergonomie multi-OS
- Configuration macOS par défaut
- Layer Windows dédié avec raccourcis adaptés
- Switch Windows en position V (layer System)
- Gestion caractères français avec accents

### Trackball et souris
- Trackball droit intégré
- Layer mouse avec précision variable
- Support scroll et clics multiples
- Gestion native ZMK

### Behaviors avancés
- Hold-tap pour home row mods
- Sticky keys pour modificateurs
- Layer-tap pour accès rapide
- Combo keys pour fonctions spéciales
- Macros personnalisées avec feedback

## Structure des fichiers
- `config/imprint.keymap` - Configuration principale
- `config/glove80_config.keymap` - Référence Glove80 originale
- `config/default keymaps/` - Layouts alternatifs

## Notes importantes
- Windows toggle: position V dans layer System
- Bluetooth profiles: feedback RGB vert (connecté) / rouge (erreur)
- Mode Windows: feedback RGB violet permanent
- Trackball: configuré main droite par défaut

