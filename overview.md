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

# Utenti, gruppi e computer

**Utenti**  
Active Directory permette la creazione di diversi tipi di account e utenti fornendo un diverso grado di informazioni, privilegi e scopi.  

Gli account *MSA*, managed service account, sono account che non possono effettuare un login interattivo e il cui scopo è fornire ai servizi credenziali di autenticazione al sistema. Per questi tipi di account AD DS crea automaticamente password complesse, rinnovate ogni 30 giorni.  
In caso si renda necessario utilizzare lo stesso MSA su più server all'interno del dominio è necessario configurare il gruppo MSA previa l'attivazione del servizio di distribuzione di chiavi di Microsoft, *MKDS* (`kdssvc.dll`).  
Questo servizio permette di recuperare l'ultima chiave identificatica di un account di Active Directory, nel caso del gruppo MSA permette di calcolare la password dell'account di servizio a partire da questa chiave ottenuta dal controller di dominio.  

**Gruppi**  
I gruppi in Active Directory si suddividono in **Sicurezza** e **Distribuzione**.  
**Sicurezza**  
I gruppo di sicurezza servono per assegnare e gestire i permessi alle risorse.  
**Distribuzione**  
I gruppi di distribuzione sono tipicamente utilizzati da applicazioni di posta elettronica.  

Per ogni gruppo è necessario specificare lo scope, ovvero **Locale**, **Locale al dominio**, **Globale** e **Universale**.  
**Locale**  
Un gruppo locale è disponibile solo nel computer o server di dominio (che non sia il controller di dominio) ma i cui membri possono essere del dominio/foresta stessa.  
**Locale al dominio**  
Un gruppo locale al dominio è creato nei controller di dominio ed è valido solo all'interno del dominio stesso ma i membri possono essere qualsiasi oggetto all'interno della foresta.  
**Globale**  
Un gruppo globale serve per raggruppare utenti che hanno caratteristiche simili, ad esempio per unire utenti provenienti dallo stesso dipartimento o dalla stessa area geografica. I membri sono solo oggetti locali al dominio e possono avere permessi sulle risorse nell'intera foresta.  
**Universale**  
Un gruppo universale unisce le caratteristiche dei gruppi locali al dominio e dei gruppi globali.  

**Computer**  
Un oggetto computer è considerato un *Security Principal* ([Entità di sicurezza](https://learn.microsoft.com/it-it/windows-server/identity/ad-ds/manage/understand-security-principals)) come un utente. Nel momento in cui un computer è aggiunto al dominio viene creato un oggetto nel contenitore *Computer*. Tale contenitore è predefinito in AD DS ma non + equiparabile a un OU, ad esempio non è possiible associare delle GPO. Per best practices è consigliato creare una o più OU ad hoc.  

# Foreste e domini

**Foreste**  
Una foresta contiene uno o più alberi di dominio AD DS. I domini in una foresta condividono una root comune (primo dominio creato nella foresta), uno schema comune e un global catalog.

<img src="https://learn.microsoft.com/en-us/training/wwl-windows-server/introduction-to-ad-ds/media/m6-forest-bc4595bc.png">

Nel Forest Root Domain esistono i seguenti oggetti:
- Schema Master Role
- Domain Naming Master Role
- Gruppo Enterprise Admin
- Gruppo Schema Admins

Inizialmente assegnati al primo controller di dominio creato, questi oggetti possono essere assegnati ad controller nella foresta.  

Utenti esterni alla foresta non possono accedere a risorse interne alla foresta. Tutti i domini all'interno della stessa foresta condividono un legame *di fiducia* che semplifica l'accesso alle risorse.  
Le repliche delle configurazioni e delle partizioni del AD DS databse non possono uscire dai confini della foresta.  

In ogni dominio della foresta esisono i seguenti oggetti:
- RID Master Role
- Infrastructure Master Role
- PDC Emulator Master Role
- Gruppo Domain Admins

**Domini**  
Un dominio è un container logico amministrativo di oggetti come utenti, gruppi, computer e altri oggetti.  
Il database AD DS memorizza tutti gli oggetti del dominio e ogni controller di dominio memorizza una copia del database.  

<img src="https://learn.microsoft.com/en-us/training/wwl-windows-server/introduction-to-ad-ds/media/m6-domain-755853b7.png">  

Quando sono apportate modifiche agli oggetti nel dominio, il controller di dominio replica le modifiche in tutti gli altri controller di dominio. Se esistono più domini in una foresta, solo una parte delle modifiche è replicata negli altri domini. AD DS utilizza un modello di replica *multi-master* in cui ogni controller ha il potere di effettuare modifiche agli oggetti.  
Il dominio AD DS contiene un account **Administrator** e un gruppo **Domain Admins**. Questo account è membro del gruppo Domain Admins che è a sua volta membro del gruppo Administrators per ogni computer membro al dominio. Inoltre, sempre per default, i membri al gruppo Domain Admins **hanno pieni poteri su ogni oggetto nel dominio**.  

**Relazioni di fiducia**  
In foreste multi-dominio i legami di fiducia (trust) tra i domini sono fondamentali per garantire accesso alle risorse condivise presenti nei singli domini.  
I legami di fiducia che si creano automaticamente nella foresta hanno tutti proprietà transitive, ovvero se il dominio A ritiene che il dominio B sia fidato e a sua volta il dominio B ritiene che il dominio C sia affidabile, allora anche il dominio A ritiene il dominio C fidato.  
