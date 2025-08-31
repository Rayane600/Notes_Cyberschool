# Allocation Dynamique de Mémoire

## 1. Objectifs
- Allouer dynamiquement de la mémoire à l'exécution.
- Gérer des tailles et durées de vie variables.

### Fonctions typiques:
```c
void* malloc(size_t size);  // Alloue une zone mémoire
void free(void* ptr);       // Libère la mémoire allouée
```

---

## 2. Gestion de Zones de Mémoire
- **Zone mémoire** : suite d'emplacements contigus.
  - Adresse de début et taille variable.
  - **Zone libre (trou)** vs. **Zone occupée**.

- **Allocation** : trouver une zone libre.
- **Libération** : réintégrer la zone libérée dans la structure.

---

## 3. Fragmentation

### Fragmentation externe :
- Mélange zones libres/occupées.
- Fusionner trous adjacents.
- Petits trous inutilisables.

### Fragmentation interne :
- Allocation de plus de mémoire que nécessaire (arrondi à un multiple).

---

## 4. Classes d'Algorithmes d’Allocation
- **Bitmap**
- **Sequential et Indexed Fits**
- **Buddy Systems**
- **Politiques hybrides** selon la taille des blocs

---

## 5. Algorithme Bitmap
- Taille fixée (`Tmin`).
- 1 bit par bloc : `1` occupé, `0` libre.

**Allocation :**
- Chercher `n` blocs libres consécutifs.
- Mettre les bits à `1`.

**Libération :**
- Vérifier que les bits sont à `1`.
- Mettre les bits à `0`.

**Exemple :**
```
1111010110110000 malloc(30), Tmin = 16
1111010110111100 après allocation
1111010110110000 après libération
```

---

## 6. Sequential Fits
- Chaînage des trous (liste chaînée).
- **Boundary Tags** : Entête/Footer avec taille et état.

**Stratégies :**
- **First Fit** : Premier trou suffisamment grand.
- **Next Fit** : Liste circulaire.
- **Best Fit** : Trou le plus petit suffisant.

---

## 7. Buddy Systems
- Tailles prédéfinies (puissances de deux, Fibonacci).
- Chaque bloc possède un "buddy" avec lequel il fusionne.
- Provoque une fragmentation interne élevée.

---

## 8. Mémoire Virtuelle et Heap
- Tas (heap) : séquence d'adresses virtuelles.
- Pages physiques allouées sur référence uniquement.
- `brk()` et `sbrk()` pour gérer la taille du heap.
  - Utilisées par malloc, éviter utilisation directe.

---

## 9. `ptmalloc2` (Glibc malloc)
- Sous-tas (arena) par thread.
- Tailles arrondies par multiples de 16 octets.
- Structures :
  - **Bins** : Listes doublement chaînées (127 bins).
  - **Fastbins** : LIFO pour petits blocs (<80 octets).
  - **Unsorted bin** : pour réutilisation immédiate avant classement.

**Documentation complémentaire :**
- [Painless intro to the Linux heap](https://sensepost.com/blog/2017/painless-intro-to-the-linux-userland-heap/)
- [Understanding glibc malloc](https://sploitfun.wordpress.com/2015/02/10/understanding-glibc-malloc/)

---

## 10. Guide de Survie à l'Allocation Dynamique

### Bonnes pratiques :
- Éviter si possible l'allocation dynamique.
- Préférer tableaux statiques quand la taille est connue :
```c
#define N 10
char tab[N]; // au lieu de malloc
```

- Utiliser allocations locales (`int tab[n];`) pour durées de vie connues.

### Problèmes courants :
- Mémoire non libérée (fuites mémoire).
- Consommation excessive de mémoire virtuelle.
- Toujours vérifier les cas d'erreurs.

**Outil utile :** `valgrind`

---

## 11. Ramasse-Miettes (Garbage Collection)

### Problèmes de libération manuelle :
- Oubli, double libération, utilisation après libération.

### Techniques :
- **Comptage de références** :
  - Incrémenter/décrémenter compteur sur ajout/suppression de référence.
  - Ne gère pas les cycles de références.

- **Marquage et balayage (Mark-and-Sweep)** :
  - Marquer objets atteignables à partir de racines.
  - Libérer objets non marqués.

### Avantages/Inconvénients :
- Comptage rapide mais ignore cycles.
- Mark-and-sweep libère tous les objets mais introduit des pauses.

### GC modernes :
- Parallèles, incrémentaux, générationnels, réduisent fragmentation.
