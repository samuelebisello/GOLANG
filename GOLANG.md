# GOLANG

### Visibility (Exported - Unexported)

In Go un nome viene esportato (exported, pubblico), ossia è visibile al di fuori del package se comincia con la lettera maiuscola.
Quando si importa un package, ci si può solo riferire a i nomi "exported" (pubblici). Qualsiasi nome "unexported" (privato) non è accessibile al di fuori del package.

### Functions

Una funzione può avere 0 o più argomenti.

```go
func f() int {...}
func f(i int) int {...}
func f(i int, s string) {...}
```

Quando 2 o piu parametri di una funzione condividono un tipo, è possibile omettere il tipo ed inserirlo solo in fondo alla dichiarazione.

```go
func f(i, y int) int {...}
```

Una funzione può ritornare qualsiasi numero di risultati (1,2,...n valori), indicati con il tipo.

Se il # dei valori ritornati da una funzione è > 1, bisogna racchiudere il tipo del valore ritornato tra parentesi tonde.

```go
func f(x, y string) (string, string) {
    return y, x
}
```

Si può dare un nome ai valori di ritorno. Se si decide di dare un nome alle variabli da restiture dalla funzione la sintassi è la seguente:

```go
func f(i int) (x, y int) {
    x = 5 + 4
    y = x * 4
    return
}
```

I nomi dati alle variabili di ritorno dovrebbero essere significativi per documentare il tipo di ritorno

Lo statement `return` senza gli argomenti viene detto "naked" e ritorna i valori di ritorno a cui è stato dato un nome.

### Variables

La dichiarazione `var` crea una variabile di un determinato tipo, le assegna un nome ed un valore iniziale.
Ogni dichiarazione ha la seguente forma.

```go
var name type = expression
```

Sia il `type` che `= expression` possono essere omessi, ma non entrambi. 

1. Se il tipo è omesso, allora viene dedotto (type inference) dall'espressione inizializzatrice. 
2. Se invece è  omessa l'espressione inizializzatrice allora il valore iniziale della variabile è il valore zero per quel tipo.

```go
var str, b = "hello", true // caso 1, type inferece
var i = 27      	       // caso 1, type inference

var i int 				   // caso 2, inizializzato con valore zero per tipo int
```

Con lo statement `var` è possibile dichiarare anche una lista di variabili.

```go
var i, y int
var x string
```

Anche in questo caso la dichiarazione di una variabile può includere l'inizializzazione, una per variabile

```go
var i, j int = 1, 2
```

### Short variables declaration

**Solo all'interno di una funzione** è possibile utilizzare (molto consigliato) l'operatore `:=` (short variable declaration) al posto della dichiarazione con `var` (vedi sopra). 

```go
name := expression
```

La *short variable declaration* consiste in dichiarazione + inizializzazione con deduzione del tipo (type inference). 

> **NB:** una *short variable declaration* non necessariamente dichiara tutte le variabile poste nel lato sinistro. Se **alcune** di queste sono già state dichiarate nello stesso blocco lessicale, allora la *short variable declaration* agisce come **assegnamento**.

```go
func main() {  
    i:= 8
    i, k:= 7,9 // i assegnamento, k dichiarazione + inizializzazione
}
```

### Type Inference

Quando si dichiara una variablie senza specificarne esplicitamente il tipo (sia usando la sintassi `:=`che `var` ), il tipo della variabile è dedotto (inferred) dal valore dell'espressione. Se l'espressione ha un **tipo**, la nuova variabile assume quel tipo.

```go
var i int

j := i    // j è un int
var k = i // k è un int
```

Se invece il valore dell'espressione è una **costante numerica** senza tipo, la nuova variabile potrebbe assumere come tipo un  `int`, `float64`, o `complex128` a seconda della precisione della costante

```go
i := 42     	  // int
f := 3.142  	  // float64
g := 0.867 + 0.5i // complex128
```

### Basic Types

I tipi di base del linguaggio GO sono

```go
bool

string

int  int8  int16  int32  int64
uint uint8 uint16 uint32 uint64 uintptr

byte // alias per uint8

rune // alias per int32
     // rappresenta un Unicode code point

float32 float64

complex64 complex128bool
```

I tipi `int` ,`uint`, e `uintptr` occupano di solito 32 bits su sitemi a 32-bit e 64 in sistemi a 64-bit.

Quando si vuole usare un valore intero, è consigliato usar e `int` a meno che ci sia una ragione specifica per usare un tipo intero con size specifica o senza segno.

### Zero Values

