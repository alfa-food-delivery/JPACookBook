# Application Servers
Server HTTP che consente di eseguire applicazioni JAVA.  
Posso utilizzare diverse API, applicando implementazioni di specifiche JEE, tra cui:
- JAX-WS (servizi SOAP)
- JAX-RS (servizi REST)
- CDI (Contexts and Dependency Injection)
- JPA (Java Persistence API)


Ambiente software, fornisce un'infrastruttura completa per lo sviluppo, il deployment e l'esecuzione di applicazioni.  
Supporta lo sviluppo di applicazioni web, enterprise e aziendali, offrendo un insieme di servizi e funzionalità.  

### Apache TomEE
Un esempio di application server Java EE, basato su Apache Tomcat, è Apache TomEE.  
Garantisce di fornire le implementazioni delle API. Ha diverse caratteristiche.  
Combina la semplicità di Tomcat con la potenza delle specifiche Java EE, fornendo un ambiente di sviluppo e deployment per applicazioni Java Enterprise.  
Supporta le specifiche Java EE e offre funzionalità come EJB, JPA, CDI, JAX-RS, JMS e altro ancora, permettendo agli sviluppatori di creare e gestire applicazioni enterprise.

#### Supporto a varie tecnologie
Supporta diverse tecnologie e specifiche, come servlet, JSP (JavaServer Pages), Java EE (Enterprise Edition), JPA (Java Persistence API), EJB (Enterprise JavaBeans), JMS (Java Message Service), JTA (Java Transaction API), CDI (Contexts and Dependency Injection), JAX-RS (Java API for RESTful Web Services), e molte altre.

#### Gestione delle transazioni
Offre un supporto robusto per la gestione delle transazioni. Eseguo operazioni atomiche e coerenti nel contesto di transazioni distribuite.

#### Gestione delle risorse
Gli application server gestiscono le risorse di sistema e forniscono pool di connessioni per database, connessioni JMS e altre risorse, ottimizzando l'uso e la disponibilità delle risorse.

#### Sicurezza
Fornisce meccanismi di sicurezza integrati per autenticazione, autorizzazione e crittografia, garantendo un ambiente sicuro per le applicazioni.

#### Gestione del ciclo di vita delle applicazioni
Deployment, manutenzione e aggiornamento applicazioni in modo controllato e gestito.

#### Scalabilità e bilanciamento del carico
Supporta la scalabilità orizzontale e verticale delle applicazioni per gestire il carico elevato e distribuire le richieste tra più risorse.

#### Monitoraggio e gestione
Fornisce strumenti di monitoraggio prestazioni, analisi log, rilevamento errori, gestione risorse.

#### Interoperabilità e integrazione
Consente l'integrazione con altri sistemi e servizi attraverso standard come SOAP, REST, JMS e altri protocolli.

#### Supporto per lo sviluppo
Può essere integrato con strumenti e plug-in per lo sviluppo e il testing di applicazioni, oltre a un ambiente di sviluppo integrato (IDE) per semplificare il processo di sviluppo.


### Architettura a Microservizi
Evoluzione delle architetture layered, separando il ciclo di vita ad ogni singolo servizio

### SOA - Service Oriented Architecture
Architettura software adatta al supporto di servizi web per garantire l'interoperabilità tra diversi sistemi.

Consente l'uso di singole applicazioni come componenti del processo di business.

Soddisfa le richieste degli utenti in modo integrato e trasparente.

Posso fare *API Economy* :  
Fabric (fintech) , AWS , Google Cloud , Informazioni Meteo , Calorie Cibo , Ingredienti dei cocktail

Per chiamare servizi web di terze parti devo conoscere **endpoints**, **metodi**, **oggetti** da passare, eventualmente anche la **chiave privata**.

## Servizi Web
Funzionalità business in rete:
- utilizzabili da chiunque
- rappresentano API

Ci sono due tipi principali di servizi web:  
- SOAP - Simple Object Access Protocol
- REST - Representational State Transfert

### SOAP - Simple Object Access Protocol
Protocollo per lo scambio di messaggi tra componenti software.  
Principali caratteristiche : 
0. Modello B2B o B2C , Servlet , JAX-WS
1. Trasporto Neutro  
HTTP , SMTP (Simple Mail Trasnfert Protocol),FTP (File Transfert Protocol)
Metodi:
- GET , POST , PUT , PATCH , DELETE
2. Stateless  
Ogni richiesta ad un servizio è indipendente dalla successiva, non mantenendo nessuno stato interno. Questo consente una maggiore scalabilità.
3. Rappresentazione oggetti XML  
Le risposte e le richieste del client vengono effettuate tramite oggetti descritti in un file di formato .xml
- Markup, tag dichiarativi
- Human Readable, con dati in chiaro
- Limitato : necessario un parsing corretto, quindi vincola molto lo sviluppo lato client


Come posso comunicare con dei servizi SOAP?
Mi serve il WSDL , descrittore del servizio.  
Generiamo dunque gli **stub** con cui richiamiamo il servizio SOAP.

### REST - REpresentational State Transfert
Sistema di trasmissione di dati basato sul protocollo HTTP.  


Principali caratteristiche:
0. Modello B2C , JAX-RS
Si basa sul modello richieste-risorse per semplificare la comunicazione client-server:
- **Risorsa** : //...
- **Risposta** : Stato , informazione ( tipi, testo, pdf, . . . )
1. Trasferimento HTTP
2. Stateless
3. Rappresentazione in diversi formati
XML , JSON(JavaScript Object Notation)
4. **ENDPOINT UNIVOCO** = url(ip) + porta + nomeRisorsa
5. Creazione e sfruttamento della *CACHE* sul Client

Processo di creazione di una RESTfull API
1) Individuare ENDPOINT (url : server + porta + nomeRisorsa )
2) Definizione Dati (request e response )
3) Descrizione dei metodi (create, get, getById, update, replace, delete)
4) Definizione versione, sicurezza, ...
5) Modelli di dati che sono passati devono essere descritti accuratamente
   tipi di campo, campi obbligatori, enumerations , ...

endpoint -> Path  
tipo metodo -> @GET , @POST , ...  
queryparam -> annotation @QueryParam o oggetto passato

??? **Wordnik** : (progetto open di Swagger) ha portato alla definizione di uno standard internazionale per i servizi REST.

#### REST - OpenAPI
Specification language per descrivere e definire in maniera standard le proprie API (analogo al WSDL in soap).

1. Principio Design First  
Prima scrivo il documento, poi sviluppo il servizio.  
Analogo al TDD (Test Driven Development), definisco *specifiche funzionali* e *standard di definizione* come rotte, oggetti da passare, ecc.
2. Cambiamenti Controllati  
Tutti i cambiamenti sono documentati.
3. Versioning Semplice  
Bisogna cercare sempre di garantire la retrocompatibilità.  
Se devo cambiare un campo, ne aggiungo uno nuovo a parte. Non elimino quello vecchio.

Due linguaggi:  
- `JSON` : markup  
- `YAML` : annotato, non ho i marcatori ma descrivo gli attributi in modo posizionale (il markup è la posizione )

Sezioni fondamentali di un file descrittivo per generare documentazione OpenAPI:
- info  
- generiche  
- servers  
- url  
anche più di uno in diversi ambienti (sviluppo, test , production )
- paths  
contiene tutti gli endpoint associati alle API
- components  
descrizione dei modelli (stati) trasferiti tra client e server
- security
- tag  
raggruppamento in base ai tags in base a