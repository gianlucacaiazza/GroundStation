# GroundStation

Sorgente Android della *Stazione a Terra* del progetto di Ingegneria del Software 2019/2020 dell'Università Ca' Foscari di Venezia.

> Compatibile con Android 4.0 (Kitkat) e successivi (API level 19).

## Utilizzo:

All'avvio l'applicazione partirà in modalità **Advertisement**, per forzare la disconnessione e portare l'app in stato di **Discovery** premere il tasto indietro sul dispositivo.
Per agevolare lo sviluppo il flag `debug` nel file *MainActivity* è settato a **true**.

La stazione a terra si occuperà di gestire la connessione tra tutti gli agenti attraverso la Strategia **P2P_STAR**; questa strategia prevede l'invio dei messaggi alla stazione che procederà a reinviare a tutti i dispositivi connessi il messaggio ricevuto (broadcast).


Per maggiori informazioni riguardo la sintassi del protocollo consultare il documento "*Specifica Progetto*" disponibile nel portale Moodle.

## Debug - Da GroundStation a Robot:

Il nome di default del nodo può essere settato attraverso la variabile `mName` nella funzione `onCreate` del file *MainActivity*. E' possibile, attraverso questa variabile, modificare il nome; nel codice è disponbile una funzione `generateRandomName` che crea a **run-time** un numero di 5 cifre random (vedi codice commentato).

 Per procedere alla trasformazione di una GroundStation in un Robot è necessario:

1. Inserire un nome alternativo in `mName`
2. Opzionale: modificare lo stato di avvio nella funzione `onStart` da **ADVERTISING** a **DISCOVERING**
3. Commentare `send(payload)` (l'invio automatico dei messaggi ricevuti) nella funzione `onReceive` .

`Il nome associato alla variabile mName (endpoint) è visualizzato nell'angolo destro della barra superiore`


> **NOTA:** il comportamento di default implementato prevede la connessione automatica a qualsiasi nodo con lo stesso `SERVICE_ID` all'interno di *MainActivity*. Per evitare collegamenti non voluti in **fase di sviluppo** procedete con la modifica di questa variabile nel codice in modo che la vostra stazione a terra ed il vostro/i robot abbiano lo stesso valore.


## Funzioni disponibili:

In questo paragrafo entreremo nel dettaglio dei pulsanti disponibili all'interno dell'applicazione.


#### Secret Key value:

A schermo viene visualizzata la chiave di cifratura attualmente in uso dall'applicazione. Il valore di default è `abcdefgh`, per modificarlo, utilizzare il tasto **EDIT** disponibile a schermo, inserire nel pop-up la nuova chiave e premere il tasto **DONE** per aggiornare il valore.

> **NOTA:** come detto nelle specifiche, le chiavi devono essere di **8** caratteri. Nessun check sulla chiave è eseguito in fase d'inserimento, le eccezioni a run-time però restituiscono dei messaggi a schermo.

Dal momento che la chiave di default verrà riapplicata ad ogni riavvio dell'applicazione, è possibile modificarne il valore di default attraverso la variabile `KEY` nel file *MainActivity*.

#### Advertise:

Questo tasto permette di avviare la funzione di Advertisement senza dover riavviare l'applicazione. Prima di procedere con le altre funzionalità dell'applicazione attendere di vedere a schermo la scritta **CONNECTED**.


#### Test All Msg:

Questo tasto farà partire la funzione `send_Byte` del file *MainActivity*.

Verranno inviati, in sequenza, tutte le possibili varianti di messaggi disponibili nel protocollo del progetto. Nel dettaglio i messaggi di test che verranno inviati ai dipositivi sono i seguenti:


- Protocollo passivo: `Coordinate recupero:3;6;`
- Test encryptato: `Operazione in corso:4;8;<TIMESTAMP>;`**\****
- Test benvenuto: `Benvenuto sono Pippo`
- Stop globale: `0STOP`
- Stop singolo (Agente 1): `1STOP`


> **NOTA:** nel codice della stazione a terra non è necessario distinguere i messaggi di STOP, il codice per distinguerli è disponibile commentato nella funzione `onReceive`. Nell'esempio fornito si suppone di essere il robot 1, questo valore va parametrizzato nella vostra soluzione!
>
> **NOTA\**** **:** il `<TIMESTAMP>` nel messaggio di test dell'applicazione è un valore numerico (vedi il codice della funzione per maggiori dettagli).


#### Coodinate:

Questo tasto farà partire la routine di invio delle coordinate fornite all'interno del file `test.csv` nella directory *assets*.
Nel dettaglio, ogni 4 secondi verranno inviate le coordinate della mina successiva fino al termine del file. Ad ogni invio ed al termine del thread verranno visualizzati dei messaggi a schermo sulla stazione a terra.

#### Start/Stop {All,1,2}:

Questi tasti fanno partire rispettivamente i messaggi di Start/Stop per tutti i robot (All), per il robot con ID 1 e per il robot con ID 2.
