# JSON (JavaScript Object Notation)

Formato di testo per scambio dati.

Facile da leggere e scrivere per gli esseri umani.
Facile da generare e analizzare sintassi per le macchine.

Besato sul linguaggio di programmazione JavaScript (Standard ECMA-262 Terza Edizione - Dicembre 1999).

## Caratteristiche principali di JSON

- **Semplicità**: formato testuale semplice e intuitivo
- **Indipendenza Linguaggio**: basato su JavaScript ma indipendente dal linguaggio
- **Convenzioni Universali**: convenzioni comuni dei linguaggi basati su C ( C++, C#, Java, JavaScript, Perl, Python ... )
- **Due Strutture Dati**:
    - Insieme di coppie nome/valore, simile a oggetti, record, struct, dizionari, tabelle hash o array associativi
    - Elenco ordinato di valori, simile ad array, vettori, liste o sequenze

## Strutture JSON

- **Oggetto**: Una serie non ordinata di nomi/valori racchiusi tra parentesi graffe.
  Ogni nome è seguito da due punti e le coppie di nome/valore sono separate da virgole.

## Esempi JSON

```json
{
"nome": "Mario Rossi",
"eta": 30,
"indirizzo": {
"via": "Via Roma",
"citta": "Milano",
"CAP": "20100"
},
"email": "mario@email.com",
"telefoni": ["123456789", "987654321"],
"giorno": "2023-12-14T10:45:30"
}
```

```json
[
"Mela",
"Banana",
"Arancia",
"Pera",
"Kiwi"
]
```
