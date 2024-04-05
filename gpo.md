## A cosa servono i Group Policy Object
Le GPO servono per amministrare centralmente gli utenti e i computer nel dominio.  
Attraverso una GPO è possibile specificare configurazioni che si applicano ai computer (**Computer Configuration**) indipendentemente dagli utenti che effettuano il logon ed è possibile specificare configurazioni che si applicano ad utenti selezionati (**User Confgiuration**).  

Le regole sono applicate al momento dell'avvio del sistema operativo, all'accesso dell'utente, in background e ogni 90/120 minuti.

Esempi di gestione centralizzata sono:
- **applicazione di impostazioni di sicurezza**: ad esempio forzare l'utilizzo di criteri di complessità delle password, regole per il Firewall di Windows Defender
- **creazione di desktop e ambienti per le applicazioni omogenei**: creare una base uniforme per i desktop dei client e personalizzare/preparare l'ambiente di esecuzione nei client per applicazioni che supportano le GPO
- **distribuire software**: distribuzione massiva di programmi ai computer e agli utenti (a partire dal formato .msi)
- **reindirizzamento delle cartelle**: reindirizzamento delle cartelle personali in base alla sessione utente verso risorse centralizzate che ospitano i file richiesti
- **configurare impostazioni di rete**: ad esempio forzare la connessione solo verso determinate reti wi-fi

Le regole impostate nei **Modelli amministrativi** comportano modifiche a livello di **Registro di Sistema**:
- **Configurazione computer\Modelli amministrativi**: `HKEY_LOCAL_MACHINE`
- **Configurazione utente\Modelli amministrativi**: `HKEY_CURRENT_USER`

**! NEL CASO DI CONFLITTO LE REGOLE APPLICATE PER IL COMPUTER HANNO LA PRIORITA'**


## Applicazione delle GPO ed ereditarietà
Le GPO si possono applicare ai Siti, Domini e OU.  
Le regole della GPO si applicano quindi a cascata in tutti gli elementi figli a partire dal padre.  

È possibile applicare filtri alle GPO al fine di specificare nel dettaglio per quali computer, utenti e gruppi sono valide all'interno del sito, dominio (Security Filters) oltre a filtri WMI che restringono l'applicazione a requisiti necessari sui client (WMI Filters).

L'ordine di applicazione delle GPO è:
- GPO locali
- GPO collegate al sito
- GPO collegate al dominio
- GPO collegate all'OU
- GPO collegate all'OU figlio

Le GPO sono ereditate dagli oggetti figli ma l'ereditarietà si può bloccare manualmente (da usare raramente, meglio filtrare con i filtri sopra).

Ogni GPO contiene un valore che identifica la priorità. Più basso è il numero maggiore è la priorità e quindi le regole contenute nella GPO avranno un'importanza maggiore e sovrascriveranno le altre. 

Forzare una GPO significa che viene applicata in maniera forzosa anche quando l'ereditarietà è bloccata e ha la precedenza su tutte le altre GPO.  

## Policy predefinite  

Quando si installa AD DS per la prima volta, Windows crea due policy predefinite: **Default Domain Policy** e **Default Domain Controllers Policy**.  

**Default Domain Policy**  
Applicata al dominio e gli utenti autenticati. Questa GPO non ha filtri WMI ed è quindi applicata a tutti gli utenti e computer nel dominio. Questa GPO contiene regole per le password, account lockout e autenticione Kerberos.  

*Best practice*: non modificare questa GPO in quando contiene impostazioni critiche per il corretto funzionamento di AD DS.  

**Default Domain Controllers Policy**  
Applicata sollo all'Organizational Unit dei Controller di dominio. Non ci sono controindicazioni nel modificarla secondo esigenze anche se la creazione di GPO separate rimane comunque la strada migliore.  

Inoltre, Active Directory include delle GPO preconfigurate, chiamate Starter GPO, che hanno impostazioni in *Administrative Template* per i client già configurate secondo best practices di Microsoft.

## Come sono memorizzate le GPO  
Le GPO sono formate da due elementi:
- **Group Policy container**: si trova in Active Directory e memorizza i metadati relativi alla GPO, ovvero il timestamp di creazione, quanti computer e utenti ne sono influenzati e quali modifiche apporta, la versione della GPO e il suo GUID
- **Group Policy template**: questo template è una raccolta di file memorizzati nella cartella SYSVOL di ogni controller di dominio al percorso `%SystemRoot%\SYSVOL\Domain\Policies\GPOGUID`. Il template contiene le Group Policy Settings



## Comandi Powershell

Crea una nuova GPO
```
New-GPO
```

Collega una GPO a un sito, dominio od OU
```
New-GPLink
```

Ottiene informazioni sull'ereditarietà delle policies per un determinato dominio od OU
```
Get-GPInheritance
```

Blocca o sblocca l'ereditarietà per un determinato dominio od OU
```
Set-GPInheritance
```

Recupera una GPO o tutte le GPO del dominio
```
Get-GPO
```

## Troubleshooting
Il set di risorse **[RSoP](https://learn.microsoft.com/it-it/troubleshoot/windows-server/group-policy/use-resultant-set-of-policy-logging)** aiuta nel determinare quali regole sono applicate a un determinato utente o computer al fine di determinare eventuali problemi con le GPO. È anche in grado di anticipare quali regole saranno applicate simulando modifiche prima di applicarle.  

