# VS Code - Guida Pratica alle Regex

## Come usare le regex in VS Code

### 1. Ricerca Semplice

**Passo 1**: Apri la ricerca
- Premi `Ctrl+F` (Windows/Linux) o `Cmd+F` (Mac)

**Passo 2**: Attiva la modalità regex
- Clicca sull'icona `.*` nella barra di ricerca
- Oppure premi `Alt+R`

**Passo 3**: Inserisci il tuo pattern
- Esempio: `\d+` per trovare tutti i numeri

---

## 2. Ricerca e Sostituzione

### Esempio 1: Invertire nome e cognome

**Situazione iniziale**:
```
Mario Rossi
Laura Bianchi
Luca Verdi
```

**Obiettivo**:
```
Rossi, Mario
Bianchi, Laura
Verdi, Luca
```

**Procedura**:
1. Premi `Ctrl+H` (Windows/Linux) o `Cmd+Option+F` (Mac)
2. Attiva regex con `.*`
3. **Cerca**: `(\w+)\s+(\w+)`
4. **Sostituisci con**: `$2, $1`
5. Clicca su "Replace All" o `Ctrl+Alt+Enter`

**Spiegazione**:
- `(\w+)` → cattura il primo nome (gruppo 1)
- `\s+` → uno o più spazi
- `(\w+)` → cattura il cognome (gruppo 2)
- `$2, $1` → usa gruppo 2, virgola, gruppo 1

---

### Esempio 2: Formattare numeri di telefono

**Prima**:
```
Telefono: 0781234567
Cellulare: 0791234567
```

**Dopo**:
```
Telefono: 078 123 45 67
Cellulare: 079 123 45 67
```

**Procedura**:
- **Cerca**: `0(\d{2})(\d{3})(\d{2})(\d{2})`
- **Sostituisci**: `0$1 $2 $3 $4`

---

### Esempio 3: Aggiungere punto e virgola alla fine delle righe

**Prima**:
```javascript
let nome = "Mario"
let cognome = "Rossi"
let eta = 25
```

**Dopo**:
```javascript
let nome = "Mario";
let cognome = "Rossi";
let eta = 25;
```

**Procedura**:
- **Cerca**: `^(.+)$`
- **Sostituisci**: `$1;`
- Assicurati che la modalità multiline sia attiva

---

### Esempio 4: Rimuovere linee vuote

**Cerca**:
```regex
^\s*$\n
```

**Sostituisci con**: (lascia vuoto)

Questo rimuove tutte le righe vuote o che contengono solo spazi.

---

### Esempio 5: Convertire da snake_case a camelCase

**Prima**:
```python
nome_completo = "Mario Rossi"
data_nascita = "10/11/2000"
codice_fiscale = "ABC123"
```

**Dopo**:
```javascript
nomeCompleto = "Mario Rossi"
dataNascita = "10/11/2000"
codiceFiscale = "ABC123"
```

**Procedura**:
- **Cerca**: `_([a-z])`
- **Sostituisci**: (segui con "Replace with Uppercase" o usa script)

Soluzione alternativa con JavaScript:
```regex
// Cerca: _(\w)
// Sostituisci: in JavaScript, usa una funzione
```

---

## 3. Ricerca Multi-File

### Esempio: Trovare tutti i TODO

**Passo 1**: Apri ricerca globale
- Premi `Ctrl+Shift+F`

**Passo 2**: Attiva regex
- Clicca su `.*`

**Passo 3**: Pattern di ricerca
```regex
(//|#)\s*TODO:.*
```

Questo trova:
- `// TODO: fix this`
- `# TODO: implement`

---

### Esempio: Trovare funzioni JavaScript

**Pattern**:
```regex
function\s+(\w+)\s*\([^)]*\)\s*\{
```

Trova:
- `function calcola() {`
- `function somma(a, b) {`

---

## 4. Casi d'uso pratici

### 4.1 Pulizia codice

#### Rimuovere console.log
**Cerca**:
```regex
console\.log\(.*\);?\n?
```

**Sostituisci**: (vuoto)

---

#### Rimuovere commenti di una riga
**JavaScript/C++**:
```regex
//.*$
```

**Python**:
```regex
#.*$
```

---

### 4.2 Refactoring

#### Rinominare variabile con cautela
**Cerca**:
```regex
\bold_name\b
```

**Sostituisci**:
```
new_name
```

Il `\b` assicura che si trovino solo parole intere, non parti di parole.

---

### 4.3 Formattazione

#### Aggiungere virgola dopo ogni elemento
**Prima**:
```javascript
const arr = [
  'item1'
  'item2'
  'item3'
]
```

**Cerca**: `'([^']+)'$`  
**Sostituisci**: `'$1',`

---

### 4.4 Trovare pattern specifici

#### Email in tutto il progetto
```regex
[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
```

#### URL
```regex
https?://[^\s]+
```

