# Configuration ZMK Imprint - Migration depuis Glove80

Ce repository contient une configuration ZMK complète pour le clavier Imprint, adaptée depuis une configuration personnalisée Glove80. Cette migration préserve le workflow et les habitudes de frappe tout en optimisant pour le layout Imprint.

## Objectifs de la Migration

- **Préservation du workflow** : Conservation du layout QWERTY modifié, home row mods, layers et thumb cluster
- **Compatibilité multi-OS** : Support optimisé pour macOS et Windows
- **Caractères spéciaux français** : Accès efficace aux accents et caractères spéciaux
- **Facilitation de la transition** : Documentation complète des différences entre Glove80 et Imprint

## Structure du Repository

```
config/
├── imprint.keymap              # Configuration principale
├── imprint_simplified.keymap   # Version simplifiée pour tests
├── glove_config.keymap        # Configuration originale Glove80 (référence)
├── imprint.conf               # Configuration du build
└── default keymaps/           # Keymaps par défaut pour différentes variantes
```

## Fonctionnalités Principales

### Layout QWERTY Personnalisé
- **é en position 3** : Accès direct au é français
- **Apostrophe personnalisée** : Solutions pour US International
- **Inversion P/;** : P à la place du point-virgule (comme Glove80)
- **Home row mods** : Ordre macOS (Ctrl/Alt/GUI/Shift)

### Thumb Cluster (6 touches par main)
Adaptation du système Glove80 6x6 pour l'Imprint :

**Main gauche :**
- `ESC` / `BACKSPACE` / `DELETE`
- `Hyper Key` / `Caps Word` / `Layer 1`

**Main droite :**
- `INSERT` / `ENTER` / `SPACE`
- `Caps Word` / `TAB` / `Layer 1`

### Layers (7 couches)
1. **QWERTY** : Layout principal avec modifications
2. **Cursor** : Navigation et raccourcis système
3. **Number** : Pavé numérique et symboles
4. **Symbol** : Symboles et caractères spéciaux
5. **System** : Bluetooth, reset, et contrôles système
6. **World** : Caractères français et internationaux
7. **Function** : Touches fonction F1-F12
8. **Windows** : Mode adaptatif pour Windows US International

## 🔧 Macros et Behaviors Personnalisés

### 1. Hyper Key
**Problématique :** ZMK ne supporte pas les modificateurs imbriqués comme `&kp LS(LC(LA(LGUI)))`

**Solution :** Macro `hyper_key`
```c
hyper_key: hyper_key {
    compatible = "zmk,behavior-macro";
    #binding-cells = <0>;
    bindings = <&macro_press &kp LSHFT &kp LCTRL &kp LALT &kp LGUI>, 
               <&macro_pause_for_release>,
               <&macro_release &kp LSHFT &kp LCTRL &kp LALT &kp LGUI>;
};
```

### 2. Caractères Français Multi-OS

**Problématique :** Différences entre macOS (Option+touches) et Windows US International (dead keys, AltGr)

**Solution :** Behaviors mod-morph avec macros Windows
```c
// macOS : Option+E pour é, Windows : ' puis e
world_e_acute: world_e_acute {
    bindings = <&kp RA(E)>, <&kp RA(LS(E))>;  // é -> É
};

world_e_acute_windows: world_e_acute_windows {
    bindings = <&kp SQT &kp E>; // Windows US Intl: ' puis e pour é
};
```

**Caractères supportés :**
- `à/á` (grave/aigu)
- `è/é` (grave/aigu) 
- `ù/û` (grave/circonflexe)
- `î/ô` (circonflexe)
- `ç/Ç` (cédille)
- `ý/ÿ` (aigu/tréma)

### 3. Apostrophe Intelligente (US International)

**Problématique :** En US International, `'` devient une dead key qui attend une voyelle, empêchant l'usage normal dans les contractions (`don't` → `dón't`)

**Solutions disponibles :**

#### Smart Apostrophe (macro)
```c
smart_apostrophe: smart_apostrophe {
    bindings = <&kp SQT &kp SPACE &kp BSPC>; // ' + espace + backspace
};
```
- Séquence : apostrophe + espace + backspace
- Résultat : apostrophe normale sans attendre de voyelle
- Usage : contractions fonctionnent normalement

#### Context Apostrophe (mod-morph)
```c
context_apostrophe: context_apostrophe {
    bindings = <&smart_apostrophe>, <&kp SQT>;  // Smart par défaut, normale avec Shift
    mods = <(MOD_LSFT|MOD_RSFT)>;
};
```
- Normal : apostrophe intelligente
- Shift + apostrophe : apostrophe normale pour dead key

### 4. Mode Windows Adaptatif

**Problématique :** Adaptation dynamique des macros selon l'OS

**Solution :** Layer toggle (Layer 7) + behaviors adaptatifs
```c
os_toggle: os_toggle {
    compatible = "zmk,behavior-tri-state";
    bindings = <&tog 7>, <&none>, <&tog 7>;
};
```

**Accès :** Layer System → touche `V` ou `Caps` pour activer/désactiver le mode Windows

### 5. Layer System Complet

