È la versione 6 del protocollo IP, ha un nuovo formato di pacchetto (IPv6 e IPv4 non sono compatibili). La differenza principale (motivo per cui è stato inventato) è che ha un grande spazio di indirizzamento rispetto a IPv4:
- Questo comporta un header modificato che però permette varie funzionalità tra cui la **gestione del roaming**, sicurezza integrata (**IPsec**) ed è stata rivista semplificata e ottimizzata per il multicast.
## Indirizzamento

![[Screenshot 2024-02-16 alle 18.23.36.png]]

Spazio di indirizzamento a 128 bit, rappresentati in esadecimale, diviso in gruppi da 2byte ciascuno. Esiste una notazione compressa:
- si tolgono gli 0 più a sinistra
- togliere la sequenza di 0 più lunga, sostituendola con `::`

![[Screenshot 2024-02-16 alle 18.24.08.png]]
## Scope

Tutti gli indirizzi hanno uno scope che specifica in quale parte della rete è valido, si divide in:
- globale
- locale
- interface

![[Screenshot 2024-02-16 alle 18.25.38.png]]

- **Loopback** --> `::1/128` usato per accedere ai servizi che stano funzionando sull'host stesso
- **Global Unicast Address** --> tutti gli indirizzi che iniziano con `2000::/3`. Denotano tutti gli indirizzi pubblici che possono essere assegnati a delle interfacce di un host, hanno lo stesso scopo degli ip pubblici in IPv4
- **Link-Local Address** --> tutti gli indirizzi `fe80::/10` utilizzati per le operazioni di *Neighbor Discovery protocol* e altri protocolli tipo DHCPv6. IPv6 richiede che sia assegnato un indirizzo link-local su ogni interfaccia di rete, anche se è già assegnato un indirizzo globale

## Subnetting

Gli ultimi (meno significativi) 64 bit sono fissi e sono detti **Interface ID**, mentre i primi 64 bit specificano il Network Prefix.
- Il prefix è delegato da IANA e dagli ISP

# Header

![[Screenshot 2024-02-16 alle 18.37.15.png]]

La dimensione dell'header IPv6 è fissa a `40byte` per aumentare la velocità di parsing:
- Tolto il campo fragment
- tolto il checksum --> perchè sia il livello2 che il livello 4 lo fanno già

I campi che compongono l'header sono:
- **Version** --> 6
- **Flow** --> serve per indicare il traffico, specifica che un pacchetto fa parte di un flusso di pacchetti
- **Payload Lenght** --> dimensione del payload che segue
- **Next header** --> specifica il tipo del prossimo header (se c'è) o quello del livello superiore 
- **Hop Limit** -->numero massimo di Hop che un pacchetto può attraversare
- **Indirizzi**

I campi opzionali del IPv6 vengono accodati come payload del pacchetto, proprio per questo l'ordine è importante.
![[Screenshot 2024-02-16 alle 18.43.57.png]]

I router non fanno frammentazione, lo fanno gli host. Se al router arriva un pacchetto troppo grande per il link, manda un errore ICMP6. 

# Neighbor Discovery Protocol

Non esiste ARP. Il NDP utilizza ICMP (e il multicast), spostanco così il lavoro dal livello 2 al livello 3, rendendo il protocollo indipendente dal mezzo trasmissivo. Questo oltretutto viene fatto anche in maniera autenticata, grazie al layer di sicurezza.

Questo protocollo rende possibile scoprire i router, i parametri dei link, qual'è il prefisso a me assegnato e permette il tracciamento del cambiamento di indirizzi. 

Questo protocollo introduce nuovi pacchetti ICMP:
- **Neighbor sollecitation** --> sapendo l'ip di destinazione prende i 24 bit meno significativi e genera il *sollicitated-node-address* accodandolo direttamente al prefisso `ff02::1:ff00:0/104`
![[Screenshot 2024-02-16 alle 18.55.22.png]]

- **Neighbor Advertisment** --> serve per rispondere a una sollecitation
- Redirect --> ugualmente a Ipv4
- **Router Sollecitation** --> serve per sapere quali sono gli indirizzi globali disponibili per l'assegnamento
- **Router advertisment** --> invia i dati per la configurazione automatica di IPv6

## Configurazione Globale

1. Creare un link-local Address --> l'interfaceID viene creato in maniera casuale o derivato dal mac address, i primi 64 bit sono sempre `fe80::`
2. Controllo se ci sono duplicati nella rete locale --> l'indirizzo generato automaticamente è un *candidato*, mando una neighbor sollecitation a l'indirizzo appena generato, se non ricevo risposte allora diventa fisso
3. Ricerca del router --> l'interface ID rimane quello Link-Local, si manda una router sollecitation e trovo i prefissi e la subnet delegati dal mio ISP. Ho una router ADV che mi manda: l'indirizzo del router, 0 o più link-prefix, infomrazioni su come generare l'interfaceID
4. Creo un Global Unicast Address: attraverso il dhcp opppure attraverso SLACC --> configurazione automatica, o staticamente
