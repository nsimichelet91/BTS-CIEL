---
puppeteer:
  format: A4
  margin:
    top: 1cm
    bottom: 1.5cm
    left: 1.5cm
    right: 1.5cm
  displayHeaderFooter: true
  printBackground: true
  headerTemplate: "<span></span>"
  footerTemplate: <div style='font-size:10px; width:100%; display:flex; justify-content:space-between; padding:0 1cm;'>
      <span>tp01_sentinel_sondes_printf_scanf_types_op</span>
      <span>Page <span class='pageNumber'></span> / <span class='totalPages'></span></span>
      <span>CC BY-NC-SA</span>
    </div>
---

### 🧭 Mission Sentinel : Surveillance d’un data center

Vous faites partie d'une petite équipe technique chargée de **surveiller et maintenir un centre de données** (data center) hébergeant des serveurs informatiques.

Ce centre ne fonctionne pas avec une interface graphique, mais avec des outils de **diagnostic en ligne de commande**, écrits en **langage C**. Votre mission : **déboguer, tester et améliorer ces outils** pour vous assurer que toutes les machines fonctionnent correctement.

Au fil des séances, vous apprendrez à :

- Lire et afficher des données système (températures, charge processeur, erreurs)

- Faire des calculs simples et des vérifications

- Analyser des états et automatiser des tests

- Lire et transformer des données binaires et hexadécimales

- Créer des mini-interfaces console pour suivre le fonctionnement du data-center.

## 🌡 TP1 – Lecture de sondes système

### 🎯 Objectifs pédagogiques

- Comprendre et utiliser `printf` et `scanf`
- Connaître les **types de variables** (entiers, flottants, caractères, booléens)
- Apprendre les **formats d'affichage et de lecture**
- Utiliser la commande `sizeof` et les plages de valeurs

!!! Warning IMPORTANT
    1. Pour l'ensemble des exercices suivants, le **dossier de travail TP_C** doit être ouvert dans VS Code. Voir "**Guide d'utilisation de VS Code**". 
    2. Il faut utiliser la **documentation_prinf_scanf** fournie.

### 🔗 Partie 1 – Connexion à la console Sentinel

#### 💻 Exercice 1 - Identification de l’agent

1. Ouvrir le fichier *tp01_sentinel_sondes.c* dans VS-Code (voir *guide_utilisation_vs-code.pdf*)
2. Lire et analyser le code.
3. Complèter la dernière ligne du programme suivant qui :

   - Demande à l’utilisateur son prénom
   - Affiche un message de bienvenue personnalisé  

<div class="pagebreak"></div>

!!! Example Code
    ```c
        //connexion au système
        // Affiche un message d’introduction à l’utilisateur
        printf("Connexion au système...\n");

        // Déclare une variable de type tableau de caractères (chaîne de caractères) 
        // qui peut contenir jusqu’à 29 caractères 
        // + 1 caractère nul '\0' (terminaison de chaîne)
        char prenomAgent[30];

        // Invite l’utilisateur à saisir le prénom de l’agent
        printf("Entrez le prénom de l'agent : ");

        // Lit la saisie utilisateur depuis le clavier 
        // et la stocke dans la variable `prenomAgent`
        // %29s : lit une chaîne de caractères avec une limite de 29 caractères 
        scanf("%29s", prenomAgent);

        // Affiche un message de bienvenue, mais ici la fonction est incorrecte :
        // il manque l'insertion de la variable contenant le prénom de l'agent 
        // ainsi que le formattage approprié
        printf("Bienvenue, agent ... Lancement du diagnostic ! \n", ...);
    ```
!!! Example Sortie console
    ```c
    Bienvenue, agent Alice. Lancement du diagnostic !
    ```

### 📑 Partie 2 – Types et formatages

#### 💻 Exercice 2 – Test des types
1. Ouvrir le fichier *tp01_tests_affichages.c*.
2. A l'aide de la documentation fournie (*documentation_printf_scanf.pdf*)

  Pour chaque ligne du tableau :
  - Déclarer la **variable**
  - Demander la saisie de la **valeur** avec `scanf`
  - Afficher sa **valeur** avec `printf`
  - Afficher sa **taille** avec `sizeof`

<div class="pagebreak"></div>

3. Complèter le tableau dans le rapport *tp01_rapport* fourni pour comparer l’affichage des types suivants :

!!! Example Exemple de Code
    ```c
    int val;
    scanf("%d", &val)
    printf("val = %d\n", val);
    // Le format %zu permet d’afficher la taille en octets d'une variable.
    printf("Taille : %zu octets\n", sizeof(val)); 
    ```


