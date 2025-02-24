# Partie 1
#### **1. Architecture d’un ordinateur**

- **Composants techniques** :
    - **Processeur** : exécute des instructions via un cycle (chargement, décodage, exécution).
    - **Mémoire volatile (RAM)** : stockage temporaire des données.
    - **Stockage non volatile** : disques, mémoire flash.
    - **Périphériques** : claviers, souris, imprimantes, capteurs.
    - **Réseau** : communication externe.
    - **Bus** : interface de communication entre composants.

---

#### **2. Rôle et interface d’un système d’exploitation**

- **Définition** :
    - Abstraction du matériel via une interface logicielle (machine virtuelle).
- **Interface** :
    - Appels système (exemple POSIX) pour :
        - Exécution multi-tâche.
        - Gestion des fichiers et répertoires.
        - Entrées/sorties.
        - Communication et synchronisation (messages, sémaphores, tubes).
        - Gestion du temps (alarmes, accès au temps système).

---

#### **3. Intérêts d’un système d’exploitation**

- **Abstraction** :
    - Masque la complexité du matériel (processeurs, périphériques, contrôleurs).
- **Portabilité** :
    - Code compatible entre différents systèmes.
- **Gestion des ressources** :
    - Processeur : exécution parallèle des tâches.
    - Périphériques : accès synchronisé et parallèle.

---

#### **4. Problèmes liés au multi-tâche**

- **Processeur** :
    - Sauvegarde/restauration des contextes.
    - Synchronisation et ordonnancement des tâches.
- **Mémoire** :
    - Allocation pour nouveaux programmes.
    - Protection et synchronisation des accès concurrents.
- **Périphériques** :
    - Gestion concurrente des accès (ex. impression partagée).

---

#### **5. Unix, Linux et POSIX**

- **POSIX** :
    - Norme d’interfaces système pour UNIX.
    - Gestion standardisée des API.
- **Linux** :
    - Implémentation partielle de POSIX (variable POSIXLY_CORRECT pour conformité).
# Partie 2 
#### **1. Notion de processus**

- **Définition** :
    - Exécution d’un programme : suite d’actions effectuées lors de son exécution.
    - Un même programme peut correspondre à plusieurs processus.
- **Types de processus** :
    - **Coopérants** : tâches parallèles programmées explicitement.
    - **Concurrents** : système arbitre l’utilisation des ressources.

---

#### **2. Parallélisme**

- **Séquentialité** :
    - Un seul processeur : P1 puis P2, exécution non préemptive.
- **Pseudo-parallélisme** :
    - Un seul processeur : exécution partielle alternée de P1 et P2.
    - Une seule action à la fois, impression de parallélisme.
- **Parallélisme réel** :
    - Deux processeurs : P1 sur un processeur, P2 sur l’autre.
    - Exécution réelle de deux actions simultanées.

---

#### **3. Relations temporelles et indéterminisme**

- **Relations temporelles** :
    - Ordre des actions atomiques au sein d’un processus est défini.
    - Temps réel entre actions peut varier (interruptions, autres processus).
    - Raisonnement basé sur un temps logique uniquement.
- **Indéterminisme** :
    - Ordre d’exécution des actions de processus distincts non prédictible.
    - Imposé par l’ordonnanceur ou contraint par des synchronisations explicites.

---

#### **4. Processeur virtuel**

- **Concept** :
    - Chaque processus possède un processeur virtuel (PC, registres).
    - Abstraction des mécanismes réels pour se concentrer sur le parallélisme.
- **Particularités** :
    - Vitesse du processeur virtuel inconnue.
    - Mémoire reste partagée entre processus.

---

#### **5. Processus vs threads**

- **Processus lourd** :
    - Fil de contrôle et ressources dédiées (mémoire, fichiers).
    - Protection entre processus (espaces mémoire séparés).
- **Threads (processus léger)** :
    - Fil de contrôle simple, partage l’espace mémoire avec d’autres threads.
    - Pas de protection entre threads d’un même processus.

---

#### **6. Interface POSIX**

- **Processus lourds** :
    - `fork` : duplication de processus.
    - `exec` : exécution d’un nouveau programme.
    - `wait` : attente de fin d’un processus fils.
    - `exit` : terminaison d’un processus.
- **Threads** :
    - `pthread_create` : création.
    - `pthread_join` : attente d’un thread spécifique.
    - `pthread_exit` : terminaison.

---

#### **7. Ordonnancement des processus/threads**

- **Rôle** :
    - Sélectionner le processus activable à exécuter.
- **Critères** :
    - **Équité** : partage équitable du temps processeur.
    - **Efficacité** : maximisation de l’utilisation du processeur (réduction des commutations de contexte).

---

#### **8. Préemption**

- **Types d’ordonnancement** :
    - **Préemptif** : processus peut perdre le processeur involontairement.
    - **Non préemptif** : processus conserve le processeur jusqu’à sa libération volontaire.
- **Mécanismes** :
    - Partage de temps : quantum de temps alloué à chaque processus.
    - Priorités : processus le plus prioritaire actif.

---

#### **9. Stratégies d’ordonnancement**

- **Round-robin** :
    - Partage de temps en FIFO.
    - Assure l’équité mais peut être inefficace pour des processus variés.
- **Priorités** :
    - Statique : priorité fixée.
    - Dynamique : recalculée selon des critères (charge CPU, délai d’exécution).
- **Linux** :
    - Temps réel : `SCHED_FIFO`, `SCHED_RR`.
    - Généraliste : `SCHED_NORMAL` (CFS), `SCHED_BATCH`, `SCHED_IDLE`.

---
