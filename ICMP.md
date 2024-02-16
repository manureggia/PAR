È un protocollo di livello 3 di supporto a IP:
- i router o gateway della rete inviano dei messaggi ICMP per segnalare malfunzionamenti nella rete

Un pacchetto ICMP utilizza un header IP standard, come se fosse un livello superiore, ma è parte del protocollo IP stesso e quindi supportato da tutti i dispositivi di livello 3:
- Ciò nonostante non rende IP affidabile, ma cerca di fornire delle informazioni sullo stato della rete

Oltre a fornire feedback sullo stato delle connessioni, ICMP viene utilizzato per controllare lo stato della rete:
- alcune applicazioni di rete utilizzano attivamente ICMP per verificare la connettività della rete
- altre applicazioni utilizzano i messaggi ICMP per ottenere informazioni sulla reta in maniera indiretta (**traceroute**)

## Schema di un pacchetto ICMP

Utilizza un header IP standard, in cui il campo protocollo è messo a 1:
- indirizzi IP sorgente e destinazione identificano la comunicazione
- campo TTL per evitare che il pacchetto circoli all'infinito nella rete

>Può includere informazioni riguardanti altri livelli dello stack TCP/IP (es. port-unreachable).

La prima informazione fornita nel payload del pacchetto ICMP è il campo **type**:
- identifica il tipo di informazione fornita dal pacchetto e la struttura e logica del resto del pacchetto, infatti il pacchetto stesso cambia in base a questo campo

Alcuni esempi di messaggi possono essere:
- Echo Reply
- Destination Unreachable
- Redirect
- Echo
- Time exceeded

### Echo e echo replay (type = 0)

Se un host riceve un echo (request) deve rispondere con una echo replay. Pensato appositamente per testate attivamente la connettività di rete (usato da ping)

![[Screenshot 2024-02-16 alle 15.11.49.png]]

### Destination Unreachable (type = 3)

Pacchetto utilizzato per informare che è impossibile raggiungere una certa destinazione (non solo IP).
L'indirizzo IP di destinatzione utilizzato sarà l'indirzzo IP sorgente del pacchetto intereassato all'errore

![[Screenshot 2024-02-16 alle 15.18.30.png]]

