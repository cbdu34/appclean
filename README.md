# 🧹 AppClean

[![Python](https://img.shields.io/badge/Python-3.9%2B-blue.svg)](https://www.python.org/)
[![Tkinter](https://img.shields.io/badge/GUI-Tkinter-green.svg)](https://docs.python.org/3/library/tkinter.html)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Release](https://img.shields.io/badge/Télécharger-latest%20release-blue)](https://github.com/cbdu34/appclean/releases/latest)

> Petit utilitaire de bureau qui range automatiquement les fichiers d'un ou plusieurs dossiers dans des sous-dossiers par catégorie (Images, Vidéos, Documents, Archives…).

Interface graphique Tkinter, aucune dépendance externe, fonctionne sous Windows, Linux et macOS.

---

## ✨ Fonctionnalités

- 📂 **Rangement récursif** : analyse les sous-dossiers et trie les fichiers par extension.
- 🗂️ **8 catégories** : Images, Scripts Python, Documents LibreOffice, PDF/Textes, Vidéos, Musique, Archives et Autres.
- 🧹 **Nettoyage** : supprime les dossiers devenus vides après le tri.
- 🛡️ **Garde-fous** : ignore les dossiers système (`Windows`, `Program Files`, `AppData`, `.git`, `OneDrive`) et le fichier du programme lui-même.
- 🔁 **Doublons** : renomme automatiquement en `nouveau_<fichier>` si un fichier du même nom existe déjà.
- 🖥️ **Multi-dossiers** : ajoutez plusieurs dossiers à traiter en une seule fois.

---

## 🚀 Utilisation

### Option 1 : depuis le code source

```bash
git clone https://github.com/cbdu34/appclean.git
cd appclean
python appclean.py
```

Prérequis : Python 3.9+ avec Tkinter (inclus par défaut sous Windows/macOS ; sous Linux : `sudo apt install python3-tk`).

### Option 2 : exécutable Windows

Téléchargez `clean.exe` depuis la page [Releases](https://github.com/cbdu34/appclean/releases/latest).

---

## 🛠️ Reconstruction de l'exécutable

```bash
pip install pyinstaller
pyinstaller --onefile --windowed appclean.py
```

Le binaire est généré dans `dist/`.

---

## 📁 Catégories de rangement

| Dossier | Extensions |
| --- | --- |
| `Images` | jpg, jpeg, png, gif, bmp, svg, webp |
| `Scripts_Python` | py, pyw, ipynb |
| `LibreOffice_Docs` | odt, ott, sxw, ods, odp |
| `PDF_et_Textes` | pdf, txt, doc, docx |
| `Videos` | mp4, mov, avi, mkv |
| `Musique` | mp3, wav, flac |
| `Archives` | zip, rar, 7z |
| `Autres_Fichiers` | tout le reste |

> ⚠️ L'outil **déplace** les fichiers, il ne les supprime pas. Faites tout de même une sauvegarde avant le premier passage sur un dossier important.

---

## 📄 Licence

Distribué sous licence MIT. Voir le fichier [LICENSE](LICENSE).