Variabili dichiarate senza un inizializzatore esplicito sono automaticamente inizializzate con il proprio "valore zero", il quale è :

​	`0` per i tipi numerici	

​	`false` per il tipo booleano

​	`""` stringa vuota per le stringhe

​	`nil` per interfacce e tipi riferimento cioè *slice, pointers, map, channel, function*

Il valore zero di un tipo aggregato come  *array e struct* ha il valore zero di tutti i suoi elementi o campi.

> Il meccanismo "zero values" garantisce che una variabile contenga sempre un valore ben definito del suo tipo.

### Type Conversions 

L'espressione `T(v)` converte il valore `v` al tipo `T`

```go
i := 27
f := float64(i)
u := uint(f)
```

A differenza del C, in Go l'assegnamento tra elementi di tipi differenti rechiede una **conversione esplicita**. 

### Constants

Le costanti sono dichiarate come le variabili, ma con la keyword `const`.Le costanti possono essere caratteri, stringhe, booleani, o valori numerici.
Le costanti non possono essere dichiarati usando la sintassi `:=`.

### Numeric Constants

Le costanti numeriche sono valori ad alta precisione. Una costante senza tipo prende il tipo dal suo contesto.

### For statements

Go ha solo un costrutto per i cicli, il ciclo `for`.
Il ciclo `for` di base ha **tre** componenti separati da `;`

- *init statement*: viene eseguito prima della prima iterazione
- *condition expression*: viene valutato prima di ogni iterazione
- *post statement*: viene esegutio alla fine di ogni iterazione

L' *init statement* solitamente è una *short variable declaration*, e la variablie dichiarata è visibile solo nello scope del ciclo `for`. Non ci sono parentesi che circondano i 3 componenti del ciclo `for`.

```go
for i := 0 ; i < 10; i++ {...}
```

L' *init e post statements* sono opzionali. Omettendoli si ottiene un ciclo while.

```go
sum := 1
for sum < 100 {
	sum += sum    
}
```

Se si omette anche la "condition expression" allora il ciclo va all'infinito.

```go
for {...}
```

### If statemets

In Go, come per il ciclo `for`, nello statement `if` l'espressione non ha bisogno di essere racchiusa da parentesi rotonde, ma per il corpo sono richieste le parentesi graffe.

```go
if condition {...}
```

Inoltre l' `if `statement può iniziare con un breve statement che viene eseguito prima della condizione. Le variabili dichiarate dallo statement sono visibili solo nello scope dell'`if`.

```go
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
	}
	return lim
}
```

Le variabile dichiarate all'interno di un `if` short statement sono anche visibili all'interno dei blocchi `else`.

```go
func pow(x, n, lim float64) float64 {
	if v := math.Pow(x, n); v < lim {
		return v
    } else {
        fmt.Println(v) // v è visibile 
    }
	return lim
}
```

### Switch

Uno `switch` statement è il modo piu rapido per scrivere una sequenza di `if - else` statements. 

**Viene eseguito il primo caso il cui valore sia uguale alla condizione**. Gli switch cases vengono valutati dall'alto verso il basso, fermandosi quando viene eseguito il primo caso.

A differenza di altri linguaggi, in Go: 

1. non è necessario usare lo statement `break` in quanto è automaticamente inserito dal linguaggio alla fine di ogni case. 
2. gli *switch cases* non devono necessariamente essere costanti 
3. i valori coinvolti negli switch cases non devono necessariamente essere interi.

```go
switch i := expression; i {  
    case value1:
    	...
    case value2:
    	...
    default:
    	...
}

// oppure si può evitare l'init statement della variabile
i := "Hello"
switch i {  
    case value1:
    	...
    case value2:
    	...
    default:
    	...
}
```

Uno `switch` senza una condizione è lo stesso di uno `switch true` . E' un modo pulito per scrivere catene di lunghi if-then-else

```go
switch {  
    case value1 < value:
    	...
    case value2 == value:
    	...
    default:
    	...
}
```



### Pointers

Un puntatore contiene l'indirizzo in memoria di una variabile. Il tipo `*T` è un puntatore ad un valore di tipo `T`. Il valore zero di un puntatore è `nil`.

```go
var p *int
```

L'operatore  `&` applicato ad un valore genera il puntatore a quel valore

```
i := 27
p := &i
```

L'operatore `*` si chiama operatore di dereferenziazion/indirezione ed applicato ad un puntatore restituisce l'oggetto puntato

```go
fmt.Println(*p) // leggo i attraverso il puntatore p
*p = 23			// msetto i attraverso il puntatore p
```

