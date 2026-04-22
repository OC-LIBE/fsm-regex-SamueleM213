# Esercizi Pratici - Regex

## Istruzioni
Risolvi gli esercizi seguenti usando le espressioni regolari. Testa le tue soluzioni con https://regex101.com/, VS Code, Python o JavaScript.

---

## Livello 1: Principiante

### Esercizio 1.1: Trova i numeri
**Obiettivo**: Scrivi una regex che trova tutti i numeri in un testo.

**Testo di test**:
```
Ho comprato 3 mele, 5 banane e 12 arance.
Il prezzo totale è 42.50 CHF.
```

**Risultato atteso**: `3`, `5`, `12`, `42`, `50`

<details>
<summary>💡 Soluzione</summary>

```regex
\d+
```
</details>

---

### Esercizio 1.2: Email semplice
**Obiettivo**: Trova tutte le email in un testo.

**Testo di test**:
```
Contatta mario@example.com o laura.bianchi@scuola.ch
```

**Risultato atteso**: `mario@example.com`, `laura.bianchi@scuola.ch`

<details>
<summary>💡 Soluzione</summary>

```regex
\w+@\w+\.\w+
```

Nota: Questa è una versione semplificata. Per una soluzione più completa:
```regex
[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}
```
</details>

---

### Esercizio 1.3: Parole che iniziano con maiuscola
**Obiettivo**: Trova tutte le parole che iniziano con una lettera maiuscola.

**Testo di test**:
```
Mario e Laura vanno a Scuola ogni Giorno.
```

**Risultato atteso**: `Mario`, `Laura`, `Scuola`, `Giorno`

<details>
<summary>💡 Soluzione</summary>

```regex
\b[A-Z]\w*
```
</details>

---

## Livello 2: Intermedio

### Esercizio 2.1: Validatore CAP Svizzero
**Obiettivo**: Crea una funzione che valida un codice postale svizzero (4 cifre).

**Test**:
- `6900` → ✓
- `1000` → ✓
- `123` → ✗
- `12345` → ✗

<details>
<summary>💡 Soluzione Python</summary>

```python
import re

def valida_cap(cap):
    return re.match(r'^\d{4}$', cap) is not None

# Test
print(valida_cap("6900"))   # True
print(valida_cap("123"))    # False
```
</details>

<details>
<summary>💡 Soluzione JavaScript</summary>

```javascript
function validaCAP(cap) {
    return /^\d{4}$/.test(cap);
}

// Test
console.log(validaCAP("6900"));  // true
console.log(validaCAP("123"));   // false
```
</details>

---

### Esercizio 2.2: Estrai hashtag
**Obiettivo**: Estrai tutti gli hashtag da un post social media.

**Testo di test**:
```
Oggi ho imparato #regex e #programming! È stato #fantastico 🎉
```

**Risultato atteso**: `regex`, `programming`, `fantastico`

<details>
<summary>💡 Soluzione Python</summary>

```python
import re

def estrai_hashtag(testo):
    return re.findall(r'#(\w+)', testo)

post = "Oggi ho imparato #regex e #programming!"
print(estrai_hashtag(post))  # ['regex', 'programming']
```
</details>

---

### Esercizio 2.3: Formatta numero di telefono
**Obiettivo**: Converti numeri di telefono da `0781234567` a `078 123 45 67`.

<details>
<summary>💡 Soluzione Python</summary>

```python
import re

def formatta_telefono(numero):
    pattern = r'0(\d{2})(\d{3})(\d{2})(\d{2})'
    return re.sub(pattern, r'0\1 \2 \3 \4', numero)

print(formatta_telefono("0781234567"))
# Output: 078 123 45 67
```
</details>

<details>
<summary>💡 Soluzione JavaScript</summary>

```javascript
function formattaTelefono(numero) {
    return numero.replace(/0(\d{2})(\d{3})(\d{2})(\d{2})/, '0$1 $2 $3 $4');
}

console.log(formattaTelefono("0781234567"));
// Output: 078 123 45 67
```
</details>

---

## Livello 3: Avanzato

