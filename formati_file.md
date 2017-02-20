CHANGELOG
=========

|  **Data**     | **Revisione**   | **Autore**                | **Note**                                                                          |
|  ------------ | --------------- | ------------------------- | --------------------------------------------------------------------------------- |
|  13/10/2015   | 1               | A. Fabbri, S. Sinigardi   | Definizione standard per acc e gyro                                               |
|  27/10/2015   | 2               | A. Fabbri                 | Definizione standard per dati GNSS come CSV                                       |
|  10/11/2015   | 3               | S. Sinigardi              | Upgrade standard dati GNSS al formato JSON                                        |
|  10/3/2016    | 4               | S. Sinigardi              | Definizione standard dati inerziali                                               |
|  22/3/2016    | 5               | A. Fabbri                 | Upgrade standard dati inerziali: aggiunta del campo speed, timestamp relativo     |
|  19/7/2016    | 6               | A. Fabbri, S. Sinigardi   | Definizione standard dati inerziali georeferenziati                               |
|  13/10/2016   | 7               | A. Fabbri, S. Sinigardi   | Upgrade standard dati inerziali georeferenziati: aggiunta modalità interpolated   |

Formato dati accelerometrici
============================

Formato di standardizzazione interno che associa ogni record di tipo
accelerometrico ad un preciso timestamp del modulo GNSS.

Descrizione
-----------

File di testo ASCII di tipo CSV (separatore \\tab) a 4 colonne. Le
accelerazioni sono riportate in millesimi di **g = 9.81 m/s\^2**
assumendo un **sistema di riferimento inerziale** in cui cioè
accelerazioni con segno positivo corrispondono ad accelerazioni impresse
nel verso positivo del relativo asse. L'offset di gravità, tipicamente
visibile sull'asse z, è riportato in accordo con il modello della molla
sospesa. Ad esempio, orientando l'asse z verso l'alto si vedrà un offset
di circa 1 g positivo nelle accelerazioni in tale asse, in accordo con
quanto riportato sopra tali valori aumenteranno(diminuiranno) in
corrispondenza di accelerazioni verso l'alto(il basso). Il timestamp è
espresso come numero di **secondi a partire dal 1/1/2000 UTC +1** (fuso
di Roma).

Dettaglio
---------

| Offset   | Nome              | Tipo    | Dimensioni   | Note                 |
| -------- | ----------------- | ------- | ------------ | -------------------- |
| 0        | Timestamp         | Float   | Secondi      | Dal 1/1/2000 UTC+1   |
| 1        | Accelerazione X   | Float   | Unità di g   | Inerziale            |
| 2        | Accelerazione Y   | Float   | Unità di g   | Inerziale            |
| 3        | Accelerazione Z   | Float   | Unità di g   | Inerziale            |

Output
------

Generato come output dai seguenti programmi:

