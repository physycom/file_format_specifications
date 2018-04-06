# Table of Contents
1. [Formato dati accelerometrici](#formato-dati-accelerometrici)
2. [Formato dati giroscopici](#formato-dati-giroscopici)
3. [Formato dati inerziali](#formato-dati-inerziali)
4. [Formato dati inerziali georeferenziati](#formato-dati-inerziali-georeferenziati)
5. [Formato dati di geoposizionamento](#formato-dati-di-geoposizionamento)

CHANGELOG
=========

|  **Data**     | **Revisione**   | **Autore**                | **Note**                                                                          |
|  ------------ | --------------- | ------------------------- | --------------------------------------------------------------------------------- |
|  13/10/2015   | 1               | A. Fabbri, S. Sinigardi   | Definizione standard per acc e gyro                                               |
|  27/10/2015   | 2               | A. Fabbri                 | Definizione standard per dati GNSS come CSV                                       |
|  10/11/2015   | 3               | S. Sinigardi              | Upgrade standard dati GNSS al formato JSON                                        |
|  10/03/2016   | 4               | S. Sinigardi              | Definizione standard dati inerziali                                               |
|  22/03/2016   | 5               | A. Fabbri                 | Upgrade standard dati inerziali: aggiunta del campo speed, timestamp relativo     |
|  19/07/2016   | 6               | A. Fabbri, S. Sinigardi   | Definizione standard dati inerziali georeferenziati                               |
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

Esempio
-------
```
#  timestamp(s)  #  acc_x (g)  #  acc_y (g)  #  acc_z (g)  #
  518716448.047      -0.048         0.015         0.993
  518716448.057      -0.044         0.045         1.003
  518716448.067      -0.033         0.036         1.015
```

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

Esempio
-------
```
#  timestamp(s)  #  w_x (dps)  #  w_y (dps)  #  w_z (dps)  #
  518716448.047       0.567        0.567         1.217
  518716448.057       0.067        0.267         1.300
  518716448.067      -0.117        0.500         1.517
```

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

Esempio
-------
<!--
```
#    timestamp    #  speed #     ax   #    ay   #    az   #    gx   #    gy   #    gz   #   |a|   #  timestamp_rel #
  518716448.0567    5.9580   -0.04400   0.04500   1.00300   0.06667	  0.26667   1.30000   1.00497       0.0099
  518716448.0666    5.9580   -0.03300   0.03600   1.01500   0.11667	  0.50000   1.51667   1.01617       0.0198
  518716448.0766    5.9580   -0.03500   0.00400   1.00800   0.00000	  0.73333   1.55000   1.00862       0.0298
  518716448.0865    5.9580   -0.04600   0.00400   0.99300   0.31667	  0.63333   1.26667   0.99407       0.0397
```
-->
```
# timestamp # speed #  ax   #   ay  #   az  #   gx  #   gy  #   gz  #  |a|  #  timestamp_rel #
  16448.05    5.958   0.044   0.045   1.003   0.066   0.266   1.300   1.004       0.009
  16448.06    5.958   0.033   0.036   1.015   0.116   0.500   1.516   1.016       0.019
  16448.07    5.958   0.035   0.004   1.008   0.000   0.733   1.550   1.008       0.029
  16448.08    5.958   0.046   0.004   0.993   0.316   0.633   1.266   0.994       0.039
```


Dettaglio
---------

| Offset   | Nome              | Tipo    | Dimensioni        | Note                 |
| -------- | ----------------- | ------- | ----------------- | -------------------- |
| 0        | Timestamp         | Float   | Secondi           | Dal 1/1/2000 UTC+1   |
| 1        | Velocità          | Float   | Kilometri / Ore   |                      |
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

Esempio
-------
<!--
```
#    timestamp   #    lat   #   lon    #    alt   #  heading  #  speed  #     ax   #   ay    #    az   #    gx    #    gy   #    gz   #   |a|  # timestamp_rel  #
  518716448.0567   44.49972   11.35368   45.86800   294.00000   5.95800    0.04400   0.04500   1.00300    0.06667   0.26667   1.30000   1.00497       0.0099
  518716448.0666   44.49972   11.35368   45.86800   294.00000   5.95800	  -0.03300   0.03600   1.01500   -0.11667   0.50000   1.51667   1.01617       0.0198
  518716448.0766   44.49972   11.35368   45.86800   294.00000   5.95800   -0.03500   0.00400   1.00800    0.00000   0.73333   1.55000   1.00862       0.0298
```
-->
```
#  timestamp  # lat  # lon  # alt  # head # speed # ax  # ay  # az  # gx  # gy  # gz # |a| # t_rel #
  448.05   44.4   11.3   45.8    12      5     0.4   0.4   1.0   0.6   0.2   1.3  1.0   0.00
  448.06   44.4   11.3   45.8    13      5     0.3   0.3   1.0   0.1   0.5   1.5  1.0   0.01
  448.07   44.4   11.3   45.8    18      5     0.3   0.0   1.0   0.0   0.7   1.5  1.0   0.02
```


Dettaglio
---------
| Offset   | Nome              | Tipo    | Dimensioni        | Note                                      |
| -------- | ----------------- | ------- | ----------------- | ----------------------------------------- |
| 0        | Timestamp         | Float   | Secondi           | Dal 1/1/2000 UTC+1                        |
| 1        | Latitudine        | Float   | Gradi             | Da -90 a +90                              |
| 2        | Longitudine       | Float   | Gradi             | Da -180 a 180                             |
| 3        | Altitudine        | Float   | Metri             | Sul livello del mare, riferimento WGS     |
| 4        | Orientamento      | Float   | Gradi             | Misurati dal nord magnetico, da 0 a 360   |
| 5        | Velocità          | Float   | Kilometri / Ore   |                                           |
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

Esempio
-------
```
[
    {
        "alt":45.0,
        "date":"8/6/2016 16:54:08.10",
        "fix":1,
        "heading":294.0,
        "lat":44.4997189,
        "lon":11.3536798,
        "speed":4.9176,
        "timestamp":518716448.1001034
    }
]
```

Dettaglio
---------

| Nome                            | Priorità              | Chiave          | Tipo     | Dimensioni      | Note                                                                                |
| ------------------------------- | --------------------- | --------------- | -------- | --------------- | ----------------------------------------------------------------------------------- |
| Altitudine                      | Opzionale             | alt             | Float    | Metri           | Sul livello del mare, riferimento WGS                                               |
| Latitudine                      | Obbligatorio          | lat             | Float    | Gradi           | Da -90 a +90                                                                        |
| Longitudine                     | Obbligatorio          | lon             | Float    | Gradi           | Da -180 a +180                                                                      |
| Timestamp                       | Obbligatorio          | timestamp       | Float    | Secondi         | Dal 1/1/2000 UTC +1                                                                 |
| Velocità                        | Obbligatorio          | speed           | Float    | Kilometri / Ore | json incapsulato contenente velocità minima, massima e media dal record precedente  |
| Direzione                       | Opzionale             | heading         | Float    | Gradi           | Dal nord magnetico in senso orario, valori da 0 a 360                               |
| Data e ora                      | Opzionale             | date            | Testo    |                 | UTC +1                                                                              |
| Qualità FIX                     | Opzionale             | fix             | Intero   |                 | Da 0 a 3, secondo standard API modulo GNSS                                          |
| IMEI                            | Opzionale             | imei            | Testo    |                 | Identificativo OBU                                                                  |
| Distanza dal record precendente | Opzionale             | delta\_dist     | Float    | Metri           | Calcolata dal record precedente tenendo conto della correzione geodetica            |
| Causa                           | Opzionale             | enabling        | Testo    |                 | Identificativo di presenza del record                                               |
| Indice globale di acquisizione  | Opzionale             | global\_ index  | Intero   |                 | Contatore incrementale indice di acquisizione                                       |

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