**Fonctionnalités :**
- **Bluetooth** : Sélection profils (0-4), clear
- **Reset** : `sys_reset`, `bootloader` 
- **Factory Test** : Test des touches
- **Toggle Windows** : Activation mode US International
- **Sticky Keys** : Modificateurs temporaires

## Guide d'Utilisation

### Caractères Français

#### Sur macOS
```
é : Layer World + E
à : Layer World + A  
ç : Layer World + C
etc.
```

#### Sur Windows (US International)
1. Activer le mode Windows : `Layer System` → `V`
2. Utiliser les mêmes touches, les macros s'adaptent automatiquement

### Apostrophes en US International

**Pour les mots :** Utiliser l'apostrophe normale (macro smart intégrée)
```
don't → fonctionne normalement
l'école → fonctionne normalement
```

**Pour les accents (rare) :** 
- Layer Symbol → touche `U` pour apostrophe dead key
- Ou Shift + apostrophe (si context_apostrophe est activé)

### Thumb Cluster

**Navigation rapide :**
- Maintenir pouce gauche : Layer World (caractères)
- Maintenir pouce droit : Layer Number, Symbol, System
- Tap : ESC, Backspace, Delete, Insert, Enter, Space

## Migration depuis Glove80

### Différences Principales

1. **Thumb cluster** : 6 touches au lieu de 6 (adaptation du layout)
2. **Touches fonction** : Row dédiée sur Imprint vs layer sur Glove80
3. **Layout physique** : Adaptation des positions pour l'Imprint

### Transition

1. **Muscle memory** : Le thumb cluster principal reste identique
2. **Layers** : Même logique et organisation
3. **Caractères spéciaux** : Même accès via Layer World
4. **Home row mods** : Positions conservées

## Compilation et Installation

### Prérequis
- ZMK firmware compatible
- Clavier Imprint avec Function Row Full Bottom Row

### Build
```bash
# Cloner le repository
git clone [repository-url]
cd imprint_keyboard

# Build via ZMK
# Le fichier build.yaml contient la configuration
```

### Configuration Système

#### macOS
- Layout : US ou ABC
- Les macros Option+touches fonctionnent directement

#### Windows  
- Layout : **US International** obligatoire
- Activer le mode Windows dans la configuration
- Les dead keys et AltGr fonctionnent automatiquement

## Historique des Modifications

### Version Actuelle
- ✅ Migration complète des 7 layers depuis Glove80
- ✅ Adaptation du thumb cluster 6x6 pour Imprint
- ✅ Behaviors mod-morph pour caractères français
- ✅ Macros Windows US International
- ✅ Hyper key macro (contournement ZMK)
- ✅ Solutions apostrophe US International
- ✅ Layer System avec gestion Bluetooth
- ✅ Mode adaptatif macOS/Windows

### Problématiques Résolues
1. **Modificateurs imbriqués** → Macro hyper_key
2. **Caractères français multi-OS** → Behaviors adaptatifs + macros Windows
3. **Apostrophe dead key** → Smart apostrophe macro
4. **Caps Word ZMK** → Correction `&caps_word` 
5. **Compatibilité cross-platform** → Layer toggle + macros conditionnelles

## Contribution

Cette configuration est optimisée pour un workflow spécifique de migration Glove80 → Imprint. Les adaptations et améliorations sont les bienvenues via issues et pull requests.

## Documentation Complète

- **[README.md](./README.md)** : Vue d'ensemble et guide d'utilisation
- **[MACROS_TECHNIQUES.md](./MACROS_TECHNIQUES.md)** : Documentation technique détaillée des macros et behaviors
- **[PROBLEMATIQUES_RESOLUES.md](./PROBLEMATIQUES_RESOLUES.md)** : Résumé complet des problématiques et solutions
- **[Configuration originale Glove80](./config/glove_config.keymap)** : Référence pour comparaison

### Guides Spécialisés (à créer si nécessaire)
- Comparaison layouts (Glove80 vs Imprint)
- Guide caractères spéciaux français
- Troubleshooting compilation ZMK
- Tests et validation configuration

## Résumé Technique

Cette configuration résout **10 problématiques majeures** identifiées lors de la migration :

1. **Modificateurs imbriqués ZMK** → Macro hyper_key
2. **CAPS_WORD incorrect** → Correction behavior
3. **Caractères français multi-OS** → Behaviors adaptatifs
4. **Apostrophe dead key US International** → Smart apostrophe  
5. **Adaptation thumb cluster** → Mapping intelligent
6. **Accès touches fonction** → Optimisation layout
7. **Support français incomplet** → Système complet
8. **Incohérence cross-platform** → Mode adaptatif
9. **Transition workflow** → Migration transparente
10. **Efficacité caractères spéciaux** → Multi-accès optimisé

**Résultat :** Configuration robuste, compatible multi-OS, préservant le workflow Glove80 tout en optimisant pour l'Imprint.

## Ressources

- [Documentation ZMK](https://zmk.dev/)
- [Configuration Imprint](https://docs.cyboard.digital/)
- [Comparaison layouts](./LAYOUT_COMPARISON.md) (si disponible)
- [Caractères spéciaux](./CARACTERES_SPECIAUX.md) (si disponible)