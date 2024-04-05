AD DS sfrutta un modello di replica multi-master, ovvero più Controller di Dominio hanno poteri di scrittura e possono quindi apportare modifiche al database e replicarle negli altri Controller.  
Per best-practice è consigliato installare almeno due Controller di Dominio per ogni sito in modo da garantire availability in caso di disservizio di uno dei due controller.  

## Cestino Active Directory
Si tratta di una funzione che permette di mantnere in un cestino dedicato gli oggetti eliminati per un periodo di tempo predeterminato, per impostazione predefinita 180 giorni.  
Il ripristino di oggetti dal cestino è immediato e non comporta downtime del sistema operativo.

## Ripristino del database
Al fine di ripristinare correttamente AD DS è necessario effettuare backup che includono dati relativi al *System State*, ovvero una collezione di file critici del sistema operativo e del server che includono il database AD DS e il registro.  
Per ripristinare il database occorre riavviare il controller in modalità DSRM e utilizzare la password specificata a inizio configurazione di Active Directory.  
Se sono presenti altri controller di dominio, il controller appena ripristinato richiederà la replica delle eventuali modifiche apportate durante il periodo di downtime.  

**Ripristino non autoritativo**  
Il controller ripristinato recupera le modifiche apportate durante il periodo di downtime e quindi rende impossibile il ripristino di un oggetto eliminato.  

**Ripristino autoritativo**  
Prima di ricollegare il controller di dominio alla rete/dominio, gli oggetti ripristinati che devono essere mantenuti sono segnalati.  
Una volta ricollegato alla rete, le modifiche sono replicate come prima con eccezione per gli oggetti specificati le cui modifiche seguono il percorso inverso, ovvero dal controller appena ripristinato verso gli altri controller di dominio.  

## Global catalog
Il database condiviso in ogni controller all'interno di un dato dominio contiene tutte le informazioni sugli oggetti del dominio stesso.  
Una parte di queste informazioni è trasmesso nel Global Catalog che serve come indice per trovare oggetti in caso di foresta multi-dominio.  
Il ruolo di Global Catalog Server assegnato a un controller di dominio rende quest'ultimo il nodo all'interno di un dominio a cui fare riferimento per risolvere query per oggetti in altri domini.  
