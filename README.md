# LAB 6 : Analyse statique d'un APK avec MobSF dans la VM Mobexler

## Informations générales
- **Date d'analyse :** 17 Mars 2026
-  Nom Et Prénom:** Hafssa Azarg
- **APK analysé :** app-debug.apk
- **Outils utilisés :** MobSF v3.9+ dans VM Mobexler

## Objectifs du Lab
L'objectif de ce laboratoire est de réaliser l'analyse statique détaillée d'une application Android (APK) en utilisant l'outil MobSF (Mobile Security Framework) au sein de la machine virtuelle Mobexler. Cela permet d'identifier les vulnérabilités de sécurité, d'étudier le code et la configuration réseau, et de corréler les résultats avec le standard OWASP MASVS.

---

## Étape 1 : Préparation de l'environnement d'analyse
1. Démarrage de la VM Mobexler et connexion.
2. Création de l'arborescence de travail (dans la VM).
   ```bash
   mkdir -p ~/apk_analysis/$(date +%Y-%m-%d)
   cd ~/apk_analysis/$(date +%Y-%m-%d)
   ```
3. Copie de l'APK (ex: `app-debug.apk`) dans le répertoire de travail et calcul de son intégrité via `sha256sum`.
   ```bash
   sha256sum app-debug.apk > apk_hash.txt
   ```
4. Création du fichier de traçabilité `analyse_info.txt`.

![](https://github.com/user-attachments/assets/b9f09f62-88bb-470f-b3ce-fdd17d80abdd)

---

## Étape 2 : Lancement de MobSF
1. Ouverture du terminal pour lancer MobSF :
   ```bash
   cd ~/tools/Mobile-Security-Framework-MobSF
   ./run.sh 127.0.0.1:8000
   ```
2. Accès à l'interface web locale via `http://127.0.0.1:8000`.
3. Validation du démarrage du serveur.

![](https://github.com/user-attachments/assets/7b1ec168-1fcc-4a8b-81b0-438d0a15c220)


## Étape 3 : Import et analyse de l'APK
1. Clic sur **Upload & Analyze** dans l'interface de MobSF.
2. Sélection de l'APK (`app-debug.apk`).
3. Attente de la fin de l'analyse et affichage du tableau de bord global avec le "Security Score".
   
![](https://github.com/user-attachments/assets/15f5c480-0721-42ff-96d8-4f3e3a67ec38)


## Étape 4 : Analyse du manifeste et des permissions
1. Navigation vers l'onglet **App Information** pour voir le package, la version et le SDK.
2. Examen de l'onglet **Manifest Analysis** pour découvrir des configurations dangereuses (p. ex. `android:debuggable="true"` ou `android:allowBackup="true"`).
3. Identification des permissions dangereuses (comme `ACCESS_FINE_LOCATION`, etc.).
4. Analyse des composants exportés (`android:exported="true"`).


![](https://github.com/user-attachments/assets/b77993c6-1260-4f36-ba37-93f5888934d9)

![](https://github.com/user-attachments/assets/fa0b12ca-2a83-479b-b6c2-b5239420084b)


## Étape 5 : Analyse de la configuration réseau
1. Recherche du fichier `network_security_config.xml` dans l'onglet **Files** si existant.
2. Vérification de l'attribut `android:usesCleartextTraffic="true"` ou du support des trafics non chiffrés.
3. Listage des endpoints et URL communiquant avec l'application.

![5038abc5-17f1-4c15-90ef-5b4175ca3236](https://github.com/user-attachments/assets/74b93863-014d-4b8d-80c7-b8a5f5c7faa0)

## Étape 6 : Analyse du code et des ressources
1. Accès à la section **Code Analysis** pour découvrir les vulnérabilités par sévérité.
2. Vérification des secrets embarqués dans **Hardcoded Secrets** (clés d'API, mots de passe).
3. Vérification des **URLs and Emails** ainsi que l'examen manuel des fichiers `.xml` et `.properties`.

![](https://github.com/user-attachments/assets/2c3dcf4d-f5b6-4154-b5ca-b0fd94234199)

![](https://github.com/user-attachments/assets/a3e35c0c-5159-4c5d-8442-d41e2d1ee1ac)

## Étape 7 : Corrélation avec OWASP MASVS
1. Rapprochement des vulnérabilités avec les règles du standard OWASP MASVS (ex: MSTG-STORAGE-14 pour un secret hardcodé).
2. Identification des guides de test liés (MASTG) pour confirmer les vulnérabilités trouvées de façon statique.

> **Note :** Insérez ici une capture d'écran ou un extrait de votre fichier de corrélation MASVS.
> ![Capture d'écran - Corrélation MASVS](./screenshots/etape7_masvs.png)

---

## Étape 8 : Exportation et analyse du rapport complet
1. Dans l'interface web, clic sur **Generate PDF Report** pour obtenir le rapport détaillé.
2. Analyse du rapport généré et validation des vulnérabilités critiques pour trier les faux positifs des vrais problèmes exploitables.

![](https://github.com/user-attachments/assets/a873d84b-1747-4e1e-bf41-7e0329e58606)


## Étape 9 : Rédaction du mini-rapport d'audit
Consultez le fichier `rapport_final.md` pour le résumé exécutif complet et détaillé des vulnérabilités et des recommandations.

