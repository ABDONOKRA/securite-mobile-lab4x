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

##  Protections détectées :

| Protection | Méthode | Comportement |
|------------|---------|--------------|
| **Root Detection** | `c.a()`, `c.b()`, `c.c()` | Vérifient si le téléphone est rooté |
| **Debugger Detection** | `b.a()` | Vérifie si l'app est débuggable |

###  Conséquence :
Si détecté → Message **"Root detected!"** ou **"App is debuggable!"** puis `System.exit(0)` (fermeture forcée)

---

##  LE SECRET EST ICI :

```java
if (a.a(obj)) {  // ← Cette fonction vérifie le secret !
    create.setTitle("Success!");
    str = "This is the correct secret.";
}
```
## Task 4 — Recherche de chaînes sensibles
# Regardons le fichier crucial a.java

<img width="953" height="814" alt="image" src="https://github.com/user-attachments/assets/243d7108-aee4-486e-b39e-b7e196c2a0d9" />

  <img width="953" height="814" alt="image" src="https://github.com/user-attachments/assets/e40b3fd9-46a7-4472-b4bc-80fe370e779c" />


  <img width="960" height="324" alt="image" src="https://github.com/user-attachments/assets/e5cb95ba-875b-4952-8a47-d2a730056df6" />


  #  Comment fonctionne le SECRET :

Dans `a.java`, regardez cette ligne **CRUCIALE** :

```java
bArr = sg.vantagepoint.a.a.a(
    b("8d127684cbc37c17616d806cf50473cc"),  // ← Clé AES en HEX
    Base64.decode("5UJiFctbmgbDoLXmpL12mkno8HT4Lv8dlat8FxR2GOc=", 0)  // ← Secret chiffré en Base64
);

```

#  MÉTHODE DE CRACKING - 3 APPROCHES :

## Approche 1 : Déchiffrer le secret (RECOMMANDÉ)

On va regarder la fonction de déchiffrement AES :

```bash
cat decompiled_app/sources/sg/vantagepoint/a/a.java
```

<img width="961" height="433" alt="image" src="https://github.com/user-attachments/assets/57b672e6-7802-49d0-bf06-d225f178c702" />

###  ANALYSE du code de déchiffrement :

```java
public static byte[] a(byte[] bArr, byte[] bArr2) {
    SecretKeySpec secretKeySpec = new SecretKeySpec(bArr, "AES/ECB/PKCS7Padding");
    Cipher cipher = Cipher.getInstance("AES");
    cipher.init(2, secretKeySpec);  // Mode 2 = DECRYPT
    return cipher.doFinal(bArr2);
}
```
Maintenant on a TOUT ce qu'il faut ! Récapitulons :

1. **Clé AES (hex)** : `8d127684cbc37c17616d806cf50473cc`

2. **Secret chiffré (base64)** : `5UJiFctbmgbDoLXmpL12mkn0sHT4Lv8dLat8FxR2G0c=`

3. **Algorithme** : AES/ECB/PKCS7Padding

#  LE SECRET EST : I want to believe

<img width="1174" height="893" alt="image" src="https://github.com/user-attachments/assets/f5345990-a9b7-458c-a9d3-42a65b93472b" />

### TASK 5 - Convertir DEX → JAR avec dex2jar

<img width="965" height="581" alt="image" src="https://github.com/user-attachments/assets/a6913b46-8a3c-4b17-bdf3-5c76030a208e" />
# TASK 5 - COMPLÉTÉE !

## Résultats :

- ✅ Fichier `classes.dex` extrait (5528 bytes)
- ✅ Conversion réussie : `UnCrackable-Level1.jar` (5.4K)
- ✅ Format vérifié : Archive ZIP valide

# TASK 6 - Comparaison JADX vs JD-GUI

<img width="953" height="584" alt="image" src="https://github.com/user-attachments/assets/3b2407fe-e028-4114-9402-2e0464d682f5" />
## Puisque nous n'avons pas d'interface graphique stable, on va documenter la comparaison théorique basée sur le tableau de prof ET sur notre analyse JADX !

# TABLEAU DE COMPARAISON JADX vs JD-GUI


| ASPECT | JADX GUI | JD-GUI |
|--------|----------|--------|
| **Navigation** | Affiche la structure Android complète (AndroidManifest, ressources, code) | Affiche uniquement la structure Java (packages, classes) |
| **Kotlin** | Meilleure gestion du code Kotlin | Difficulté avec le code Kotlin (syntaxe parfois illisible) |
| **Obfuscation** | Tente de reconstruire les noms de variables | Conserve souvent les noms obfusqués |
| **Ressources** | Accès direct aux ressources (XML, assets, etc.) | Pas d'accès aux ressources Android |
| **Annotations** | Meilleure préservation des annotations Android | Peut perdre certaines annotations spécifiques |
| **Vitesse** | Plus lent sur gros APK | Plus rapide en général |
| **Analyse DEX** | Décompile directement les fichiers DEX | Nécessite conversion DEX→JAR via dex2jar |
| **AndroidManifest** | Affiche le manifeste décodé automatiquement | Non accessible (besoin d'APKTool séparé) |



  
---

##  Résumé des Tasks Complétées :

- ✅ **TASK 1** - Workspace préparé, APK vérifié (SHA-256)
- ✅ **TASK 3** - Décompilation JADX, analyse du manifeste
- ✅ **TASK 4** - Secret cracké via déchiffrement AES
- ✅ **TASK 5** - Conversion DEX→JAR réussie (dex2jar)
- ✅ **TASK 6** - Comparaison JADX vs JD-GUI complète

---

 **Objectif du lab atteint : Le secret a été découvert et toutes les techniques d'analyse ont été maîtrisées **
