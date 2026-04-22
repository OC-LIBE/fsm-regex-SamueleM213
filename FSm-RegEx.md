# Tutorial: FSM ed Espressioni Regolari

## Indice

1. [Introduzione alle Finite State Machines (FSM)](#1-finite-state-machines-fsm)
2. [Espressioni Regolari (Regex)](#2-espressioni-regolari-regex)
3. [Uso Pratico in VS Code](#3-uso-pratico-in-vs-code)
4. [Esempi in Python](#4-esempi-in-python)
5. [Esempi in JavaScript](#5-esempi-in-javascript)
6. [Esercizi Pratici](#6-esercizi-pratici)

---

## 1. Finite State Machines (FSM)

### 1.1 Cos'è una FSM?

Una **Finite State Machine** (Macchina a Stati Finiti) è un modello matematico di computazione che può trovarsi in uno di un numero finito di **stati** in un dato momento.

#### Definizione formale

Matematicamente, una FSM è definita come una **quintupla** (5-tupla):

**M = (Q, Σ, δ, q₀, F)**

dove:

- **Q**: insieme finito di **stati**  
 _Esempio_: Q = {q₀, q₁, q₂}

- **Σ** (sigma): **alfabeto** di input (insieme finito di simboli)  
  _Esempio_: Σ = {0, 1} per una FSM binaria

- **δ** (delta): **funzione di transizione**  
  - Per DFA: δ: Q × Σ → Q (da uno stato con un input si va in un solo stato)
  - Per NFA: δ: Q × Σ → P(Q) (può andare in più stati o nessuno)
  
  _Esempio_: δ(q₀, '1') = q₁ significa "dallo stato q₀, ricevendo input '1', vai in q₁"

- **q₀**: **stato iniziale** (q₀ ∈ Q)  
  _Lo stato da cui parte la computazione_

- **F**: insieme degli **stati finali/accettanti** (F ⊆ Q)  
  _Stati che indicano che l'input è stato accettato_

#### Componenti principali:

- **Stati**: situazioni o condizioni in cui può trovarsi il sistema
- **Transizioni**: passaggi da uno stato all'altro
- **Input**: eventi che causano le transizioni
- **Stato iniziale**: punto di partenza
- **Stati finali/accettanti**: stati che indicano un risultato valido

### 1.2 Esempio visivo: Semaforo

![FSM Semaforo](img/FSM-semaforo.png)

**Stati**: Verde, Giallo, Rosso  
**Transizioni**: basate su un timer  
**Input**: tempo trascorso

### 1.3 Esempio: Riconoscimento di una password

Immaginiamo una FSM che riconosce la sequenza "123":

![FSM Password](img/FSM-password.png)

- **S0**: stato iniziale
- **S1**: abbiamo letto '1'
- **S2**: abbiamo letto '1' e '2'
- **S3**: stato finale/accettante (abbiamo letto '123')

### 1.4 Tipi di FSM

#### Deterministic Finite Automaton (DFA)
- Da ogni stato, per ogni input, c'è esattamente una transizione possibile
- Più facile da implementare

#### Non-deterministic Finite Automaton (NFA)
- Possono esistere più transizioni per lo stesso input
- Può includere transizioni ε (vuote)
- Teoricamente equivalente a DFA, ma spesso più compatto

### 1.5 Applicazioni pratiche

- **Compilatori**: analisi lessicale
- **Protocolli di rete**: gestione delle connessioni TCP
- **Interfacce utente**: navigazione tra schermate
- **Controlli industriali**: logica di funzionamento di macchinari
- **Videogiochi**: comportamento dei personaggi (AI)
- **Validazione**: espressioni regolari!

---

## 2. Espressioni Regolari (Regex)

[Testare RegEx](https://regex101.com/)

### 2.1 Collegamento con le FSM

Le **espressioni regolari** sono una notazione compatta per descrivere pattern che possono essere riconosciuti da una FSM. Ogni regex corrisponde a una FSM!

### 2.2 Sintassi di base

#### Caratteri letterali
```regex
abc     → corrisponde esattamente alla sequenza "abc"
hello   → corrisponde a "hello"
```

#### Metacaratteri speciali

| Simbolo | Significato | Esempio |
|---------|-------------|---------|
| `.` | Qualsiasi carattere (tranne newline) | `a.c` → "abc", "a7c", "a c" |
| `^` | Inizio della riga | `^Hello` → "Hello world" ✓, "Say Hello" ✗ |
| `$` | Fine della riga | `end$` → "The end" ✓, "end game" ✗ |
| `*` | 0 o più ripetizioni | `ab*c` → "ac", "abc", "abbc" |
| `+` | 1 o più ripetizioni | `ab+c` → "abc", "abbc" (non "ac") |
| `?` | 0 o 1 occorrenza | `colou?r` → "color", "colour" |
| `|` | OR logico | `cat|dog` → "cat" o "dog" |
| `[]` | Set di caratteri | `[aeiou]` → qualsiasi vocale |
| `[^]` | Negazione del set | `[^0-9]` → qualsiasi carattere non numerico |
| `()` | Gruppo di cattura | `(ab)+` → "ab", "abab", "ababab" |

### 2.3 Classi di caratteri

```regex
\d    → cifra (digit) [0-9]
\D    → non-cifra [^0-9]
\w    → carattere di parola [a-zA-Z0-9_]
\W    → non-carattere di parola
\s    → spazio bianco (spazio, tab, newline)
\S    → non-spazio bianco
```

### 2.4 Quantificatori

```regex
{n}      → esattamente n ripetizioni
{n,}     → almeno n ripetizioni
{n,m}    → da n a m ripetizioni

Esempi:
\d{3}    → esattamente 3 cifre (es: "123")
\d{2,4}  → da 2 a 4 cifre (es: "12", "123", "1234")
[a-z]{5,} → almeno 5 lettere minuscole
```

### 2.5 Esempi pratici

#### Email (versione base - con limitazioni)
```regex
[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
```
⚠️ **Attenzione**: Questo pattern ha delle limitazioni! Accetta anche email non valide come `.@a.com` o `test..name@example.com` perché non controlla i punti all'inizio/fine o consecutivi.

#### Email (versione migliorata)
```regex
^[a-zA-Z0-9][a-zA-Z0-9._%+-]*[a-zA-Z0-9]@[a-zA-Z0-9][a-zA-Z0-9.-]*\.[a-zA-Z]{2,}$
```
✓ Questa versione:
- Non permette punti all'inizio o alla fine della parte locale
- Richiede almeno 2 caratteri nella parte locale (o usa `|^[a-zA-Z0-9]@...` per permettere email da 1 carattere)
- Controlla meglio il formato del dominio

**Nota**: La validazione perfetta delle email secondo RFC 5322 è molto complessa. Per uso reale, considera di usare librerie dedicate o semplicemente verificare la presenza di `@` e inviare un'email di conferma!

#### Numero di telefono svizzero
```regex
0\d{2}\s?\d{3}\s?\d{2}\s?\d{2}
```
Corrisponde a: "078 123 45 67" o "0781234567"

#### URL
```regex
https?:\/\/[a-zA-Z0-9\.\-]+\.[a-zA-Z]{2,}(\/.*)?
```

#### Data formato DD/MM/YYYY
```regex
\d{2}\/\d{2}\/\d{4}
```

#### Codice postale svizzero
```regex
\d{4}
```

### 2.6 Gruppi e Backreferences

#### Gruppi di cattura
```regex
(abc)+         → cattura ripetizioni di "abc"
(\d{2})-(\d{2})  → cattura due gruppi di cifre
```

#### Gruppi nominati
```regex
(?<anno>\d{4})-(?<mese>\d{2})-(?<giorno>\d{2})
```

#### Backreferences
```regex
(\w+)\s+\1    → trova parole duplicate
               → "hello hello" ✓
```

---

## 3. Uso Pratico in VS Code

### 3.1 Ricerca di base

1. Premi `Ctrl+F` (Windows/Linux) o `Cmd+F` (Mac)
2. Clicca sull'icona `.*` per attivare la modalità regex
3. Inserisci il tuo pattern

### 3.2 Ricerca e sostituzione

1. Premi `Ctrl+H` (Windows/Linux) o `Cmd+H` (Mac)
2. Attiva la modalità regex (`.*`)
3. Usa i gruppi di cattura per riutilizzare parti del match

#### Esempio 1: Invertire nome e cognome

**Cerca:**
```regex
(\w+)\s+(\w+)
```

**Sostituisci con:**
```
$2, $1
```

**Risultato:**
```
Mario Rossi    → Rossi, Mario
Laura Bianchi  → Bianchi, Laura
```

#### Esempio 2: Formattare numeri di telefono

**Cerca:**
```regex
0(\d{2})(\d{3})(\d{2})(\d{2})
```

**Sostituisci con:**
```
0$1 $2 $3 $4
```

**Risultato:**
```
0781234567 → 078 123 45 67
```

### 3.3 Ricerca multi-file

1. Premi `Ctrl+Shift+F`
2. Attiva regex
3. Usa "Include/Exclude files" per filtrare

**Esempio:** Trovare tutti i TODO nel codice
```regex
//\s*TODO:.*
```

### 3.4 Esempi utili per la programmazione

#### Trovare funzioni in JavaScript
```regex
function\s+\w+\s*\([^)]*\)
```

#### Trovare import in Python
```regex
^import\s+\w+|^from\s+\w+\s+import
```

#### Trovare numeri magici (hardcoded numbers)
```regex
=\s*\d+\.?\d*
```

#### Rimuovere console.log
```regex
console\.log\(.*\);?\n?
```

---

## 4. Esempi in Python

### 4.1 Modulo `re`

```python
import re

# Sintassi di base
# re.search()  → trova la prima occorrenza
# re.match()   → controlla dall'inizio della stringa
# re.findall() → trova tutte le occorrenze
# re.sub()     → sostituisce
```

### 4.2 Esempio 1: Validazione email

```python
import re

def valida_email(email):
    # Pattern migliorato: evita punti all'inizio/fine e punti consecutivi
    pattern = r'^[a-zA-Z0-9][a-zA-Z0-9._%+-]*[a-zA-Z0-9]@[a-zA-Z0-9][a-zA-Z0-9.-]*\.[a-zA-Z]{2,}$|^[a-zA-Z0-9]@[a-zA-Z0-9][a-zA-Z0-9.-]*\.[a-zA-Z]{2,}$'
    return re.match(pattern, email) is not None

# Test
print(valida_email("mario.rossi@example.com"))  # True
print(valida_email("invalid.email"))            # False
print(valida_email(".test@example.com"))        # False (inizia con punto)
print(valida_email("test..name@example.com"))   # False (punti consecutivi)
```

### 4.3 Esempio 2: Estrarre informazioni

```python
import re

testo = """
Studente: Mario Rossi, Età: 17
Studente: Laura Bianchi, Età: 18
Studente: Luca Verdi, Età: 16
"""

# Pattern con gruppi nominati
pattern = r'Studente: (?P<nome>\w+ \w+), Età: (?P<eta>\d+)'

studenti = re.findall(pattern, testo)
for nome, eta in studenti:
    print(f"{nome} ha {eta} anni")
```

**Output:**
```
Mario Rossi ha 17 anni
Laura Bianchi ha 18 anni
Luca Verdi ha 16 anni
```

### 4.4 Esempio 3: Validazione password

```python
import re

def valida_password(password):
    """
    Password valida deve avere:
    - Almeno 8 caratteri
    - Almeno una lettera maiuscola
    - Almeno una lettera minuscola
    - Almeno un numero
    - Almeno un carattere speciale
    """
    if len(password) < 8:
        return False
    
    # Controlli individuali
    ha_maiuscola = re.search(r'[A-Z]', password) is not None
    ha_minuscola = re.search(r'[a-z]', password) is not None
    ha_numero = re.search(r'\d', password) is not None
    ha_speciale = re.search(r'[!@#$%^&*(),.?":{}|<>]', password) is not None
    
    return ha_maiuscola and ha_minuscola and ha_numero and ha_speciale

# Test
print(valida_password("Password123!"))  # True
print(valida_password("weakpass"))      # False
```

### 4.5 Esempio 4: Pulizia del testo

```python
import re

def pulisci_testo(testo):
    # Rimuove spazi multipli
    testo = re.sub(r'\s+', ' ', testo)
    
    # Rimuove caratteri speciali (mantiene lettere, numeri, spazi)
    testo = re.sub(r'[^a-zA-Z0-9\s]', '', testo)
    
    # Trim degli spazi iniziali e finali
    testo = testo.strip()
    
    return testo

testo_sporco = "  Hello,   World!!!   "
print(pulisci_testo(testo_sporco))  # "Hello World"
```

### 4.6 Esempio 5: Parsing di log file

```python
import re
from datetime import datetime

log_entry = "2025-11-10 14:30:45 ERROR: Database connection failed"

pattern = r'(?P<data>\d{4}-\d{2}-\d{2}) (?P<ora>\d{2}:\d{2}:\d{2}) (?P<livello>\w+): (?P<messaggio>.*)'

match = re.match(pattern, log_entry)
if match:
    info = match.groupdict()
    print(f"Data: {info['data']}")
    print(f"Ora: {info['ora']}")
    print(f"Livello: {info['livello']}")
    print(f"Messaggio: {info['messaggio']}")
```

### 4.7 Esempio 6: Sostituzioni complesse

```python
import re

def formatta_telefono(testo):
    """Formatta numeri svizzeri da 0781234567 a 078 123 45 67"""
    pattern = r'0(\d{2})(\d{3})(\d{2})(\d{2})'
    return re.sub(pattern, r'0\1 \2 \3 \4', testo)

testo = "Chiamami al 0781234567 o al 0791234567"
print(formatta_telefono(testo))
# Output: "Chiamami al 078 123 45 67 o al 079 123 45 67"
```

---

## 5. Esempi in JavaScript

### 5.1 Sintassi di base

```javascript
// Creazione di una regex
const regex1 = /pattern/flags;
const regex2 = new RegExp('pattern', 'flags');

// Flags comuni:
// g = global (tutte le occorrenze)
// i = case-insensitive
// m = multiline
// s = dotAll (. include anche newline)
```

### 5.2 Metodi principali

```javascript
// String methods
string.match(regex)      // Trova corrispondenze
string.search(regex)     // Trova l'indice
string.replace(regex, replacement)  // Sostituisce
string.split(regex)      // Divide la stringa

// RegExp methods
regex.test(string)       // Ritorna true/false
regex.exec(string)       // Ritorna array con dettagli
```

### 5.3 Esempio 1: Validazione form

```javascript
function validaForm(dati) {
    // Email
    const emailRegex = /^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$/;
    if (!emailRegex.test(dati.email)) {
        return { valido: false, errore: "Email non valida" };
    }
    
    // Telefono svizzero
    const telRegex = /^0\d{9}$/;
    if (!telRegex.test(dati.telefono.replace(/\s/g, ''))) {
        return { valido: false, errore: "Numero di telefono non valido" };
    }
    
    // Codice postale svizzero
    const capRegex = /^\d{4}$/;
    if (!capRegex.test(dati.cap)) {
        return { valido: false, errore: "CAP non valido" };
    }
    
    return { valido: true };
}

// Test
const risultato = validaForm({
    email: "mario.rossi@example.com",
    telefono: "078 123 45 67",
    cap: "6900"
});

console.log(risultato);
```

### 5.4 Esempio 2: Parsing di dati

```javascript
const testo = `
Nome: Mario, Cognome: Rossi, Email: mario@example.com
Nome: Laura, Cognome: Bianchi, Email: laura@example.com
`;

const regex = /Nome: (\w+), Cognome: (\w+), Email: ([\w.@]+)/g;

let match;
const persone = [];

while ((match = regex.exec(testo)) !== null) {
    persone.push({
        nome: match[1],
        cognome: match[2],
        email: match[3]
    });
}

console.log(persone);
```

### 5.5 Esempio 3: Evidenziare testo (HTML)

```javascript
function evidenzia(testo, parola) {
    const regex = new RegExp(`(${parola})`, 'gi');
    return testo.replace(regex, '<mark>$1</mark>');
}

const paragrafo = "JavaScript è fantastico. javascript è ovunque!";
const risultato = evidenzia(paragrafo, "javascript");
console.log(risultato);
// Output: "<mark>JavaScript</mark> è fantastico. <mark>javascript</mark> è ovunque!"
```

### 5.6 Esempio 4: Formattazione numeri

```javascript
function formattaNumero(numero) {
    // Aggiunge apostrofi ogni 3 cifre (stile svizzero)
    return numero.toString().replace(/\B(?=(\d{3})+(?!\d))/g, "'");
}

console.log(formattaNumero(1234567));    // "1'234'567"
console.log(formattaNumero(1000000));    // "1'000'000"
```

### 5.7 Esempio 5: Slug URL

```javascript
function creaSlug(titolo) {
    return titolo
        .toLowerCase()                    // Minuscolo
        .replace(/[àáâãäå]/g, 'a')       // Normalizza caratteri accentati
        .replace(/[èéêë]/g, 'e')
        .replace(/[ìíîï]/g, 'i')
        .replace(/[òóôõö]/g, 'o')
        .replace(/[ùúûü]/g, 'u')
        .replace(/[^a-z0-9\s-]/g, '')    // Rimuove caratteri speciali
        .replace(/\s+/g, '-')             // Spazi -> trattini
        .replace(/-+/g, '-')              // Trattini multipli -> singolo
        .replace(/^-|-$/g, '');           // Rimuove trattini all'inizio/fine
}

console.log(creaSlug("Guida alle Espressioni Regolari!"));
// Output: "guida-alle-espressioni-regolari"
```

### 5.8 Esempio 6: Mascheramento dati sensibili

```javascript
function maschera(testo) {
    // Maschera email (mantiene primo carattere e dominio)
    testo = testo.replace(
        /([a-zA-Z])[a-zA-Z0-9._-]*@/g,
        '$1***@'
    );
    
    // Maschera carte di credito (mantiene ultime 4 cifre)
    testo = testo.replace(
        /\b\d{4}[\s-]?\d{4}[\s-]?\d{4}[\s-]?(\d{4})\b/g,
        '**** **** **** $1'
    );
    
    return testo;
}

const dati = "Email: mario.rossi@example.com, Carta: 1234 5678 9012 3456";
console.log(maschera(dati));
// Output: "Email: m***@example.com, Carta: **** **** **** 3456"
```

---

## 6. Esercizi Pratici

### Esercizio 1: Validatore di codice fiscale svizzero (AVS)
Crea una funzione che valida un numero AVS nel formato: 756.1234.5678.90

**Suggerimento:** Il formato è fisso: 3 cifre, punto, 4 cifre, punto, 4 cifre, punto, 2 cifre

### Esercizio 2: Parser di orario
Scrivi una regex che riconosca orari nel formato 24h (es: 14:30, 09:15) e li converta in formato 12h (es: 2:30 PM, 9:15 AM)

### Esercizio 3: Censura di parolacce
Crea una funzione che sostituisce una lista di parole non appropriate con asterischi, mantenendo la lunghezza

### Esercizio 4: Estrazione hashtag
Estrai tutti gli hashtag da un testo social (es: "#tutorial #regex #coding")

### Esercizio 5: Validatore IBAN svizzero
Gli IBAN svizzeri iniziano con CH seguito da 2 cifre di controllo e 17 caratteri alfanumerici

### Esercizio 6: Contatore di parole
Crea un contatore che distingue tra parole normali, numeri e punteggiatura

### Esercizio 7: Formattatore di codice
Scrivi un parser che identifica e formatta blocchi di codice in Markdown (```language ... ```)

### Esercizio 8: FSM per distributore automatico
Implementa una FSM che simula un distributore automatico:
- Accetta monete da 0.50, 1.00, 2.00 CHF
- Bevanda costa 2.50 CHF
- Restituisce resto se necessario

---

## Risorse aggiuntive

### Tool online per testare regex
- [Regex101](https://regex101.com/) - Debugger con spiegazioni
- [RegExr](https://regexr.com/) - Visual regex tester
- [Debuggex](https://www.debuggex.com/) - Visualizzatore di FSM

### Cheat sheets
- [Regex Quick Reference](https://www.rexegg.com/regex-quickstart.html)
- [Python re module](https://docs.python.org/3/library/re.html)
- [MDN JavaScript RegExp](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)

### Libri e tutorial
- "Mastering Regular Expressions" di Jeffrey Friedl
- "Automate the Boring Stuff with Python" - capitolo sulle regex

---

## Note per gli studenti

1. **Iniziate semplici**: Non cercate di scrivere regex complicate subito
2. **Testate spesso**: Usate tool online per verificare i vostri pattern
3. **Commentate**: Le regex possono diventare illeggibili, usate commenti
4. **Performance**: Regex complesse possono essere lente su testi grandi
5. **Alternative**: A volte, metodi di stringa semplici sono più leggibili

**Buon lavoro con le espressioni regolari! 🚀**
