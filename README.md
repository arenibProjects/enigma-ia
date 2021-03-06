# enigma-ia

Une librairie réutilisable d'IA, conçue pour tourner sur Raspberry PI.

## **Contenu de la lib** :
### **protocol.hpp**
#### Présentation:
C'est une classe 'abstraite' qui est sensée servir de template aux protocoles de votre robot. Ce protocole sera communiqué à l'IA qui se chargera de l'executer
#### Fonctions membres abstraites:
- **void nextMove(IA \*ia)** : Est exécutée tant que le protocole est en cours d'execution, et que les actionneurs ont terminés leurs actions.<br>
- **void reset()** : Dois remettre à l'état initial le protocole. Nécessaire pour le mécanisme de reset de l'IA.<br>
- **bool isCompleted()** : Doit indiquer quand le protocole est terminé<br>
- **unsigned short int getPriority(IA \*ia)** : Doit indiquer le niveau de priorité du protocole. Cette priorité doit varier au fur et à mesure des actions de l'IA (par exemple priorité nulle d'un protocole *ChargerPince* quand il y a déja un cube de chargé). On précise que quelques macros pour les priorités sont disponibles: PRIORITY_HIGHEST,
PRIORITY_VERY_HIGH,
PRIORITY_HIGH,
PRIORITY_MEDIUM,
PRIORITY_LOW,
PRIORITY_VERY_LOW et
PRIORITY_NULL.
### **ia.hpp**
#### Présentation:
Un gestionnaire de protocole, le cœur de notre système d'IA. À partir du moment ou il est lancé, il exécute étape par étape les protocoles qui sont marqués comme les plus intéressants à lancer, en suivant le système de priorité. Il propose aussi un système de 'flag' pour gagner du temps dans la création des protocoles: il s'agit de petites variables accessibles entre les protocoles, sans pour autant les rendre globales. On peux par exemple imaginer un 'flag' *isCubeLoaded* pour savoir si un cube a déja été chargé.
#### Constructeurs:
- **IA(Protocol \*protocols[], short unsigned int protocolCount)** : Crée une IA en lui fournissant un pointeur vers sa future liste de protocol, et sa taille.
- **IA()** : Crée une IA avec les paramètres par défaut (nombre max de protocole stocké dans la macro MAX_PROTOCOL_NUMBER)
#### Fonctions membres:
- **addProtocol(Protocol \*protocol)** : Rajoute un protocole dans les liste des protocoles à gérer<br>
- **setFlag(String flagName, unsigned char value)** : Crée, le cas échéant, un flag et lui assigne une valeur. Le nombre max de protocole est stocké dans la macro MAX_FLAG_NUMBER<br>
- **short int getFlag(String flagName)** : Donne en sortie la valeur du flag spécifié sous la forme d'un unsigned char, ou -1 si le flag n'existe pas.<br>
- **void start()** : Lance l'ia dans son thread propre. Elle commence à lancer les protocoles les uns après les autres. Il est à noter que cette fonction n'est pas bloquante. <br>
- **void pause()** : Met en pause l'IA. Elle pourra reprendra plus tard là ou elle s'est arrétée.<br>
- **void reset()** : Reset les flags, et arrête l'ia. Elle recommencera au début si elle est relancée.<br>
- **bool isRunning()** : A pour valeur ``true`` si le thread de l'IA est en train de tourner. Sinon, a pour valeur ``false``.
- **bool hasRan()** : A pour valeur ``false`` si l'IA est à son état initial. Sinon, a pour valeur ``true``.
## À faire:
- [X] Refactor l'ancien code
- [ ] Modifier le code pour qu'il puisse tourner en standalone sur son propre thread.
- [ ] Créer un utilitaire pour gérer les flux Serial. Il doit pouvoir être utilisé de la même manière coté logique est coté actionneurs. Au programme, envoi de commande, attente de commandes, pouvoir paramétrer une réponse automatique à la réception de certaines commandes.
- [ ] Intégrer l'utilitaire Serial dans l'IA.
