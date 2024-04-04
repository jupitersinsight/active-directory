# Componenti logici
**Partizione**  
Una partizione, o naming context, è una porzione del database AD DS. Sebbene il database consista in un file, *Ntds.dit*, ogni partizione contiene un set diverso di dati.  
Active Directory memorizza copie delle partizioni nei controller di dominio presenti e le mantiene aggiornate attraverso la replica della directory.

**Schema**  
Uno schema è il set di definizione dei tipi di oggetto e attributi che gli amministratori usano del definire gli oggetti creati in AD DS.  

**Dominio**  
Un dominio è un contenitore logico per oggetti come utenti e computer. Un dominio è identificato da una specifica partizione (*Domain partition*).

**Albero di dominio**  
Un albero di dominio è una collezione di domini che condividono un dominio principale (*Domain root*) e uno spazio DNS contiguo.  

**Foresta**  
Una foresta è una collezione di uno o più domini che condividono un AD DS root, uno schema e un catalogo globale (global catalog).  

**OU**  
Una OU (*Organizational Unit*) è un contenitore di oggetti come utenri, gruppi e computer che fornisce un framework per delegare diritti amministrativi e regole attraverso i Criteri di gruppo (**Group Policy Objects**).  

**Container**  
Un container è un oggetto che fornisce un framework organizzativo in AD DS. Non posso essere associati a GPO.  

# Componenti fisici
**Controller di dominio**  
Un controller di dominio contiene una copia del database AD DS. Per la maggior parte delle operazioni, ogni controller di dominio è in grado di apportare modifiche al database così come replicarle verso gli altri controller.  

**Data store**  
Una copia del data store esiste su ogni controller di dominio. Le informazioni sull'archivio sono memorizzate nel file Ntds.dit. La cartella +C:\Windows\NTDS* è la cartella predefinita.  

**Global catalog server**  
Un server *global catalog* è un controller di dominio che ospita il global catalog. Questo è una copia partiale, di sola lettura, di tutti gli oggetti presenti in una foresta multi-dominio. Lo scopo è di velocizzare le ricerche degli oggetti all'interno dell'intera foresta.  

**Controller di dominio di sola lettura (RDOC)**  
Un RODC è una installazione speciale di AD DS. Queste installazioni sono comuni nei distacamenti (branch offices) dove la sicurezza fisica non è ottimale e il supporto IT non è ottimizzato.  

**Sito**  
Un sito è un contenitore di oggetti di AD DS come computer e servizi che sono specifici a un luogo fisico. Si contrappone al *Dominio* che identifica un insieme logico di oggetti.  

**Subnet**  
Una subnet è un segmento nella rete utilizzata dai computer. Ogni sito può avere più subnet.
