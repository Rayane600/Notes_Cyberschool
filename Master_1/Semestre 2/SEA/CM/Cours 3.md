# Édition de liens

## 1. Introduction

### Définition

- L’**édition de liens** consiste à associer les objets logiques (variables, fonctions) à leurs représentations physiques en mémoire.
- Elle est réalisée par l’**éditeur de liens** qui regroupe le code et les données de plusieurs modules et résout les références externes.

### Étapes de la liaison

1. **Compilation / Assemblage** : conversion du code source en code machine.
2. **Édition de liens** : résolution des références externes.
3. **Chargement** : initialisation en mémoire pour exécution.

## 2. Types d'édition de liens

### 2.1 Édition de liens statique

- Tous les identificateurs sont convertis en **adresses fixes** avant l'exécution.
- Toutes les fonctions appelées sont **incorporées dans l’exécutable**.
- Exemple de compilation et d'édition de liens statique :
    
    ```sh
    gcc -O0 -c main.c
    gcc -O0 -c foo.c
    gcc -o main main.o foo.o
    ```
    
- Le fichier binaire résultant contient tout le code nécessaire à l'exécution.

### Avantages

- Pas de dépendance externe après compilation.
- Temps d'exécution plus rapide (résolution des adresses faite en avance).

### Inconvénients

- Difficulté de mise à jour des bibliothèques (nécessite une recompilation).
- Consommation d'espace disque et mémoire accrue.

---

### 2.2 Édition de liens dynamique

- Les références à certaines fonctions/bibliothèques restent **non résolues** jusqu'à l'exécution.
- Les bibliothèques ne sont pas incorporées dans l’exécutable mais chargées au **moment du besoin**.
- Exemple :
    
    ```sh
    gcc -O0 -c main.c
    gcc -O0 -fPIC -c foo.c
    gcc -shared -o libfoo.so foo.o
    gcc -o main main.o -L. -lfoo
    ```
    
    - `-fPIC` : position-independent code (nécessaire pour les bibliothèques partagées).
    - `-shared` : création d'une bibliothèque dynamique.
    - `-L.` et `-lfoo` : lien avec la bibliothèque dynamique.

### Avantages

- **Facilité de mise à jour** des bibliothèques (pas besoin de recompiler tout l'exécutable).
- **Réduction de la consommation mémoire** grâce au partage de bibliothèques entre processus.

### Inconvénients

- **Temps d’exécution légèrement plus long** (résolution des liens dynamique).
- **Dépendance aux bibliothèques externes** (si une bibliothèque est supprimée ou modifiée, cela peut casser le programme).

## 3. Fonctionnement de l’édition de liens dynamique sous Linux

### Mécanismes utilisés

- **PLT (Procedure Linkage Table)** : table des adresses des fonctions appelées dynamiquement.
- **GOT (Global Offset Table)** : table stockant les adresses réelles des fonctions après leur première résolution.
- **LD_LIBRARY_PATH** : variable d’environnement définissant où chercher les bibliothèques dynamiques.
    
    ```sh
    export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
    ```
    

### Résolution des symboles

- **À la première utilisation d’un symbole** :
    - L'appel passe par `foo@plt`.
    - `PLT` fait un saut vers une adresse inconnue stockée dans la `GOT`.
    - La `GOT` contient une adresse temporaire qui déclenche le **linker dynamique**.
    - Le linker résout l’adresse réelle et met à jour la `GOT`.
- **Au chargement** :
    - Si `LD_BIND_NOW` est activé (`export LD_BIND_NOW=1`), tous les liens sont résolus dès le chargement.

### API pour charger des bibliothèques dynamiques

- **dlopen()** : charge une bibliothèque à l’exécution.
- **dlsym()** : récupère l’adresse d’une fonction.
- **dlclose()** : libère la bibliothèque chargée.
- **dlerror()** : gère les erreurs de liaison.
    
    ```c
    #include <dlfcn.h>
    void *handle = dlopen("libfoo.so", RTLD_LAZY);
    int (*foo)(int) = dlsym(handle, "foo");
    dlclose(handle);
    ```
    

## 4. Outils pour analyser l’édition de liens

### Commandes utiles

- **ld** : éditeur de liens
    
    ```sh
    ld -o mon_programme fichier1.o fichier2.o
    ```
    
- **nm** : affiche la table des symboles
    
    ```sh
    nm mon_programme
    ```
    
- **objdump** : désassemble un exécutable
    
    ```sh
    objdump -d mon_programme
    ```
    
- **readelf** : affiche les sections ELF d'un exécutable
    
    ```sh
    readelf -S mon_programme
    ```
    
- **strip** : supprime les symboles d’un exécutable
    
    ```sh
    strip mon_programme
    ```
    