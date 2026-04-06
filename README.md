# Baten Kaitos II (Origins) HD Remaster — Fan-Traduction Française

Patch de traduction française non officiel pour **Baten Kaitos Origins** dans le HD Remaster (Steam).

**20 178 textes traduits** : dialogues, descriptions de Magnus, menus en jeu, bestiaire, quêtes, tutoriels, et plus encore.

---

## Aperçu

| Avant (anglais) | Après (français) |
|:---:|:---:|
| *A long, long time ago, in the age of divinity...* | *Il y a très longtemps, à l'ère de la divinité...* |

Ce patch injecte la traduction française directement dans les fichiers du jeu. Aucune manipulation complexe n'est requise : lancez l'installateur, et c'est fait.

### Ce qui est traduit

- Tous les dialogues (événements, zones, PNJ)
- Les descriptions de Magnus (cartes, équipements, objets)
- Le bestiaire complet
- Les quêtes et sous-quêtes
- Les menus en jeu (combat, inventaire, statuts)
- Les tutoriels et textes système
- Le résumé de l'histoire et les lettres
- Les crédits

### Ce qui reste en anglais

Le **menu de démarrage** du jeu (New Game, Load Game, Options, etc.) utilise des textes intégrés directement dans l'interface Unity et ne fait pas partie du système de localisation. Ces textes ne peuvent pas être modifiés par ce patch.

Une image de correspondance des menus est affichée à la fin de l'installation :

| Anglais | Français |
|---|---|
| New Game | Nouvelle Partie |
| Load Game | Charger une Partie |
| License | Licence |
| Options | Options |
| Privacy Policy | Politique de Confidentialité |
| Quit Game | Quitter le Jeu |

---

## Prérequis

- **Baten Kaitos I & II HD Remaster** acheté et installé via Steam
- Environ **100 Mo** d'espace disque temporaire pendant l'installation
- **Windows** : aucun prérequis, tout est embarqué dans l'installateur
- **Linux / Steam Deck** : Python 3.8+ (préinstallé sur la plupart des distributions)

---

## Installation

### Windows

**Option 1 — Installateur .exe (recommandé)**

1. Téléchargez `BK2_FanTrad_FR_v1.0b_Setup.exe` depuis les [Releases](../../releases)
2. Lancez l'exécutable
3. Acceptez la licence
4. Cliquez sur "Installer"
5. L'interface graphique s'ouvre automatiquement
6. Cliquez sur **"Installer la traduction"**
7. C'est terminé — lancez le jeu via Steam

**Option 2 — Archive ZIP**

1. Téléchargez `BK2_FanTrad_FR_Windows.zip` depuis les [Releases](../../releases)
2. Dézippez le dossier `BK2_FanTrad_FR`
3. Double-cliquez sur `installer_fantrad_bk2.bat`
4. L'interface graphique s'ouvre
5. Cliquez sur **"Installer la traduction"**

> **Note antivirus** : certains antivirus peuvent signaler l'installateur comme suspect (faux positif courant avec les exécutables créés par Inno Setup). Vous pouvez vérifier le fichier sur [VirusTotal](https://www.virustotal.com/) si vous avez un doute. Le code source complet est disponible dans ce dépôt.

### Linux / Steam Deck

1. Téléchargez `BK2_FanTrad_FR_Linux.zip` depuis les [Releases](../../releases)
2. Dézippez et lancez :

```bash
unzip BK2_FanTrad_FR_Linux.zip
cd BK2_FanTrad_FR
./installer_fantrad_bk2.sh
```

3. Le script installe automatiquement les dépendances (virtualenv, UnityPy) et ouvre l'interface graphique
4. Cliquez sur **"Installer la traduction"**

> **Steam Deck** : passez en mode Bureau (Bouton Steam → Alimentation → Basculer vers le bureau), ouvrez Konsole, et suivez les mêmes étapes.

> **Mode terminal** : si l'interface graphique ne fonctionne pas, utilisez : `./installer_fantrad_bk2.sh --cli`

---

## Restaurer l'original

Pour revenir à la version anglaise du jeu :

**Windows** : relancez l'installateur et cliquez sur **"Restaurer l'original"**

**Linux** :
```bash
./installer_fantrad_bk2.sh --restore
```

