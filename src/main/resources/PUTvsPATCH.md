# PUT vs PATCH

Nel protocollo HTTP, i metodi PUT e PATCH sono entrambi utilizzati per *modificare* risorse su un server, ma differiscono
nel modo in cui vengono impiegati per apportare queste modifiche.

## Metodo PUT

`PUT` : aggiornamento completo di una risorsa esistente

Il payload della richiesta PUT contiene l'intera rappresentazione della risorsa e il server la sostituirà con i dati forniti.

## Metodo PATCH

`PATCH` : modifiche parziali a una risorsa esistente

Il payload della richiesta PATCH contiene solo le informazioni necessarie per aggiornare o modificare specifiche
parti della risorsa. Utile per piccole modifiche o aggiornamenti, senza sostituire completamente lo stato attuale.