-   [log\_parser](https://github.com/physycom/log_parser)

Input
-----

Richiesto come input dai seguenti programmi:

-   [data\_tools](https://github.com/physycom/data_tools)

Commenti
--------

Lo standard non specifica l'orientazione tra gli assi del dispositivo
rispetto al suo involucro plastico e qualsiasi riferimento fisico
rilevante per il dataset.

Formato dati giroscopici
========================

Formato di standardizzazione interno che
associa ogni record di tipo giroscopico ad un preciso timestamp del
modulo GNSS.

Descrizione
-----------

File di testo ASCII di tipo CSV (separatore \\tab) a 4 colonne. Velocità
angolari sono riportate in **gradi sessagesimali al secondo** assumendo
una convenzione **destrorsa** per l'attribuzione dei segni delle
velocità angolari. Il timestamp è espresso come numero di **secondi a
partire dal 1/1/2000 UTC +1** (fuso di Roma).

Dettaglio
---------

| Offset   | Nome             | Tipo    | Dimensioni        | Note                 |
| -------- | ---------------- | ------- | ----------------- | -------------------- |
| 0        | Timestamp        | Float   | Secondi           | Dal 1/1/2000 UTC+1   |
| 1        | Vel Angolare X   | Float   | Gradi / Secondi   | Destrorso            |
| 2        | Vel Angolare Y   | Float   | Gradi / Secondi   | Destrorso            |
| 3        | Vel Angolare Z   | Float   | Gradi / Secondi   | Destrorso            |

Output
------

Generato come output dai seguenti programmi:

-   [log\_parser](https://github.com/physycom/log_parser)

Input
-----

Richiesto come input dai seguenti programmi:

-   [data\_tools](https://github.com/physycom/data_tools)

Commenti
--------

Lo standard non specifica l'orientazione tra gli assi del dispositivo
rispetto al suo involucro plastico e qualsiasi riferimento fisico
rilevante per il dataset.

Formato dati inerziali
======================

Formato di standardizzazione interno dei dati inerziali per un sistema
fisico assimilabile a corpo rigido.

Descrizione
-----------

File di testo ASCII di tipo CSV (separatore \\tab) a 10 colonne. Per i
dati inerziali (accelerometro e giroscopio) valgono le convenzioni e i
commenti stabliti nei paragrafi precendenti. Le velocità sono quelle
fornite dal modulo GNSS e espresse in **m/s**.

Le diverse frequenze di acquisizione tra i vari tipi di sensori sono
state trattate mediante due schemi di riempimento:

-   ***Modalità unmodified :*** ad ogni riga vengono aggiornati soltanto
    i valori che uno dei sensori aggiorna, gli altri vengono
    semplicemente ripetuti.

-   ***Modalità interpolated :*** ogni riga contiene valori sempre
    diversi che vengono interpolati in maniera lineare per rendere più
    smooth l'andamento dei dati stessi.

Dettaglio
---------

| Offset   | Nome              | Tipo    | Dimensioni        | Note                 |
| -------- | ----------------- | ------- | ----------------- | -------------------- |
| 0        | Timestamp         | Float   | Secondi           | Dal 1/1/2000 UTC+1   |
| 1        | Velocità          | Float   | Metri / Secondi   |                      |
| 2        | Accelerazione X   | Float   | Unità di g        | Inerziale            |
| 3        | Accelerazione Y   | Float   | Unità di g        | Inerziale            |
| 4        | Accelerazione Z   | Float   | Unità di g        | Inerziale            |
| 5        | Vel Angolare X    | Float   | Gradi / Secondi   | Destrorso            |
| 6        | Vel Angolare Y    | Float   | Gradi / Secondi   | Destrorso            |
| 7        | Vel Angolare Z    | Float   | Gradi / Secondi   | Destrorso            |
| 8        | Modulo Acc        | Float   | Unità di g        |                      |
| 9        | Tempo relativo    | Float   | Secondi           | Dal primo record     |

Output
------

Generato come output dai seguenti programmi:

-   [log\_parser](https://github.com/physycom/log_parser)

Input
-----

Richiesto come input dai seguenti programmi:

-   [data\_tools](https://github.com/physycom/data_tools)
-   [inertial\_tools](https://github.com/physycom/inertial_tools)

Commenti
--------

Lo standard non specifica l'orientazione tra gli assi del dispositivo
rispetto al suo involucro plastico e qualsiasi riferimento fisico
rilevante per il dataset.

Formato dati inerziali georeferenziati
======================================

Formato di standardizzazione interno dei dati inerziali georeferenziati
per un sistema fisico assimilabile a corpo rigido.

Descrizione
-----------

File di testo ASCII di tipo CSV (separatore \\tab) a 14 colonne. Per i
dati inerziali (accelerometro e giroscopio) e di georeferenziazione
valgono le convenzioni e i commenti stabliti nei paragrafi precendenti.

Le diverse frequenze di acquisizione tra i vari tipi di sensori sono
state trattate mediante due schemi di riempimento:

-   ***Modalità unmodified :*** ad ogni riga vengono aggiornati soltanto
    i valori che uno dei sensori aggiorna, gli altri vengono
    semplicemente ripetuti.

-   ***Modalità interpolated :*** ogni riga contiene valori sempre
    diversi che vengono interpolati in maniera lineare per rendere più
    smooth l'andamento dei dati stessi.

Dettaglio
---------

| Offset   | Nome              | Tipo    | Dimensioni        | Note                                      |
| -------- | ----------------- | ------- | ----------------- | ----------------------------------------- |
| 0        | Timestamp         | Float   | Secondi           | Dal 1/1/2000 UTC+1                        |
| 1        | Latitudine        | Float   | Gradi             | Da -90 a +90                              |
| 2        | Longitudine       | Float   | Gradi             | Da -180 a 180                             |
| 3        | Altitudine        | Float   | Metri             | Sul livello del mare, riferimento WGS     |
| 4        | Orientamento      | Float   | Gradi             | Misurati dal nord magnetico, da 0 a 360   |
| 5        | Velocità          | Float   | Metri / Secondi   |                                           |
| 6        | Accelerazione X   | Float   | Unità di g        | Inerziale                                 |
| 7        | Accelerazione Y   | Float   | Unità di g        | Inerziale                                 |
| 8        | Accelerazione Z   | Float   | Unità di g        | Inerziale                                 |
| 9        | Vel Angolare X    | Float   | Gradi / Secondi   | Destrorso                                 |
| 10       | Vel Angolare Y    | Float   | Gradi / Secondi   | Destrorso                                 |
| 11       | Vel Angolare Z    | Float   | Gradi / Secondi   | Destrorso                                 |
| 12       | Modulo Acc        | Float   | Unità di g        |                                           |
| 13       | Tempo relativo    | Float   | Secondi           | Dal primo record                          |

Output
------

Generato come output dai seguenti programmi:

-   [log\_parser](https://github.com/physycom/log_parser)

Input
-----

Richiesto come input dai seguenti programmi:

-   [data\_tools](https://github.com/physycom/data_tools)
-   [inertial\_tools](https://github.com/physycom/inertial_tools)
-   [vbb\_meta](https://github.com/physycom/vbb_meta)
-   [vbb\_texa](https://github.com/physycom/vbb_texa)
-   [vbb\_datastore](https://github.com/physycom/vbb_datastore)

Commenti
--------

Lo standard non specifica l'orientazione tra gli assi del dispositivo
rispetto al suo involucro plastico e qualsiasi riferimento fisico
rilevante per il dataset.

Formato dati di geoposizionamento
=================================

Formato di standardizzazione interno dei dati di geoposizionamento.

Descrizione
-----------

File di arhiviazione testuale di tipo [JSON](http://www.json.org/).
Due tipologie di formati:

-   **JSON array :** il livello più esterno dell'archivio è costituito
    da un array i cui elementi sono oggetti di tipo JSON descritti come
    segue.

-   **JSON object :** il livello più esterno dell'archivio è costituito
    da un JSON i cui campi, identificati dalla chiave
    gnss\_record\_*, sono a loro volta JSON descritti come segue.

I campi sono suddivisi in due tipologie: obbligatori e opzionali. I
campi obbligatori sono tali in prospettiva di analisi o visualizzazione
dei dati stessi. I campi opzionali arricchiscono tipicamente la
visualizzazione fornendo informazioni aggiuntive.

Dettaglio
---------

| Nome                            | Priorità              | Chiave          | Tipo     | Dimensioni      | Note                                                                                |
| ------------------------------- | --------------------- | --------------- | -------- | --------------- | ----------------------------------------------------------------------------------- |
| Altitudine                      | Opzionale             | alt             | Float    | Metri           | Sul livello del mare, riferimento WGS                                               |
| Latitudine                      | Obbligatorio          | lat             | Float    | Gradi           | Da -90 a +90                                                                        |
| Longitudine                     | Obbligatorio          | lon             | Float    | Gradi           | Da -180 a +180                                                                      |
| Timestamp                       | Obbligatorio          | timestamp       | Float    | Secondi         | Dal 1/1/2000 UTC +1                                                                 |
| Velocità                        | Obbligatorio          | speed           | Float    | Metri/Secondo   | json incapsulato contenente velocità minima, massima e media dal record precedente  |
| Direzione                       | Opzionale             | heading         | Float    | Gradi           | Dal nord magnetico in senso orario, valori da 0 a 360                               |
| Data e ora                      | Opzionale             | date            | Testo    |                 | UTC +1                                                                              |
| Qualità FIX                     | Opzionale             | fix             | Intero   |                 | Da 0 a 3, secondo standard API modulo GNSS                                          |
| IMEI                            | Opzionale             | imei            | Testo    |                 | Identificativo OBU                                                                  |
| Distanza dal record precendente | Opzionale             | delta\_dist     | Float    | Metri           | Calcolata dal record precedente tenendo conto della correzione geodetica            |
| Causa                           | Opzionale             | enabling        | Testo    |                 | Identificativo di presenza del record                                               |
| Indice globale di acquisizione  | Opzionale             | global\_index   | Intero   |                 | Contatore incrementale indice di acquisizione                                       |

Output
------

Generato come output dai seguenti programmi:

-   [list\_to\_json](https://github.com/physycom/list_to_json)
-   [kml\_to\_json](https://github.com/physycom/kml_to_json)
-   [nmea\_to\_json](https://github.com/physycom/nmea_to_json)
-   [ubx\_to\_json](https://github.com/physycom/ubx_to_json)
-   [json\_undersampler](https://github.com/physycom/json_undersampler)
-   [json\_packer](https://github.com/physycom/json_packer)
-   [log\_parser](https://github.com/physycom/log_parser)

Input
-----

Richiesto come input dai seguenti programmi:

-   [json\_to\_html](https://github.com/physycom/json_to_html)
-   [json\_to\_geojson](https://github.com/physycom/json_to_geojson)
-   [multi\_json\_map](https://github.com/physycom/multi_json_map)
-   [json\_distance](https://github.com/physycom/json_distance)
-   [distance\_stats](https://github.com/physycom/distance_stats)
-   [trip\_data\_uploader](https://github.com/physycom/trip_data_uploader)

Commenti
--------

La gestione dello standard JSON è stata demandata alla libreria open
source [jsoncons](https://github.com/danielaparker/jsoncons).