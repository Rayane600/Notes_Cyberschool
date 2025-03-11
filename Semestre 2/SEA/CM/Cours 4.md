# Gestion des entrées/sorties (E/S)

## 1. Introduction

### Définition

- La **gestion des entrées/sorties (E/S)** concerne l'interaction entre le processeur et les périphériques.
- Objectif : permettre l'échange efficace de données entre le système et ses périphériques.

### Composants

1. **Logiciel** : API, pilotes, gestion des tampons.
2. **Matériel** : périphériques, contrôleurs de périphériques.
3. **Mode système** : interface de contrôle, gestion des interruptions.

---

## 2. Structure des systèmes d’E/S

### Interface des systèmes d’E/S

- **Appels système** (niveau utilisateur) : `open()`, `read()`, `write()`, `close()`.
- **Bibliothèques C** : `fopen()`, `fwrite()`, `fread()`.
- **Pilotes de périphériques** : interagissent directement avec le matériel.
- **Périphériques listés sous `/dev`** sous Unix/Linux.

### Interface de contrôle des périphériques

- **Simple contrôleur (coupleur)** : transfert direct entre le processeur et le périphérique.
- **Contrôleur avec DMA (Direct Memory Access)** : le DMA gère le transfert des données sans intervention du processeur.
- **Processeur d’E/S dédié** : gestion autonome des périphériques (ex. cartes réseau).

---

## 3. Modes de gestion des E/S

### 3.1 Attente active (polling)

- Le processeur interroge en boucle l’état du périphérique.
- Exemple de code :
    
    ```c
    while (contrôleur.état == occupé) { ; } // Attente active
    contrôleur.donnée = data;
    ```
    
- **Inconvénients** : inefficace, consomme du temps processeur.
- **Alternative** : scrutation périodique (polling espacé).

### 3.2 Gestion par interruptions

- Le périphérique signale la fin de l’opération via une **interruption matérielle**.
- **Avantages** : évite l’attente active, libère le CPU pour d’autres tâches.
- **Exemple d’implémentation** :
    
    ```c
    sema s_fin(0);
    contrôleur.contrôle = "interruption";
    
    for (i = 0; i < N; i++) {
        P(s_fin);
        contrôleur.donnée = data[i];
    }
    ```
    

### 3.3 Gestion par DMA

- **Décharge le processeur** du transfert de données.
- **Fonctionnement** :
    - Le processeur donne au **DMA** l’adresse source, destination et la taille des données à transférer.
    - Le DMA effectue le transfert et génère une **interruption** une fois terminé.

---

## 4. Gestion des interruptions

### 4.1 Mécanisme

- Lorsqu’une **interruption** est déclenchée :
    1. Sauvegarde du contexte du programme en cours.
    2. Exécution de la **routine d’interruption**.
    3. Reprise de l’exécution normale.

### 4.2 Types d’interruptions

- **Masquables / non masquables** : certaines interruptions critiques ne peuvent pas être désactivées.
- **Prioritaires** : certaines interruptions ont une priorité plus élevée que d’autres.
- **Routine d’interruption courte** : essentielle pour éviter de bloquer le système.

### 4.3 Gestion moderne des interruptions (Linux)

- **Découpage en deux niveaux** :
    - **Upper half** : exécute le minimum nécessaire (ex. acquitter une interruption).
    - **Bottom half** : exécute des tâches plus longues après interruption.

---

## 5. Gestion des disques

### 5.1 Architecture des disques

- **Composants** : plateaux, tête de lecture/écriture, moteur de rotation.
- **Adresse disque** = triplet `(face, piste, secteur)`.

### 5.2 Temps d’accès au disque

1. **Seek time** : temps pour positionner la tête sur la bonne piste.
2. **Latence rotationnelle** : attente du bon secteur sous la tête.
3. **Temps de transfert** : transfert réel des données.

### 5.3 Optimisation

- **Défragmentation** : minimise les déplacements du bras.
- **Politiques d’ordonnancement des requêtes** :
    - **FIFO** : premier arrivé, premier servi.
    - **SCAN (ascenseur)** : parcours les pistes dans un seul sens.
    - **C-SCAN** : ne sert les requêtes que dans un seul sens, retour rapide.
    - **LOOK / C-LOOK** : variation de SCAN avec meilleure adaptation.
- **RAID** (Redundant Array of Independent Disks) : parallélisation et redondance des accès disque.

---

## 6. Discussion : Attente active vs interruptions

|Critère|Attente active|Interruptions|
|---|---|---|
|CPU utilisé ?|Oui, monopolise|Non, libère CPU|
|Changement de contexte ?|Non|Oui (coût)|
|Pertinent si ?|Périphérique rapide|Périphérique lent|

- **Cas où l’attente active est préférable** : périphérique **très rapide**, évite le coût des interruptions.
- **Cas où les interruptions sont préférables** : périphérique **lent**, évite la monopolisation du CPU.

---