|Nom de la variable| Type              | Valeur testée   | Format pour `scanf`       | Format pour `printf` | Taille (en octets) |
|:----------------:|:-----------------:|:---------------:|:-------------------------:|:--------------------:|:------------------:|
|  lettre          | `char`            |   'A'           | ` %c (espace devant %)`   | `%c`                 |       1            |
|  effectif        | `unsigned char`   |   33            |           ...             |         ...          |       ...          |
|  annee           | `short`           |   -1000         |           ...             |         ...          |       ...          |
|  angle           | `unsigned short`  |   270           |           ...             |         ...          |       ...          |
|  profondeur      | `int`             |   -1234         |           ...             |         ...          |       ...          |
|  altitude        | `unsigned int`    |   4810          |           ...             |         ...          |       ...          |
|  vitesse         | `long`            |   300000000     |           ...             |         ...          |       ...          |
|  distance        | `unsigned long`   |   152000000000  |           ...             |         ...          |       ...          |
|  temperature     | `float`           |   37.2          |           ...             |         ...          |       ...          |
|  pi              | `double`          | 3.14159265358979|           ...             |         ...          |       ...          |

4. Ouvrir et exécuter le programme *tp01_plages_valeurs_types.c* pour compléter le deuxième tableau du rapport.

### 📈 Partie 3 – Lecture simple de sondes

#### 💻 Exercice 3 – Lecture de données système

A l'aide de la documentation *documentation_printf_scanf.pdf*:
**Ajouter** au programme de l'exercice 1 (*tp01_sentinel_sondes.c*), le code qui :

1. Demande à l’utilisateur d’entrer :
   - La température du processeur (`float`)
   - Le niveau de charge CPU (`int`)
   - Une lettre indiquant l'état du serveur, vous proposerez `'A'` pour actif - `'E'` pour erreur (`char`)
2. Affiche ensuite ces trois valeurs avec des messages clairs.
   Par exemple : 

  !!! Example Console 
      ```c
      Température du processeur : 45.58 °C (avec 2 décimales)
      Niveau de charge CPU : 75%  
      État du serveur : A
      ```

  #### 📢 Caractères d'échappement utiles 

  | Caractère | Signification            |
  |:---------:|--------------------------|
  | `\n`      | Saut de ligne            |
  | `%%`      | Pourcentage %            |


### 📌 Partie 4 – Synthèse : Vérification système

#### 💻 Mini-projet : Diagnostic du système Sentinel – Data Center

1. Ouvrir le fichier *tp01_mini-projet.c*.

Votre mission : développer un programme de **diagnostic automatique** qui vérifie les paramètres vitaux du **système de refroidissement** du data center.

Ce programme :
- **interroge** les sondes du système
- **analyse** les paramètres mesurés
- **affiche** un rapport de diagnostic clair

#### 🎯 Objectifs pédagogiques

- Utiliser `scanf` / `printf` efficacement
- Pratiquer les types `int`, `unsigned`, `double`, `char[]`
- Maîtriser le formatage d'affichage (`%d`, `%lf`, `%u`, `%s`, `%c`, etc.)
- Utiliser des caractères d’échappement (`\n`, `%%`)

#### 🧾 Recommandations de rédaction du code

- Déclarez toutes les variables en début de programme.
- Utilisez des noms de variables explicites et cohérents.
- Commentez votre code pour expliquer les étapes clés.
- Utilisez des blocs visuellement clairs (espacement, indentation).
- Respectez les consignes d'affichage pour rendre le rapport lisible.

<div class="pagebreak"></div>

#### 🔢 Données à saisir

Demandez à l'utilisateur de fournir les valeurs suivantes :

| Description                             | Nom de variable    | Type            | Exemple                  |
|--------------------------------------   |:------------------:|:---------------:|:------------------------:|
| Température eau circuit 1 en °c         | `tempEau1`         | `float`         | `13.86`                  |
| Température eau circuit 2 en °c         | `tempEau2`         | `float`         | `14.12`                  |
| Température salle serveur zone 1 en °c  | `tempZone1`        | `float`         | `23.7`                   |
| Température salle serveur zone 2 en °c  | `tempZone2`        | `float`         | `23.4`                   |
| Pression dans les circuits en hPa       | `pression`         | `unsigned int`  | `112`                    |
| Débit d’eau dans les circuits en L/min  | `debit`            | `unsigned int`  | `280`                    |
| Numéro du diagnostic                    | `numDiag`          | `unsigned short`| `234`                    |
| Identifiant de la sonde                 | `idSonde`          | `unsigned short`| `17`                     |
| Chemin du fichier de sauvegarde         | `cheminLog[50]`    | `char[]`        | `"C:\logs\rapport.txt"`  |

