# Progetto Dati Culturali Emilia-Romagna

Questo repository contiene codice Python per interrogare l'endpoint SPARQL dei Dati Culturali dell'Emilia-Romagna e recuperare informazioni su Musei, Teatri Storici e Aree Archeologiche.

Il codice utilizza la libreria `pandas` per gestire i dati e la libreria `requests` per effettuare le chiamate all'endpoint SPARQL.

## Struttura del Notebook

Il notebook è organizzato in sezioni, ognuna dedicata al recupero di una specifica tipologia di dato culturale.

### 1. Recupero Dati: Musei dell'Emilia-Romagna

Questa sezione contiene il codice per interrogare l'endpoint SPARQL e ottenere informazioni dettagliate sui musei presenti nel dataset.

**Endpoint Utilizzato:** `https://dati.emilia-romagna.it/sparql`

**Query SPARQL:** La query recupera vari parametri per ciascun museo, tra cui:

*   URI del museo (`?museo`)
*   Nome (`?nome`)
*   Coordinate geografiche (`?latitudine`, `?longitudine`)
*   Ubicazione e nome del comune (`?ubicazione`, `?nome_comune`)
*   Descrizioni in italiano e inglese (`?descrizione_it`, `?descrizione_en`)
*   Link `owl:sameAs` a Beniculturali e Wikidata
*   Informazioni sull'ammissione (link, descrizione, gratuità, accessibilità)
*   Informazioni sulla sede (link, nome sede, indirizzo, codice postale)
*   Homepage (`?homepage`)
*   Scopo (`?purpose_link`, `?purpose_desc`)
*   Tipo (`?type_link`)
*   Aree incluse (`?includesArea_list`)
*   Servizi offerti (`?offersService_list`)
*   Orari di apertura (`?opening_hours_list`)

Vengono utilizzate funzioni di aggregazione come `MIN` e `GROUP_CONCAT` per gestire valori multipli per le stesse proprietà.

**Codice:** La cella Python effettua una richiesta POST all'endpoint SPARQL, richiedendo i risultati in formato CSV. I dati vengono poi letti in un DataFrame pandas. Viene gestita l'eccezione in caso di errori nella richiesta.

### 2. Salvataggio Risultato Musei

Questa breve sezione salva il DataFrame contenente i dati dei musei in un file CSV denominato `risultati_musei_emilia_romagna.csv`.

**Codice:** Utilizza il metodo `.to_csv()` del DataFrame pandas.

### 3. Recupero Dati: Teatri Storici

Questa sezione è dedicata al recupero di informazioni sui Teatri Storici presenti nel dataset.

**Endpoint Utilizzato:** `https://dati.emilia-romagna.it/sparql`

**Query SPARQL:** Simile alla query per i musei, questa query è specificamente filtrata per recuperare le entità che sono `culturalis:CulturalInstitutionOrSite` e che provengono dal dataset "Teatri storici", escludendo esplicitamente i castelli (`dbpedia:Castle`). La query recupera una serie di parametri analoghi a quelli dei musei.

**Codice:** La cella Python esegue una richiesta POST all'endpoint SPARQL per la query sui teatri storici e carica i risultati in un DataFrame pandas, gestendo eventuali errori.

### 4. Salvataggio Risultato Teatri Storici

Questa sezione salva il DataFrame contenente i dati dei teatri storici in un file CSV denominato `risultati_teatri_storici_emilia_romagna.csv`.

**Codice:** Utilizza il metodo `.to_csv()` del DataFrame pandas.

### 5. Recupero Dati: Aree Archeologiche

Questa sezione recupera dati sulle Aree Archeologiche dal dataset.

**Endpoint Utilizzato:** `https://dati.emilia-romagna.it/sparql`

**Query SPARQL:** Questa query si concentra sulle entità di tipo `arco:ArchaeologicalProperty`. Recupera informazioni come:

*   URI del sito (`?site`)
*   Etichetta/Nome (`?label`)
*   Nome del comune (`?cityName`)
*   Coordinate geografiche (`?lat`, `?lon`)
*   Descrizione (`?description`)
*   Risorsa indirizzo e etichetta indirizzo (`?addressResource`, `?addressLabel`)
*   Link `rdfs:seeAlso` (`?seeAlso_links`)
*   Date (`?dates`)
*   URI dei tipi specifici (`?dc_types_uris`) associati tramite `dc:type`
*   Indirizzi completi (`?full_addresses`)
*   Codici postali (`?postal_codes`)

Vengono utilizzate `OPTIONAL` per includere informazioni che potrebbero non essere presenti per tutti i siti e `GROUP_CONCAT` per aggregare valori multipli. La query include anche un `ORDER BY` per ordinare i risultati per etichetta.

**Codice:** La cella Python esegue la richiesta POST all'endpoint SPARQL per la query archeologica, carica i risultati in un DataFrame pandas e include la gestione degli errori.

### 6. Salvataggio Risultato Aree Archeologiche

Questa sezione salva il DataFrame contenente i dati delle aree archeologiche in un file CSV denominato `risultati_aree_archeologiche_emilia_romagna.csv`.

**Codice:** Utilizza il metodo `.to_csv()` del DataFrame pandas.

## Come Utilizzare il Codice

1.  Assicurati di avere installate le librerie `pandas` e `requests`.
2.  Esegui le celle di codice nel notebook.
3.  I risultati verranno salvati nei rispettivi file CSV nella stessa directory del notebook.

Questo codice fornisce un punto di partenza per esplorare e analizzare i dati culturali aperti forniti dalla Regione Emilia-Romagna tramite il loro endpoint SPARQL.
