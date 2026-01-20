import os
import shutil
import tkinter as tk
from tkinter import filedialog, messagebox
from pathlib import Path

# --- CONFIGURATION ---
CLASSEMENT = {
    "Images": [".jpg", ".jpeg", ".png", ".gif", ".bmp", ".svg", ".webp"],
    "Scripts_Python": [".py", ".pyw", ".ipynb"],
    "LibreOffice_Docs": [".odt", ".ott", ".sxw", ".ods", ".odp"],
    "PDF_et_Textes": [".pdf", ".txt", ".doc", ".docx"],
    "Videos": [".mp4", ".mov", ".avi", ".mkv"],
    "Musique": [".mp3", ".wav", ".flac"],
    "Archives": [".zip", ".rar", ".7z"]
}

# Fichiers et dossiers à NE JAMAIS toucher
FICHIERS_A_IGNORER = ["env.py", Path(__file__).name] 
DOSSIERS_INTERDITS = ["AppData", "Windows", "Program Files", "Program Files (x86)", ".git", "OneDrive"]

def nettoyer_dossiers_vides(chemin_parent):
    for root, dirs, files in os.walk(chemin_parent, topdown=False):
        for name in dirs:
            dossier_complet = os.path.join(root, name)
            if name not in CLASSEMENT.keys() and name != "Autres_Fichiers":
                try:
                    if not os.listdir(dossier_complet): 
                        os.rmdir(dossier_complet)
                except:
                    pass

def ranger_fichiers_recursif(dossier_parent):
    compteur = 0
    bloques = []
    p_parent = Path(dossier_parent)
    
    for fichier in list(p_parent.rglob("*")):
        try:
            # SÉCURITÉ 1 : Ignorer les dossiers système/cloud
            if any(interdit in fichier.parts for interdit in DOSSIERS_INTERDITS):
                continue
            
            # SÉCURITÉ 2 : Ignorer le fichier de code (env.py)
            if fichier.name in FICHIERS_A_IGNORER:
                continue

            if fichier.is_file():
                if any(cat in fichier.parts for cat in CLASSEMENT.keys()) or "Autres_Fichiers" in fichier.parts:
                    continue
                    
                extension = fichier.suffix.lower()
                nom_categorie = "Autres_Fichiers"
                
                for categorie, extensions in CLASSEMENT.items():
                    if extension in extensions:
                        nom_categorie = categorie
                        break
                
                dest_dir = fichier.parent / nom_categorie
                dest_dir.mkdir(exist_ok=True)
                
                cible = dest_dir / fichier.name
                if cible.exists():
                    cible = dest_dir / f"nouveau_{fichier.name}"
                
                shutil.move(str(fichier), str(cible))
                compteur += 1
        except PermissionError:
            bloques.append(fichier.name)
        except Exception:
            continue
                
    return compteur, bloques

# --- INTERFACE ---
def ajouter_dossier():
    dossier = filedialog.askdirectory()
    if dossier and dossier not in liste_widget.get(0, tk.END):
        liste_widget.insert(tk.END, dossier)

def supprimer_selection():
    for i in reversed(liste_widget.curselection()):
        liste_widget.delete(i)

def lancer_rangement():
    dossiers = liste_widget.get(0, tk.END)
    if not dossiers:
        messagebox.showwarning("Attention", "Veuillez ajouter un dossier.")
        return
    
    total_ok = 0
    tous_bloques = []
    
    for d in dossiers:
        ok, err = ranger_fichiers_recursif(d)
        total_ok += ok
        tous_bloques.extend(err)
        nettoyer_dossiers_vides(d)
    
    bilan = f"Opération terminée !\n\n✅ {total_ok} fichiers rangés.\n⚠️ {len(tous_bloques)} fichiers ignorés (occupés)."
    messagebox.showinfo("Bilan", bilan)

root = tk.Tk()
root.title("Organiseur de Bureau Pro")
root.geometry("500x520")

# --- ASTUCE ICÔNE ---
# Pour mettre une icône : trouvez un fichier .ico et utilisez :
# root.iconbitmap("mon_icone.ico")
# En attendant, on utilise une icône système Windows par défaut :
try:
    root.wm_iconbitmap(default=None) # Remplacez None par le chemin d'un .ico si vous en avez un
except:
    pass

tk.Label(root, text="Dossiers à organiser :", font=("Segoe UI", 11, "bold")).pack(pady=15)

liste_widget = tk.Listbox(root, selectmode=tk.MULTIPLE, width=60, height=12, font=("Segoe UI", 9))
liste_widget.pack(padx=20, pady=5)

cadre = tk.Frame(root)
cadre.pack(pady=10)

tk.Button(cadre, text=" ➕ Ajouter ", command=ajouter_dossier, width=12).pack(side=tk.LEFT, padx=5)
tk.Button(cadre, text=" ➖ Retirer ", command=supprimer_selection, width=12).pack(side=tk.LEFT, padx=5)

btn_go = tk.Button(root, text="LANCER LE RANGEMENT", bg="#0078D4", fg="white", 
                   font=("Segoe UI", 12, "bold"), relief="flat", command=lancer_rangement)
btn_go.pack(pady=25, ipady=12, fill=tk.X, padx=50)

root.mainloop()