### Esercizio 3.1: Validatore password forte
**Obiettivo**: Crea un validatore di password che verifica:
- Almeno 8 caratteri
- Almeno una maiuscola
- Almeno una minuscola
- Almeno un numero
- Almeno un carattere speciale (!@#$%^&*)

**Test**:
- `Password123!` → ✓
- `weakpass` → ✗
- `NOLOWER123!` → ✗

<details>
<summary>💡 Soluzione Python</summary>

```python
import re

def valida_password(password):
    if len(password) < 8:
        return False
    
    requisiti = [
        re.search(r'[A-Z]', password),      # Maiuscola
        re.search(r'[a-z]', password),      # Minuscola
        re.search(r'\d', password),         # Numero
        re.search(r'[!@#$%^&*]', password)  # Speciale
    ]
    
    return all(requisiti)
```
</details>

---

### Esercizio 3.2: Parser di log file
**Obiettivo**: Estrai data, ora, livello e messaggio da righe di log.

**Formato log**:
```
2025-11-10 14:30:45 ERROR: Database connection failed
2025-11-10 14:31:12 WARNING: Slow query detected
```

**Risultato atteso**:
```
Data: 2025-11-10
Ora: 14:30:45
Livello: ERROR
Messaggio: Database connection failed
```

<details>
<summary>💡 Soluzione Python</summary>

```python
import re

pattern = r'(?P<data>\d{4}-\d{2}-\d{2}) (?P<ora>\d{2}:\d{2}:\d{2}) (?P<livello>\w+): (?P<messaggio>.*)'

log = "2025-11-10 14:30:45 ERROR: Database connection failed"
match = re.match(pattern, log)

if match:
    info = match.groupdict()
    print(f"Data: {info['data']}")
    print(f"Ora: {info['ora']}")
    print(f"Livello: {info['livello']}")
    print(f"Messaggio: {info['messaggio']}")
```
</details>

---

### Esercizio 3.3: Validatore IBAN Svizzero
**Obiettivo**: Valida un IBAN svizzero nel formato: `CH93 0076 2011 6238 5295 7`

Regole:
- Inizia con `CH`
- Seguito da 2 cifre di controllo
- 17 caratteri alfanumerici
- Può avere spazi ogni 4 caratteri

<details>
<summary>💡 Soluzione</summary>

```python
import re

def valida_iban_ch(iban):
    # Rimuove spazi
    iban_pulito = iban.replace(' ', '')
    
    # Pattern: CH + 2 cifre + 17 alfanumerici
    pattern = r'^CH\d{2}[A-Z0-9]{17}$'
    
    return re.match(pattern, iban_pulito.upper()) is not None

# Test
print(valida_iban_ch("CH93 0076 2011 6238 5295 7"))  # True
```
</details>

---

### Esercizio 3.4: Converti data formato USA in formato EU
**Obiettivo**: Converti date da `MM/DD/YYYY` a `DD/MM/YYYY`.

**Input**: `11/10/2025`  
**Output**: `10/11/2025`

<details>
<summary>💡 Soluzione Python</summary>

```python
import re

def converti_data(data_usa):
    pattern = r'(\d{2})/(\d{2})/(\d{4})'
    return re.sub(pattern, r'\2/\1/\3', data_usa)

print(converti_data("11/10/2025"))  # 10/11/2025
```
</details>

---

### Esercizio 3.5: Rimuovi tag HTML
**Obiettivo**: Rimuovi tutti i tag HTML da un testo.

**Input**: `<p>Questo è un <strong>testo</strong> HTML.</p>`  
**Output**: `Questo è un testo HTML.`

<details>
<summary>💡 Soluzione</summary>

```python
import re

def rimuovi_html(testo):
    return re.sub(r'<[^>]+>', '', testo)

html = "<p>Questo è un <strong>testo</strong> HTML.</p>"
print(rimuovi_html(html))
```
</details>

---

## Livello 4: Esperto

### Esercizio 4.1: Trova parole duplicate consecutive
**Obiettivo**: Trova parole ripetute consecutivamente.

**Testo**: `Questo questo è un test test di parole parole duplicate`  
**Risultato**: `questo`, `test`, `parole`

<details>
<summary>💡 Soluzione</summary>

```python
import re

def trova_duplicate(testo):
    pattern = r'\b(\w+)\s+\1\b'
    return re.findall(pattern, testo, re.IGNORECASE)

testo = "Questo questo è un test test"
print(trova_duplicate(testo))  # ['questo', 'test']
```
</details>

---

### Esercizio 4.2: Validatore numero AVS
**Obiettivo**: Valida numero AVS svizzero nel formato: `756.1234.5678.90`

Regole:
- Inizia sempre con `756`
- 4 gruppi separati da punti
- Formato: `756.XXXX.XXXX.XX`

<details>
<summary>💡 Soluzione</summary>

```python
import re

def valida_avs(avs):
    pattern = r'^756\.\d{4}\.\d{4}\.\d{2}$'
    return re.match(pattern, avs) is not None

# Test
print(valida_avs("756.1234.5678.90"))  # True
print(valida_avs("755.1234.5678.90"))  # False
```
</details>

---

### Esercizio 4.3: Censura parolacce
**Obiettivo**: Sostituisci parole inappropriate con asterischi mantenendo la lunghezza.

**Parole da censurare**: `["brutto", "cattivo"]`  
**Input**: `Questo brutto esempio è cattivo`  
**Output**: `Questo ****** esempio è *******`

<details>
<summary>💡 Soluzione</summary>

```python
import re

def censura(testo, parole_vietate):
    def sostituisci(match):
        return '*' * len(match.group(0))
    
    for parola in parole_vietate:
        pattern = re.compile(re.escape(parola), re.IGNORECASE)
        testo = pattern.sub(sostituisci, testo)
    
    return testo

testo = "Questo brutto esempio è cattivo"
parole = ["brutto", "cattivo"]
print(censura(testo, parole))
```
</details>

---

### Esercizio 4.4: Estrai URL da testo
**Obiettivo**: Estrai tutti gli URL (http e https) da un testo.

<details>
<summary>💡 Soluzione</summary>

```python
import re

def estrai_url(testo):
    pattern = r'https?://[^\s]+'
    return re.findall(pattern, testo)

testo = "Visita https://example.com e http://test.org per più info"
print(estrai_url(testo))
```
</details>

---

### Esercizio 4.5: FSM Distributore Automatico
**Obiettivo**: Implementa una FSM per un distributore automatico.

**Requisiti**:
- Accetta monete: 0.50, 1.00, 2.00 CHF
- Prezzo bevanda: 2.50 CHF
- Restituisce resto
- Stati: ATTESA, PAGAMENTO, EROGAZIONE

<details>
<summary>💡 Soluzione Python</summary>

```python
class DistributoreAutomatico:
    PREZZO_BEVANDA = 2.50
    
    def __init__(self):
        self.stato = "ATTESA"
        self.credito = 0.0
    
    def inserisci_moneta(self, valore):
        if valore not in [0.50, 1.00, 2.00]:
            return "Moneta non valida"
        
        self.credito += valore
        self.stato = "PAGAMENTO"
        
        if self.credito >= self.PREZZO_BEVANDA:
            return self.eroga_bevanda()
        else:
            mancante = self.PREZZO_BEVANDA - self.credito
            return f"Credito: {self.credito:.2f} CHF. Mancano {mancante:.2f} CHF"
    
    def eroga_bevanda(self):
        self.stato = "EROGAZIONE"
        resto = self.credito - self.PREZZO_BEVANDA
        self.credito = 0
        self.stato = "ATTESA"
        
        if resto > 0:
            return f"Bevanda erogata! Resto: {resto:.2f} CHF"
        else:
            return "Bevanda erogata!"

# Test
distributore = DistributoreAutomatico()
print(distributore.inserisci_moneta(1.00))
print(distributore.inserisci_moneta(1.00))
print(distributore.inserisci_moneta(1.00))
```
</details>

---

## Sfide Bonus

### Sfida 1: Validatore email completo
Crea un validatore email che gestisce:
- Nomi con punti e trattini
- Sottodomini multipli
- Domini di primo livello di 2+ caratteri

### Sfida 2: Parser Markdown
Crea un parser che riconosce:
- Titoli (`# Titolo`, `## Sottotitolo`)
- Grassetto (`**testo**`)
- Corsivo (`*testo*`)
- Link (`[testo](url)`)
- Codice (`` `codice` ``)

### Sfida 3: Analizzatore sintassi SQL
Estrai da query SQL:
- Nome tabella dopo `FROM`
- Campi dopo `SELECT`
- Condizioni dopo `WHERE`

---

## Come testare le soluzioni

### In VS Code:
1. `Ctrl+F` per ricerca
2. Clicca sull'icona `.*` per regex
3. Testa il pattern

### In Python:
```python
import re

pattern = r'tuo_pattern'
testo = "testo di test"
risultati = re.findall(pattern, testo)
print(risultati)
```

### In JavaScript:
```javascript
const pattern = /tuo_pattern/g;
const testo = "testo di test";
const risultati = testo.match(pattern);
console.log(risultati);
```

---

## Risorse per ulteriore pratica

1. **Regex101**: https://regex101.com/
2. **RegexOne**: https://regexone.com/
3. **HackerRank Regex**: https://www.hackerrank.com/domains/regex

Buon lavoro! 🚀
