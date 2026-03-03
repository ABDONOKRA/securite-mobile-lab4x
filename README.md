# securite-mobile-lab4x

## Task 1 — Préparer le workspace et vérifier l'APK
<img width="964" height="808" alt="image" src="https://github.com/user-attachments/assets/741e2573-208d-4ced-8833-1c244444cb0f" />

## Task 2 — Extraire/obtenir l'APK 
l'apk deja existe 
<img width="557" height="229" alt="image" src="https://github.com/user-attachments/assets/fa406787-e1fd-445f-b5fa-e91eaa132e19" />

## Task 3 — Analyse avec JADX GUI 
decompilation de lapk 

<img width="954" height="326" alt="image" src="https://github.com/user-attachments/assets/18d3a302-3b51-4cc8-a888-cbe9e4ddc10f" />

sources/ → Code Java décompilé
resources/ → AndroidManifest.xml, strings, etc.


<img width="934" height="664" alt="image" src="https://github.com/user-attachments/assets/ca2ac7e1-f965-4e28-a24d-3d3fdd0adb8d" />

# 🔍 ANALYSE du Manifeste (TASK 3 - Partie 1)

## 📋 Informations collectées :

| Élément | Valeur | Remarque |
|---------|--------|----------|
| **Package** | `owasp.mstg.uncrackable1` | ✅ |
| **Version** | 1.0 (versionCode: 1) | ✅ |
| **SDK** | Min 19 (Android 4.4) → Target 28 (Android 9) | ✅ |
| **Activité principale** | `sg.vantagepoint.uncrackable1.MainActivity` | ✅ |
| **Permissions** | AUCUNE ! | ✅ (pas de uses-permission) |
| **Composants exportés** | Aucun explicite | ✅ |
| **Debuggable** | NON | ✅ (pas de android:debuggable="true") |
| **AllowBackup** | OUI | ⚠️ Risque de sécurité ! |


### Explorer le CODE SOURCE décompilé

<img width="1152" height="904" alt="image" src="https://github.com/user-attachments/assets/735cf7d5-13dd-45a1-bcfc-cf8f511af7de" />
 ANALYSE CRITIQUE du MainActivity.java

## 🚨 Protections détectées :

| Protection | Méthode | Comportement |
|------------|---------|--------------|
| **Root Detection** | `c.a()`, `c.b()`, `c.c()` | Vérifient si le téléphone est rooté |
| **Debugger Detection** | `b.a()` | Vérifie si l'app est débuggable |

### ⚠️ Conséquence :
Si détecté → Message **"Root detected!"** ou **"App is debuggable!"** puis `System.exit(0)` (fermeture forcée)

---

## 🔑 LE SECRET EST ICI :

```java
if (a.a(obj)) {  // ← Cette fonction vérifie le secret !
    create.setTitle("Success!");
    str = "This is the correct secret.";
}


