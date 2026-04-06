# CONTEXTE PROJET — Fan-Traduction FR Baten Kaitos II HD Remaster

## Résumé rapide

Patch de traduction française pour BK2 dans le HD Remaster Steam. 20 178 textes traduits sur 20 316 (138 entrées BK2_ADD_* = debug interne, non traduites). Le patch est injecté dans le TextData_en du bundle Unity via UnityPy. Les CRC du catalog Addressables sont mis à zéro en pur Python.

## Architecture des fichiers du jeu

```
<STEAM>/steamapps/common/BatenKaitos HD Remaster/
└── Batenkaitos2/Batenkaitos_Data/StreamingAssets/aa/
    ├── catalog.json                                    ← CRC à patcher
    └── StandaloneWindows64/
        └── a8cc32bba98258300a54327273719c75.bundle      ← TextData_en modifié
```

Le bundle contient 16 TextAssets (un par langue + TextIDList). Le jeu charge TOUJOURS TextData_en pour BK2.

## Format des données

**TextData_en** : JSON `{"array":[{"i":[index,...],"t":"texte"},...]}`
**TextIDList** : JSON `{"textId":["HPI_000","HPI_001",...,"BK2_ADD_163"]}`
**Traductions** : JSON `{"HPI_000":"Texte FR","RASALAS_042":"Autre texte",...}`

L'injection mappe TextID → index via TextIDList, puis remplace le texte dans TextData_en à la position correspondante.

## Format du catalog.json (CRC patching)

- m_EntryDataString : base64 → binaire. Commence par 4 octets (int32 LE = nombre d'entrées), puis N × 28 octets (7 × int32). Le champ [4] (5ème int32) = dataIndex dans ExtraData.
- m_ExtraDataString : base64 → binaire. Chaque objet à un offset donné :
  - 1 octet : ObjectType (7 = JsonObject)
  - 7-bit length + UTF-8 : nom de l'assembly
  - 7-bit length + UTF-8 : nom de la classe
  - int32 LE : longueur en octets du JSON UTF-16LE
  - JSON en UTF-16LE contenant m_Crc et m_Hash
- Le patch met m_Crc=0 et m_Hash="" puis padde avec des espaces UTF-16LE pour conserver la même taille.

## Bugs résolus (à ne pas réintroduire)

1. **BOM UTF-8** : UnityPy retourne m_Script comme string avec BOM \ufeff. Il faut le strip : `if raw.startswith('\ufeff'): raw = raw[1:]`
2. **Sauvegarde string/bytes** : UnityPy retourne m_Script comme string. Il faut le restocker comme string, pas comme bytes.encode(). Sinon save() fait un double encode → crash.
3. **CRC pur Python v1** : échouait car le format du catalog utilise des entiers 7-bit (.NET), du JSON en UTF-16LE, et un header de 4 octets dans EntryData. La v2 gère tout ça.
4. **set -e dans bash** : provoque des sorties silencieuses. Retiré.
5. **Venv sans tkinter** : si tkinter est installé après la création du venv, le venv doit être recréé.
6. **Couleurs hex tkinter** : doivent être exactement 3 ou 6 caractères (#fff ou #ffffff). 5 caractères → crash.
7. **Encodage Windows** : Python utilise cp1252 par défaut. Il faut PYTHONUTF8=1 et PYTHONIOENCODING=utf-8 quand on lance Python depuis PowerShell.
8. **.bat Unicode** : les fichiers .bat Windows doivent être 100% ASCII. Pas d'accents, pas de tirets longs.

## Installateurs

### Windows
- installer_fantrad_bk2.bat : lance gui_installer.ps1
- gui_installer.ps1 : interface PowerShell Windows Forms (thème sombre, boutons, progression, logs)
- install_fantrad_bk2.py : moteur d'injection + 20 178 traductions en gzip+base64
- python/ : Python 3.12 embeddable + pip + UnityPy pré-installés
- menu_traduction.png : image popup fin d'installation
- Inno Setup (.iss) : compile le tout en .exe unique

### Linux / Steam Deck
- installer_fantrad_bk2.sh : wrapper bash (gère venv, tkinter, UnityPy, fallback CLI)
- gui_installer.py : interface tkinter (même fonctionnalités que Windows)
- install_fantrad_bk2.py : même fichier que Windows
- menu_traduction.png : même image

### Chemins clés
- Windows Inno install dir : {localappdata}\BK2_FanTrad_FR
- Windows logs : %TEMP%\BK2_FanTrad_FR\
- Linux venv : .fantrad_venv (dans le dossier du script)
- Linux logs : /tmp/BK2_FanTrad_FR/
- Backup fichiers jeu : .fantrad_backup (à côté des originaux)

## Détection Steam

### Windows
1. Registre : HKCU\Software\Valve\Steam\SteamPath
2. Registre : HKLM\SOFTWARE\WOW6432Node\Valve\Steam\InstallPath
3. Drives C-H : Program Files (x86)/Steam, SteamLibrary, Steam
4. libraryfolders.vdf

### Linux
1. ~/.local/share/Steam/
2. ~/.steam/steam/
3. ~/.var/app/com.valvesoftware.Steam/ (Flatpak)
4. /run/media/*/steamapps/ (cartes SD)
5. libraryfolders.vdf

## Conventions de traduction

- Magnus = Magnus (jamais traduit)
- HP → PV, MP → PM, TP → PT, G → PO
- Vouvoiement par défaut, tutoiement combats/intimité
- Balises préservées : <w> <p> <v> <n0> <s0>...<s> <aN> <q> <cN>...<c> <BN>...<B> %s %d \n <eN> <rN> <tN> <yN>

## Sessions de travail

11 sessions (mars-avril 2026) couvrant :
- Session 1 : setup, extraction, glossaire BK1, pipeline UnityPy
- Sessions 2-9 : traduction par batches (menus, Magnus, dialogues, zones)
- Session 10 : finalisation 100%, installateur initial, bugs corrigés
- Session 11 : installateur GUI Windows/Linux, CRC pur Python, Inno Setup, packaging

## Fichiers de traduction source

133 fichiers batch (batch1 à batch48a) fusionnés dans BK2_all_translations_merged.json (20 178 entrées). 23 doublons résolus (fichier le plus récent gagne).

## Licence

CC BY-NC-ND 4.0 — Jérémie Corbion <corbion.jeremie@gmail.com>
Projet non officiel. Baten Kaitos = Bandai Namco / Monolith Soft.

## Pour reprendre le travail

Envoyer à Claude :
1. Ce fichier CONTEXTE_PROJET.md
2. Le fichier install_fantrad_bk2.py (contient les traductions embarquées)
3. Décrire ce qu'il faut faire

Le script Python est autonome — il contient les 20 178 traductions en gzip+base64, la logique d'injection, le CRC patcher, et la détection Steam. Tout le reste (GUI, wrappers) est du packaging autour.
