È un protocollo applicativo orientato a sistemi iper-mediali (non limitato ad essi), ovvero per lo scambio di dati tipizzati. È un protocollo senza stato e generale, supporta tipizzazione e la negoziazione della rappresentazione dei dati.

Alla base c'è il paradigma *client-server* la comunicazione avviene per mezzo di un protocollo di trasporto affidabile --> [[4. Livello trasporto#TCP|TCP]]

### Schema di comunicazione

- Il server è un programma che accetta connessione e attende richieste HTTP: con il termine ==**Orign Server**== si indica il programma che è in grado di generare la risposta autoritativa per una determinata risorsa.
- Il client è un programma che stabilisce una connessione verso un server con l'obiettivo di inviare una o più richieste HTTP, anche chiamato ==**User Agent**==

L'obiettivo della richiesta HTTP è chiamato **Risorsa**. HTTP non si interessa della tipologia di risorsa, si limita ad offrire una interfaccia per l'interazione. Ogni risorsa è identificata da un ==URI== (standard).

Le risorse sono eterogenee, ma c'è bisogno di avere un'astrazione per la loro gestione. Il server si occupa di fornire una (o anche più) rappresentazioni della stessa risorsa.

Essendoci sotto il TCP è sempre presente il [[4. Livello trasporto#TCP#Three way Handshake|Three way Handshake]]

![[Screenshot 2024-02-14 alle 17.33.09.png]]

> Alcune volte il protocollo HTTP implementa la logica di *Request-Response*, il serve può mandare dati solo se il client prima fa una richiesta.

## Composizione dei messaggi

Tutti i messaggi sono composti da una sezione di ==**Intestazione**== e da un ==**Payload**==. È un protocollo testuale, quindi può essere letto da un utente umano.

### Intestazione

È organizzata per linee:
1. La prima indica l'operazione desiderata (richiesta) o il risultato ottenuto (Risposta)
2. Successivamente sono riportati campi aggiuntivi detti **header**
3. una linea vuota separa la sezione di intestazione dal payload del messaggio

![[Screenshot 2024-02-14 alle 17.38.26.png]]

### Richiesta

![[Screenshot 2024-02-14 alle 17.41.17.png]]

1. Il messaggio di richiesta inizia con una **request-line**:
	- contiene **metodo**, **URI** della risorsa, e la **versione** del protocollo
2. Seguono un numero variabile di campi **header**, uno per linea:
	- modificatori, informazioni del client, metadati relativi alla rappresentazione, ...
5. Segue una linea vuota che indica la fine dell'intestazione della richiesta, se è presente sarà riportato il **contenuto** della richiesta.

### Risposta