<br><br>

#### 🧮 Calculs à effectuer

1. Température moyenne de l’eau des 2 circuits
2. Température moyenne des 2 zones
3. Écart des températures entre les 2 circuits
4. Écart des températures entre les 2 zones
5. Conversion de la pression en Pascal (1 hPa = 100 Pa)
6. Niveau du débit en % (100% = 300 L/min)
7. Incrémenter le numéro du diagnostic (ajouter 1 au numéro saisi)
8. Déterminer si le numéro de sonde est pair (égale à 1 si le numéro est pair)

<div class="pagebreak"></div>

!!! abstract Opérateurs mathématiques de base
    
    | opérateur                            | symbole | 
    |:--------------:                      |:-------:|
    |multiplication                        |*        |
    |division fractionnaire ou entière     |/        |
    |modulo (reste de la division entière) | %       |
    |addition                              |+        |
    |soustraction                          |-        |
    |incrémentation                        |++       |
    |décrémentation                        |--       |
  
    
      !!! Example Code : exemple d'incrémentation
          ```c
          unsigned short points = 9;
          points++ \\incrémente la valeur de variable points de 1 (ajoute 1)
          ```

    **Les priorités entre ces opérateurs sont les mêmes qu'en mathématiques.**

!!! abstract Valeur absolue

    En langage C, pour calculer la valeur absolue d'un nombre (c’est-à-dire la distance à zéro sans tenir compte du signe), on utilise différentes fonctions selon le type du nombre.

    #### 1. Fonction `abs()`

      - Utilisée uniquement pour les **entiers** (`int`).
      - Définie dans la bibliothèque standard `<stdlib.h>`.

  
      !!! Example Exemple de code
        ```c
        #include <stdlib.h>
        #include <stdio.h>

        int main() {
            int x = -5;
            int valeur_absolue = abs(x);
            printf("abs(%d) = %d\n", x, valeur_absolue);
            return 0;
        }
        ```

      !!! Example Console        
        ```c
        abs(-5) = 5
        ```

    #### 2. Fonction fabs()

      - Utilisée pour les nombres à virgule flottante (double).
      - Définie dans la bibliothèque `<math.h>`.
      - Il existe aussi `fabsf()` pour `float` et `fabsl()` pour long double.

    #### Exemple :

      !!! Example Code
        ```c
        #include <math.h> \\ import de la bibliothèque math
        #include <stdio.h>

        int main() {
            double x = -3.14;
            double valeur_absolue = fabs(x);
            printf("fabs(%.2f) = %.2f\n", x, valeur_absolue);
            return 0;
        }
        ```
      !!! Example Console   
        ```c
        fabs(-3.14) = 3.14
        ```
    #### Résumé en tableau 

    | Type de variable  | Fonction à utiliser | Bibliothèque  |
    |:-----------------:|:-------------------:|:-------------:|
    | `int`             | `abs()`            | `<stdlib.h>`  |
    | `float`           | `fabsf()`          | `<math.h>`    |
    | `double`          | `fabs()`           | `<math.h>`    |
    | `long double`     | `fabsl()`          | `<math.h>`    |


#### 🖨️ Affichage attendu

Affichez un **rapport clair** à l'écran :

```txt
--------------------------------------------------------
          🔍 DIAGNOSTIC DU SYSTÈME SENTINEL
--------------------------------------------------------
🔢 Numéro prochain diagnostic   : 235

🆔 Sonde ID                     : 17
🆔 Parité sonde                 : 0

📁 Chemin log                   : 'C:\logs\rapport.txt'

🌡️  Temp. eau 1                  : 13.86 °C
🌡️  Temp. eau 2                  : 14.12 °C
📊 Ecart temp. eau              :  0.26 °C

🌡️  Temp. zone 1                 : 23.7 °C
🌡️  Temp. zone 2                 : 23.4 °C
📊 Ecart temp. zone             :  0.3 °C

🧪 Pression circuit             : 11200 Pa
🚿 Débit                        : 280 L/min
🚿 Niveau de débit              : 93.33 %
--------------------------------------------------------
```

#### 💾 Conseils de sauvegarde

- Enregistrez régulièrement vos fichiers (res)
- Faites une sauvegarde de sécurité sur :
  - Une clé USB personnelle,
  - OU dans votre dossier Cloud,
  - OU en envoyant le fichier à vous-même par e-mail.

#### ✅ Ce que vous devez rendre

Votre compte-rendu doit :
- être rédigé avec libreOffice au format odt, docx, md
- avoir une page de garde avec le titre du TP, votre nom et la date de fin du TP,
- être structuré et compréhensible et doit donc correspondre à la structure du sujet de TP.