#### Numeri di telefono svizzeri
```regex
0\d{2}\s?\d{3}\s?\d{2}\s?\d{2}
```

#### Indirizzi IP
```regex
\b\d{1,3}\.\d{1,3}\.\d{1,3}\.\d{1,3}\b
```

---

## 5. Esempi avanzati

### 5.1 Convertire array Python in JavaScript

**Prima (Python)**:
```python
frutti = ['mela', 'banana', 'arancia']
numeri = [1, 2, 3, 4, 5]
```

**Dopo (JavaScript)**:
```javascript
const frutti = ['mela', 'banana', 'arancia'];
const numeri = [1, 2, 3, 4, 5];
```

**Cerca**: `(\w+)\s*=\s*(\[.*\])`  
**Sostituisci**: `const $1 = $2;`

---

### 5.2 Aggiungere type hints Python

**Prima**:
```python
def somma(a, b):
    return a + b
```

**Dopo**:
```python
def somma(a: int, b: int) -> int:
    return a + b
```

Per questo è meglio farlo manualmente o con uno script, ma il pattern di ricerca sarebbe:
```regex
def\s+(\w+)\s*\(([^)]+)\):
```

---

### 5.3 Formattare import Python

**Prima**:
```python
import os
import sys
import re
```

**Dopo**:
```python
import os, sys, re
```

**Cerca**: `^import\s+(\w+)\n(?:import\s+(\w+)\n)*`

---

## 6. Trucchi e consigli

### 6.1 Case-insensitive search
Aggiungi il flag `i` alla fine del pattern (non supportato direttamente in VS Code)

**Alternativa**: usa `[Aa]` invece di `a`
```regex
[Hh]ello [Ww]orld
```

---

### 6.2 Multiline mode
Per far sì che `^` e `$` corrispondano a inizio/fine riga invece che inizio/fine stringa.

In VS Code, questo è automatico per le ricerche multi-riga.

---

### 6.3 Testare prima di sostituire
1. Usa prima solo la ricerca (`Ctrl+F`)
2. Verifica che trovi esattamente ciò che vuoi
3. Poi passa a ricerca e sostituzione (`Ctrl+H`)

---

### 6.4 Usare gruppi non catturanti
Se hai bisogno di raggruppare ma non catturare:
```regex
(?:pattern)
```

Esempio:
```regex
(?:http|https)://.*
```

---

## 7. Esercizi pratici in VS Code

### Esercizio 1: Formatta lista
Converti questa lista:
```
Mario Rossi, 25
Laura Bianchi, 30
Luca Verdi, 28
```

In formato JSON:
```json
{"nome": "Mario Rossi", "eta": 25}
{"nome": "Laura Bianchi", "eta": 30}
{"nome": "Luca Verdi", "eta": 28}
```

<details>
<summary>Soluzione</summary>

**Cerca**: `([^,]+),\s*(\d+)`  
**Sostituisci**: `{"nome": "$1", "eta": $2}`
</details>

---

### Esercizio 2: Aggiungi commenti
Aggiungi `//` all'inizio di ogni riga:

**Cerca**: `^`  
**Sostituisci**: `// `

---

### Esercizio 3: Estrai numeri da testo
Da: `Prezzo: 25.50 CHF`  
A: `25.50`

**Cerca**: `.*?(\d+\.\d+).*`  
**Sostituisci**: `$1`

---

## 8. Configurazione VS Code

### Impostazioni utili

Apri `settings.json` (`Ctrl+,` poi cerca "regex"):

```json
{
  "search.smartCase": true,  // Case-insensitive se tutto minuscolo
  "search.globalFindClipboard": false,
  "editor.find.seedSearchStringFromSelection": "always"
}
```

---

## 9. Shortcuts utili

| Shortcut | Azione |
|----------|--------|
| `Ctrl+F` | Ricerca |
| `Ctrl+H` | Sostituisci |
| `Ctrl+Shift+F` | Ricerca globale |
| `Alt+R` | Toggle regex |
| `Alt+C` | Toggle case-sensitive |
| `Alt+W` | Toggle whole word |
| `F3` | Prossimo risultato |
| `Shift+F3` | Risultato precedente |
| `Ctrl+Alt+Enter` | Sostituisci tutto |

---

## 10. Limiti e alternative

### Quando NON usare regex in VS Code:

1. **Refactoring complessi**: Usa invece:
   - `F2` per rinominare simboli
   - Estensioni di refactoring

2. **Parsing strutturato**: Per JSON/XML usa parser dedicati

3. **Sostituzioni su centinaia di file**: Considera script Python/JavaScript

---

## Risorse aggiuntive

- [VS Code Search Tips](https://code.visualstudio.com/docs/editor/codebasics#_search-and-replace)
- [Regex101](https://regex101.com/) - Per testare pattern complessi
- [RegExr](https://regexr.com/) - Visual regex builder

---

Buon lavoro con le regex in VS Code! 🚀
