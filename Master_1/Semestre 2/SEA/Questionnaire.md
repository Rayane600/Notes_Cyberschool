## Mécanisme d'appel système

### Mode utilisateur 
- fichiers impliqués : `syscall.h `/ `sys.s`
- Fonctions utilisées : `ecall`: numéro d'appel système stocke dans a7 , les paramètres dans a0-1-2-3
### Mode noyau
- Fichiers impliqués : `exception.cc` , `thread.h`
- Fonctions utilisées : `Start` , `join`, `Yield`, `Sleep`, `Finish`, `Exec`

## Gestion de threads et de processus

### Ce qui doit être sauvegardé 
Le contexte du thread et le contexte du simulateur .

### Variables utilisée
- La variable `g_scheduler->readyList` contient la liste des threads prêts.
- Le thread actif ne fait pas partie de cette liste, il est référencé par `g_current_thread`.

### Variable g_alive

- `g_alive` est la liste des threads vivants.
- `readyList` est la liste des threads prêts à l'execution.

### Gestion allocution mémoire 
- Les routines de gestion de listes ne gèrent pas l'allocation/désallocation des objets chaînés. Elles manipulent uniquement les pointeurs vers ces objets.
- c'est le programmeur qui s'occupe de ce travail pour éviter des fuites mémoires et des dés-allocations prématurés 

### Placement d’un objet thread bloqué sur un sémaphore 

 - Le thread est placé dans la file d'attente du sémaphore

### Empêcher les interruptions lors de la manipulation des structures de données
- Les interruptions sont désactivées en utilisant `g_machine->interrupt->SetStatus(INTERRUPTS_OFF)`

### Utilité de la méthode `SwitchTo`
- `SwitchTo` effectue le changement de contexte entre deux threads.
- `SaveSimulatorState` et `RestoreSimulatorState` sauvegardent et restaurent l'état du simulateur.
- `SaveProcessorState` et `RestoreProcessorState` sauvegardent et restaurent l'état du processeur RISC-V.

### Utilité du champ type 
- Vérifier que les appels système sont appliqués sur le bon type d'objet
## Environnement de développement 

### Outils offerts par Nachos : 
- Routines de débogage (`DEBUG`,`ASSERT`) et autres flags.

### GDB pour débogage de code NACHOS

- Oui GDB peut être utilisé pour déboger Natcho 

###  GDB pour débogage de programme utilisateur
- Oui il suffit de compiler grace au compilateur croisé 

