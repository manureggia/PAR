La posta elettronica è un sistema alquanto complesso che include diverse componenti:
- Mail Transfer Agent: il mail server per la gestione e il trasferimento della posta
- SMTP: protocollo per il trasferimento della posta
- DNS: resource record MX
- Mail User Agent: processo per la gestione della posta lato utente

Questo insieme di protocolli legati alla posta elettronica servono per gestire la comunicazione asincrona.
Siamo in un contesto decentralizzato, non c'è scambio di utenti tra i vari provider e chiunque può essere un provider Email.

![[Screenshot 2024-02-15 alle 16.43.46.png]]

Il mail user agent utilizza il protocollo SMTP per la consegna e la memorizzazione al **mail server** del destinatario un determinato messaggio:
- il provider riconosce cosa c'è dopo la `@` e deve capire dove mandarlo
- Non c'è un elenco di mail serve, il provider deve fare una richiesta DNS, utilizzando il campo **MX** per risolvere il nome del dominio di destinazione
Il destinatario scaricherà il messaggio dal mail server attraverso due protocolli principali:
- **POP**: per scaricare tutte le mail arrivate
- **IMAP**: per fare una sorta di sincronizzazione

![[Screenshot 2024-02-15 alle 16.46.54.png]]

### Interazione con il DNS

Il DNS del domiino del mail server gestisce un tipo di record MX che specifica come deve essere inoltrato un messaggio, comprende due informazioni:
- elenco di uno o più mail server (hostname, indirizzo) che sono abilitati a ricevere mail per quel dominio
- priorità relativa nel caso di più mail server

## Protocollo SMTP

Si chiama Simple Mail Transfer Protocol, è un protocollo tra:
- due mail server 
- un mail user agent e un mail server
Si utilizza anche qui un paradigma client-server. Usa il protocollo TCP per il trasferimento affidabile dei messaggi tra client e server.
Il dialogo sender-reciver avviene sulla **porta 25** ed è costituito da "frasi" in formato testuale comprensibili.

Esistono 3 fasi del trasferimento:
- Handshaking (SMTP greetings ≠ TCP handshaking)
- Trasferimento messaggi
- Chiusura

Il messaggio di mail consiste di due parti:
- un Header che contiene i campi codificati
- il body del messaggio che deve essere un testo ASCII a 8 bit

## Protocollo POP3

Post Office Protocol:
- fase di instaurazione della connessione --> TCP
- fase di autorizzazione --> user agent invia al server credenziali di login
- fase di transazione:
	- user agent recupera i messaggi
	- indicare quali messaggi eliminare
- Fase di aggiornamento

## Protocollo IMAP

Ha più funzionalità, ma è più complesso rispetto a POP3. Il server IMAP deve essere in grado di gestire una gerarchia di mailbox per ogni utente.

Questo protocollo permette all'utente di modificare la propria mailbox come se fosse locale e di ottenere alcune parti del messaggio (es attachment da scaricare).

Le fasi di una sessione IMAP sono:
- fase di instaurazione della connessione --> connessione TCP tra User agent e mail server
- Fase di autorizzazione --> il MUA invia al mail server le proprie credenziali
- Fase di transazione --> comprende comandi client, dati dal server, risultati del server

## Accesso tramite HTTP

Servizi di posta elettronica tramite tecnologie web permettono all'utente di modificare la mailbox come se fosse in locale (Tipo IMAP). In questi casi il MUA è il web browser. Questa è una soluzione più flessibile, ma più lenta

# Sicurezza

Nel protocollo orignale non c'è controllo di autenticità, quindi il mittente non è sicuro questo è detto **SPOOFING**.

Ogni volta che un email provider riceve una mail fa:
- tramite una query DNS sul campo ==txt== del provider del mittente --> in questo modo riceve un campo ==SPF== dove c'è un elenco di **IP certificati** a inviare le mail
- Nel caso non ci sia risposta perché il campo SPF non è configurato ci sono due approcci:
	- mettere la mail nello spam
	- scartare la mail
	
![[Screenshot 2024-02-15 alle 17.22.18.png]]