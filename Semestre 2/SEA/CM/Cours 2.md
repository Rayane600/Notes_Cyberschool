# Synchronisation des processus

## 1. Introduction à la synchronisation

### Exclusion mutuelle

- Problème de coopération entre processus accédant à une mémoire partagée.
- Problèmes d'incohérences dus aux opérations élémentaires non atomiques.
- Nécessité de synchroniser l'accès aux ressources partagées.

### Notion de section critique

- Partie du programme pouvant causer une incohérence si exécutée en parallèle.
- Une **section critique** doit être **indivisible** et **atomique**.
- Une **exécution parallèle** est acceptable si elle produit un résultat équivalent à une exécution séquentielle.

## 2. Exemples de problèmes de synchronisation

### 2.1 Ressource matérielle partagée

- Ex. Impression simultanée par deux processus (`P1` et `P2`).
- Sans synchronisation, les caractères peuvent être imprimés dans un ordre imprévisible.
- Objectif : garantir un ordre strict (`abcdABCD` ou `ABCDabcd`).

### 2.2 Ressource logicielle partagée

- Ex. Réservation d'une place d'avion avec une variable `libres`.
- Risque de **race condition** : deux processus peuvent décrémenter `libres` en parallèle, menant à un état incohérent (`libres = -1`).
- Solution : protéger cette section de code par une synchronisation explicite.

## 3. Exclusion mutuelle et outils de synchronisation

### 3.1 Exclusion mutuelle

- Une ressource partagée doit être manipulée en **exclusion mutuelle**.
- La structure d'un code assurant cette propriété :
    
    ```c
    <entrée en section critique>;
    <section critique>;
    <sortie de section critique>;
    ```
    

### 3.2 Mutex (Mutual Exclusion Locks)

- Outil de synchronisation simple : un verrou binaire (verrouillé / déverrouillé).
- API POSIX `pthreads` :
    
    ```c
    pthread_mutex_t mutex;
    pthread_mutex_init(&mutex, NULL);
    pthread_mutex_lock(&mutex);
    // Section critique
    pthread_mutex_unlock(&mutex);
    pthread_mutex_destroy(&mutex);
    ```
    

## 4. Sémaphores

### 4.1 Introduction aux sémaphores

- Introduits par **Edsger Dijkstra** (1965).
- Composés d'un **compteur** et d'opérations atomiques **P (Wait)** et **V (Signal)**.
- Utilisés pour réguler l'accès aux ressources partagées.

### 4.2 Interface des sémaphores POSIX

- API POSIX :
    
    ```c
    sem_t sem;
    sem_init(&sem, 0, 1); // Initialisation avec 1 (exclusion mutuelle)
    sem_wait(&sem); // P() - Attente
    // Section critique
    sem_post(&sem); // V() - Libération
    sem_destroy(&sem);
    ```
    

## 5. Problèmes classiques de synchronisation

### 5.1 Producteur / Consommateur

- Un producteur génère des données que le consommateur doit traiter.
- Nécessite un **tampon partagé** pour stocker les données temporairement.
- Utilisation de **sémaphores** :
    
    ```c
    sem_t s_plein, s_vide;
    sem_init(&s_plein, 0, 0);
    sem_init(&s_vide, 0, NB_TAMPONS);
    
    // Producteur
    sem_wait(&s_vide);
    produire(d);
    sem_post(&s_plein);
    
    // Consommateur
    sem_wait(&s_plein);
    consommer(d);
    sem_post(&s_vide);
    ```
    

### 5.2 Lecteurs / Rédacteurs

- Plusieurs **lecteurs** peuvent lire simultanément, mais un **rédacteur** doit être seul.
- **Protocole basé sur des verrous lecteurs/rédacteurs** :
    
    ```c
    pthread_rwlock_t rwlock;
    pthread_rwlock_rdlock(&rwlock); // Accès en lecture
    // Lecture
    pthread_rwlock_unlock(&rwlock);
    
    pthread_rwlock_wrlock(&rwlock); // Accès en écriture
    // Écriture
    pthread_rwlock_unlock(&rwlock);
    ```
    

## 6. Implémentations matérielles et logicielles

### 6.1 Instructions atomiques

- **Test-and-Set (TAS)** : instruction matérielle garantissant une opération indivisible.
- **Fetch-and-Add**, **Compare-and-Swap** (CAS).
- Utilisation en **spin-locks** :
    
    ```c
    while (test_and_set(&lock)) ; // Attente active
    // Section critique
    lock = 0;
    ```
    

### 6.2 Attente active vs blocage

- **Attente active (spinning)** :
    - Utilisation du processeur pendant l'attente.
    - Utilisé pour des sections critiques très courtes.
- **Blocage (sémaphores, mutex)** :
    - Libère le CPU si la ressource n'est pas disponible.
    - Privilégié pour les sections critiques longues.

## 7. Problèmes d'interblocage (Deadlocks)

### 7.1 Définition

- Blocage mutuel de deux processus essayant d'accéder à des ressources verrouillées par l'autre.
- Ex. :
    
    ```c
    P(mutex1); P(mutex2); // P1
    P(mutex2); P(mutex1); // P2 → DEADLOCK
    ```
    

### 7.2 Solutions

- **Évitement** : Ordre global sur l'acquisition des ressources.
- **Détection et récupération** : Détection et relâchement de ressources.
- **Prévention** : Allocation atomique des ressources.

## 8. Outils et techniques avancées

### 8.1 Synchronisation par message

- Transmission de données + synchronisation.
- Interface POSIX :
    
    ```c
    mq_send(); mq_receive();
    ```
    

### 8.2 Moniteurs et Java

- **Moniteurs** (synchronisation au niveau langage) :
    
    ```java
    synchronized void methode() {
        // Section critique
    }
    ```
    
