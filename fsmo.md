AD DS si basa sul concetto di multi-master per copiare informazioni tra controller di dominio e risolvere automaticamente eventuali conflitti.  
Non tutti i ruoli però supportano questo modello e alcuni ruoli, definiti FSMO (*Flexible Single Master Operation*), delegano a un solo controller il ruolo di master.  
Questi ruoli sono:
- Schema Master
- Domain-naming Master
- Infrastructure Master
- RID (Relative ID) Master
- PDC (Primary Domain Controller) emulator Master

Per impostazione predefinita, il primo controller di dominio configurato ricopre questi 5 ruoli che possono essere poi passati ad altri controller.  

Ogni foresta ha:
- uno Schema Master
- un Domain-naming Master (controller da contattare per aggiungere/rimuovere domini o effettuare modifiche al nome del dminio).  

Ogni dominio ha:
- un RID Master (si occupa di allocare a ogni dominio un set di ID per la generazione di SID univoci (Security ID) da assegnare agli oggetti ed evitare che lo stesso SID esista per più oggetti)
- un Infrastructure Master (mantiene traccia delle informazioni degli oggetti/principals/SID tra domini e risolve i SID ai nomi dei security principals
- un PDC Master (mantiene sincronizzata l'ora e fornisce un servizio "NTP" all'interno del Dominio. Inoltre le GPO sono scritte di default al server con ruolo PDC Emulator così come è il primo controller a ricevere informazioni per cambi urgenti di password. Se un utente cambia la propria password, il controller di dominio prima di autenticare l'utente contatta il PDC Emulator per verificare se ci sono state modifiche alle credenziali.).  


