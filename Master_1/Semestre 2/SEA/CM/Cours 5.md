# Gestion de la mémoire et pagination

## 1. Introduction

### Hiérarchie mémoire

- La mémoire est organisée en **niveaux hiérarchiques** :
    1. **Registres** (ultra-rapide, très petite capacité).
    2. **Caches (L1, L2, L3, L4)** (rapides, capacité croissante).
    3. **Mémoire centrale (DRAM)** (plus lente, plus grande).
    4. **Disque / SSD / Swap** (très lent, énorme capacité).
- **Principe de localité** :
    - **Localité spatiale** : accès à des adresses proches les unes des autres.
    - **Localité temporelle** : accès répétitif aux mêmes données sur une courte période.

---

## 2. Gestion des caches

### Principe général

- Les caches contiennent une partie de la mémoire principale pour accélérer les accès.
- **Taux de défaut** : proportion d'accès mémoire qui ne trouvent pas la donnée en cache.

### Types de caches

- **Direct-mapped** : chaque bloc mémoire ne peut être stocké qu’à un emplacement précis.
- **Fully associative** : chaque bloc peut être placé n'importe où dans le cache.
- **Set-associative** : compromis entre les deux précédents (ex. cache à 2 voies).

### Politiques de gestion des caches

- **Politique d’écriture** :
    - _Write-through_ : écriture en mémoire immédiate.
    - _Write-back_ : écriture en mémoire différée.
- **Politique de remplacement** :
    - _LRU (Least Recently Used)_ : éviction de la page la moins récemment utilisée.
    - _FIFO_ : éviction de la première page chargée.
    - _Random_ : remplacement aléatoire.
    - _Seconde chance_ : variante améliorée de LRU utilisant un bit d’accès.

---

## 3. Mécanisme de pagination

### Concept

- **Adresse virtuelle** : adresse utilisée par le programme.
- **Adresse physique** : adresse réelle en mémoire centrale.
- La **pagination** permet de faire correspondre des adresses virtuelles à des adresses physiques via une **table des pages (TPV)**.
- Unité de gestion mémoire (**MMU - Memory Management Unit**) gère cette traduction.

### Fonctionnement

1. Une adresse virtuelle est découpée en :
    - **Numéro de page virtuelle (PV)**.
    - **Offset dans la page**.
2. La **table des pages** associe un PV à un numéro de **page physique (PP)**.
3. Si la page demandée n'est pas en mémoire : **défaut de page**, chargement depuis le disque.

---

## 4. Pagination à la demande

### Objectifs

- Permet d'exécuter des programmes plus grands que la mémoire disponible.
- Charge uniquement les **pages nécessaires** en mémoire.
- Optimise l'utilisation de la mémoire physique.

### Mécanisme

1. **Accès à une adresse virtuelle inconnue**.
2. La **MMU génère une exception** (défaut de page).
3. L’OS récupère la page depuis le disque (**swap** ou fichier).
4. Mise à jour de la **table des pages**.
5. Redémarrage de l'instruction fautive.

### Bits de gestion des pages

- **Bit V (Validité)** : indique si la page est présente en mémoire physique.
- **Bit U (Utilisation)** : activé lorsqu’une page est accédée (utile pour l’algorithme de remplacement de pages).
- **Bit M (Modification ou Dirty Bit)** : activé lorsqu’une page a été modifiée (nécessite une écriture sur disque en cas de remplacement).
- **Bits de protection (RWX)** : définissent les permissions de la page (Lecture/Écriture/Exécution).

### Gestion des défauts de page

- **Swap** : stockage temporaire des pages sur le disque lorsque la RAM est pleine.
- **Recopie différée (Write-back)** : seules les pages modifiées sont recopiées sur le disque.

### Stratégies de remplacement de page

- **FIFO** : remplace la plus ancienne page chargée.
- **LRU** : remplace la page la moins récemment utilisée.
- **Second chance (Horloge)** :
    - Variante de LRU utilisant le **bit U**.
    - Les pages sont organisées en **cercle** (comme une horloge).
    - Lorsqu’une page est candidate à être remplacée :
        1. Si **U = 1**, on met **U = 0** et on passe à la page suivante.
        2. Si **U = 0**, la page est remplacée.
    - Cela permet d’éviter de remplacer immédiatement les pages récemment utilisées.

---

## 5. Caches de traduction (TLB - Translation Lookaside Buffer)

### Rôle du TLB

- **Accélérer la traduction des adresses** en mémorisant les correspondances récentes.
- Si la traduction est trouvée dans le TLB : accès rapide.
- Sinon : recherche dans la **table des pages** (coût élevé).

### Problèmes et solutions

- **Changement de contexte** : vidage du TLB ou utilisation de **tags ASID**.
- **Hiérarchisation des TLB** pour améliorer les performances.

---

## 6. Gestion mémoire avancée

### Tables des pages hiérarchiques

- Solution pour réduire la taille des tables des pages.
- **Deux niveaux** :
    1. Table des livres (contient des pointeurs vers des tables de pages).
    2. Tables de pages.
- **Avantages** : réduit la mémoire allouée aux tables des pages.

### Table des pages inversée

- Stocke les correspondances **page physique -> page virtuelle**.
- Utilisée dans les architectures où l'espace d'adressage est très grand (ex. 64 bits).
- **Avantage** : réduit la taille des structures de pagination.
- **Inconvénient** : nécessite une **table de hachage** pour rechercher les correspondances.

---

## 7. Partage de mémoire et sécurité

### Objectifs

- **Partager des bibliothèques** et des segments de mémoire entre processus.
- **Optimiser l’utilisation** de la mémoire sans duplication.

### Techniques

1. **Copy-on-write (COW)** :
    - Pages partagées tant qu'elles ne sont pas modifiées.
    - Si modification, duplication de la page.
2. **Fichiers mappés en mémoire** (`mmap`) :
    - Associe une zone mémoire à un fichier.
    - Exemple :
        
        ```c
        int fd = open("fichier.txt", O_RDWR);
        char *addr = mmap(NULL, 4096, PROT_READ | PROT_WRITE, MAP_SHARED, fd, 0);
        ```
        

### Sécurité

- **Protection des pages** :
    - `r` (lecture seule), `w` (écriture), `x` (exécutable).
- **Isolation mémoire entre processus** via la table des pages.

---

## 8. Problèmes de performance

### Phénomène d’écroulement (**trashing**)

- Trop de défauts de page → le système passe son temps à paginer.
- Solutions :
    - **Limiter le nombre de processus** en mémoire.
    - **Allouer suffisamment de pages** à chaque processus.

### Taille des pages et impact

- **Pages plus grandes** :
    - Moins d’entrées dans la table des pages.
    - Mais **plus de fragmentation** (espace perdu).

---