Ou en mode terminal :
```bash
python3 install_fantrad_bk2.py --restore
```

Les fichiers originaux sont automatiquement sauvegardés lors de la première installation (fichiers `.fantrad_backup` à côté des originaux).

---

## Mises à jour Steam

Si le jeu est mis à jour par Steam, **le patch sera probablement écrasé**. Dans ce cas, relancez simplement l'installateur pour réappliquer la traduction. Vos sauvegardes ne sont pas affectées.

---

## Fonctionnement technique

Ce patch ne modifie que deux fichiers du jeu :

| Fichier | Modification |
|---|---|
| `a8cc32bba98258300a54327273719c75.bundle` | Les textes anglais de `TextData_en` sont remplacés par les textes français |
| `catalog.json` | Les CRC (checksums) sont mis à zéro pour permettre le chargement du bundle modifié |

Le jeu charge toujours `TextData_en` pour BK2 quelle que soit la langue sélectionnée — c'est pourquoi la traduction est injectée dans cet asset.

Aucun asset du jeu original n'est inclus dans ce patch. Seuls les textes traduits et le code de l'installateur sont distribués.

---

## Contenu du package

### Windows (.exe)
```
BK2_FanTrad_FR_v1.0b_Setup.exe     Installateur Inno Setup tout-en-un
```

### Windows (.zip)
```
BK2_FanTrad_FR/
├── installer_fantrad_bk2.bat       Lanceur GUI
├── gui_installer.ps1               Interface graphique (PowerShell)
├── install_fantrad_bk2.py          Moteur d'injection + 20 178 traductions
├── menu_traduction.png             Image guide des menus
└── python/                         Python 3.12 embarqué + UnityPy
```

### Linux (.zip)
```
BK2_FanTrad_FR/
├── installer_fantrad_bk2.sh        Lanceur (GUI + gestion dépendances)
├── install_fantrad_bk2.py          Moteur d'injection + 20 178 traductions
├── gui_installer.py                Interface graphique (tkinter)
└── menu_traduction.png             Image guide des menus
```

---

## Conventions de traduction

| Terme anglais | Traduction française | Notes |
|---|---|---|
| Magnus | Magnus | Jamais traduit (terme propre au jeu) |
| HP | PV | Points de Vie |
| MP | PM | Points de Magie |
| TP | PT | Points Techniques |
| G (monnaie) | PO | Pièces d'Or |

- **Vouvoiement** par défaut dans les dialogues
- **Tutoiement** pour les scènes de combat et les relations proches
- Alignement sur la traduction officielle française de Baten Kaitos I (terminologie, noms propres, registre des personnages)

---

## Problèmes connus

| Problème | Solution |
|---|---|
| Menu de démarrage en anglais | Normal — ces textes sont dans l'interface Unity, pas dans les fichiers de localisation |
| Le jeu revient en anglais après une mise à jour Steam | Relancez l'installateur |
| L'antivirus bloque le .exe | Faux positif — vérifiez sur VirusTotal ou utilisez la version .zip |
| "Jeu non trouvé" sur Linux | Le script cherche dans les emplacements Steam standards. Utilisez le bouton "Choisir le dossier" ou `--path /chemin/vers/le/jeu` |

---

## Licence

**Traduction** : © Jérémie Corbion — [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0/)

- **BY** : crédit obligatoire à l'auteur
- **NC** : usage non commercial uniquement — revente interdite
- **ND** : redistribution de versions modifiées interdite

**Avertissement** : ce patch est un projet de fan-traduction non officiel. Il n'est ni approuvé ni soutenu par Bandai Namco Entertainment ou Monolith Soft. Baten Kaitos et tous les éléments associés sont la propriété de leurs détenteurs respectifs. Ce patch ne contient aucun asset du jeu original.

---

## Crédits

| | |
|---|---|
| **Traduction** | Jérémie Corbion |
| **Outils & installateur** | Développés avec l'assistance de Claude (Anthropic) |
| **Jeu original** | Monolith Soft / Bandai Namco Entertainment |
| **Traduction FR de BK1** | Bandai Namco (référence pour la cohérence terminologique) |

---

## Contact

- **Email** : corbion.jeremie@gmail.com
- **Bugs & suggestions** : [Issues GitHub](../../issues)
