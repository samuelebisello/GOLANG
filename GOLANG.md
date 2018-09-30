# GOLANG

### Visibility (Exported - Unexported)

In Go un nome viene esportato (exported, pubblico), ossia è visibile al di fuori del package se comincia con la lettera maiuscola.
Quando si importa un package, ci si può solo riferire a i nomi "exported" (pubblici). Qualsiasi nome "unexported" (privato) non è accessibile al di fuori del package.

### Funzioni

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

### Variabili

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

Quando si dichiara una variablie senza specificarne esplicitamente il tipo (sia usando la sintassi `:=` che `var` ), il tipo della variabile è dedotto (inferred) dal valore dell'espressione. 

Se l'espressione ha un **tipo**, la nuova variabile assume quel tipo.

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

### Valori zero

Variabili dichiarate senza un inizializzatore esplicito sono automaticamente inizializzate con il proprio "valore zero", il quale è :

​	`0` per i tipi numerici	

​	`false` per il tipo booleano

​	`""` stringa vuota per le stringhe

​	`nil` per interfacce e tipi riferimento cioè *slice, pointers, map, channel, function*

Il valore zero di un tipo aggregato come  *array e struct* ha il valore zero di tutti i suoi elementi o campi.

> **NB**: Il meccanismo "zero values" garantisce che una variabile contenga sempre un valore ben definito del suo tipo.

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

Quando si vuole usare un valore intero, è consigliato usare `int` a meno che ci sia una ragione specifica per usare un tipo intero con size specifica o senza segno.

### Conversione di tipo

L'espressione `T(v)` converte il valore `v` al tipo `T`

```go
i := 27
f := float64(i)
u := uint(f)
```

A differenza del C, in Go l'assegnamento tra elementi di tipi differenti rechiede una **conversione esplicita**. 

### Constanti

Le costanti sono dichiarate come le variabili, ma con la keyword `const`.Le costanti possono essere caratteri, stringhe, booleani, o valori numerici.
Le costanti non possono essere dichiarati usando la sintassi `:=`.

### Costanti numeriche

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

### Range

Un `for` statement con una clausola `range` itera attraverso tutt le entries di:

- array
- slice
- map
- valori ricevuti da un channel

L'espressione alla destra in una clausola `range` è chiamata *range expression*, e può essere un *array*, un puntatore ad un **array*, *slice*, *string*, *map* o *channel* che consente "receive operations".

```go
sli := []int{1, 2, 3, 4, 5}

// range su slice
for i, v := range sli {	 
    fmt.Println(i, v)
}

// range su map
// range su array
// range su *array
// range su string
// range su channel
```

Per ogni entry  la clausala `range` assegna i **valori di iterazione** alle **variabile di iterazione** corrispondenti se presenti e quindi esegue il blocco. 

Per ogni iterazione, i valori di iterazione sono prodotti seguendo le rispettive variablile di iterazione secondo la seguente tabella

| Range expression | Tipo               | 1st valore   | 2nd valore |
| ---------------- | ------------------ | ------------ | ---------- |
| array o slice    | a [n]E, *[n]E, []E | Index i int  | a[i] E     |
| String           | s string           | Index i int  | s[i] rune  |
| map              | m map[K]V          | Key k K      |            |
| Channel          | c chan E, <-chan   | elemento e E |            |



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

### Switch statements

Uno `switch` statement è il modo piu rapido per scrivere una sequenza di `if - else` statements. 
Ci sono 2 forme di switch: **expression switches** e **type switches**.

#### Expression switches

In un expression switch i cases contengono espressioni che sono comparate con il valore dell'espressione dello switch. **Viene eseguito il primo caso il cui valore sia uguale alla condizione**. 

Gli switch cases vengono valutati dall'alto verso il basso, fermandosi quando viene eseguito il primo caso. Se nessun case matcha con l'espressione allora, se presente, viene eseguito il default case. 

A differenza di altri linguaggi, in Go: 

1. non è necessario usare lo statement `break` in quanto è automaticamente inserito dal linguaggio alla fine di ogni case. 
2. gli *switch cases* non devono necessariamente essere costanti 
3. i valori coinvolti negli switch cases non devono necessariamente essere interi.

La switch expression può essere preceduta da uno statement semplice.

```go
// switch preceduto da statement semplice. i nello scope dello switch
switch i := expression; i {  
    case value1: // se i == value1 tutto il resto non viene valutato
    	...
    case value2:
    	...
    default:
    	...
}

// oppure si può evitare l'init statement, e inserire solo la switch expression
i := "Hello"
switch i {  
    case value1:  // se i == value1 tutto il resto non viene valutato
    	...
    case value2:
    	...
    default:
    	...
}
```

Uno `switch` senza una condizione è lo stesso di uno `switch true` . È un modo pulito per scrivere catene di `if-then-else`

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

è possibile raggruppare più casi per uno stesso comportamento con una lista separata da `,`

```go
c = '?'
switch c {
    case ' ', '?', '&', '=':
    	return true
}
```

Quando un case viene eseguito (quindi matcha con la switch expression) si esce dallo switch (break implicito). Se invece si vuol far passare il controllo al case successivo, si può usare la keyword `fallthrough` alla fine del corpo del case.

```go
v := 100
switch v {
    case 100:					// invece di stampare 100 e uscire con il break implicito
    	fmt.Println(100)		// con fallthrough il flusso dell'esecuzione passa al case
    	fallthrough				// successivo
    case 1:
    	fmt.Println(1)
    	fallthrough
    default:
    	fmt.println("default")
    
}
```

#### Type switches

### Defer

Una dichiarazione di `defer` rimanda l'esecuzione di una funzione fino a quando la funzione che la contiene non ritorna. L'argomento di una dichiarazione `defer` viene valutata immediatamente, ma la funzione viene chiamata solo al ritorno della funzione contenitrice.

```go
func main() {
    defer fmt.Println("world")
    
    fmt.Println("hello")
}

// stampa: hello
//		   world
```

Le chiamate a funzione `defer` sono inserita in uno stack (pila). Quando la funzione che contiene gli statement `defer`ritorna, le sue chiamate posticipate sono eseguito in ordine Last-In-First-Out, quindi al contrario dell'ordine di dichiarazione nel corpo della funzione.

```go
func main() {
    defer fmt.Println("1")
    defer fmt.Println("2")
    defer fmt.Println("3")
    defer fmt.Println("4")
    
    fmt.Println("hello defer")
}

// stampa: hello defer
//		   4
//		   3
//		   2
//		   1
```

### Pointers

Un puntatore contiene l'indirizzo in memoria di una variabile. Il tipo `*T` è un puntatore ad un valore di tipo `T`. Il valore zero di un puntatore è `nil`.

```go
var p *int
```

L'operatore  `&` applicato ad un valore restituisce il suo indirizzo in memoria

```
i := 27
p := &i
```

L'operatore `*` si chiama operatore di dereferenziazion/indirezione ed applicato ad un puntatore restituisce l'oggetto puntato

```go
fmt.Println(*p) // leggo i attraverso il puntatore p
*p = 23			// metto i attraverso il puntatore p
```

### Structs

Una `struct` è un tipo di dato aggregato che raggruppa 0 o più nomi di vari tipi come singole entità. Ogni valore è chiamato field (campo). I campi di una `struct` sono accedibili attraverso l'operatore `.`. 

I campi di una `struct` possono essere acceduti attraverso un puntatore alla `struct`. Per fare ciò si dovrebbe prima dereferenziare il puntatore e poi accedere al campo della struttura `(*p).X`. Tuttavia il linguaggio permette di scrivere direttamente `p.X` senza dereferenziare esplicitamente.

```go
type Person struct {
    name string
    age int
}
```

Il valore zero di una `struct  `è composto dal valore zero di ognuno dei suoi fields.

#### Struct Literals

Una struttura letterale rappresetna una nuova struttura allocata, nella quale vengono elencati i valori dei suoi campi. Ci sono 2 forme per una struttura letterale	

1. La prima richiede che i valori siano specificati per *ogni* campo, nel giusto ordine.

   ```go
   p := Person{"Samuele", 27}
   ```

2. Nell aseconda un valore della struct è inizializzato elencando alcuni o tutti i campi della struttura e i suoi corrispondenti valori.

3. ```go
   p := Person {
       name: "Samuele", 
       age:  27
   }
   ```

L'operatore prefisso `&` associato ad una struttura letterale ritorna l'indirizzo di tale `struct`.

### Arrays

Un array è una **sequenza fissa** di zero o più elementi di **un particolare tipo**.  A causa della dimensione fissa, in Go gli array sono raramente utilizzati a favore degli **slices**.

Il tipo `[n]T` è un array di `n` valori di tipo `T`. L'espressione

```go
var a [10]int	// array di 10 interi, inizializzati con il valore zero per il tipo
				// il tipo è [10]int
```

dichiara una variabile `a` come un array di dieci interi

> **NB**: La lunghezza di un array è parte del suo tipo, perciò gli array non possono essere ridimensionati.

Per accedere agli elementi di un array si usa la consueta notazione di subscripting

```go
fmt.Println(a[0])	// stampa il primo elemento dell array
a[0] = 10			// assegnazione
```

La funzione `len` del pkg `builtin `ritorna il # degli elementi dell'array.

Si può usare un array letterale per inizializzare un array con una lista di valori

```go
arr := [3]int{1, 2, 3}
var a [3]int = [3]int{1, 2, 3}
```

In un array letterale se al posto della lunghezza si inserisce l'operatore `...`, la size dell'array è determinata dal numero degli elementi inizializzatori (elementi dentro le parentesi graffe). 

```go
arr := [...]int{1, 2, 3, 4}	// arr ha size 4, e tipo [4]int
```

> **NB:** La size di un array deve essere un valore costante

Se un elemento di un array è **comparabile**, allora anche il tipo dell'array è comparabile, quindi possiamo confrontare due arrays di quel tipo usando l'operatore `==` , il quale restituisce `true` se tutti gli elementi di un array sono uguali all'altro 

```go
arr1 := [4]int{1, 2, 3, 4}
arr2 := [4]int{1, 2, 3, 4}

fmt.Println(arr1 == arr2)	// stampa: true
```

### Slices

Uno slice rappresenta una **sequenza variabile** i cui elementi hanno tutti lo stesso tipo (array dinamici). 

Il tipo slice è scritto `[]T` e contiene elementi di tipo `T`. È come la dichiarazione di un array ma senza size.

Array e slice sono connessi. Uno slice è una struttura dati leggera che consente l'accesso ad una sottosequenza di elementi di un array, il quale è chiamato **slice's underlyng array**, cioè l'array sottostante lo slice. 

Uno slice ha 3 componenti:

- **puntatore**: punta al primo elemento dell'array che è raggiungibile attraverso lo slice
- **lunghezza**: è il numero degli elementimenti dello slice. Non può eccedere la capacità
- **capacità**: è il numero di elementi compresi tra l'inizio dello slice e la fine dell'array sottostante

![slice](/Users/samuele/Learning/GOLANG/slice.png)

Più slice possono condividere lo stesso array sottostante e possono riferirsi a parti sovrapposte di quel array (figura sopra e codice sotto).

Uno slice è formato specificando 2 indici, un limite inferiore ed uno superiore, separati dal''operatore di slice `:` .

```go
s := [7]int{1, 2, 3, 4, 5, 6, 7} // dichiarazione array inizializzato con array letterale

s1 := s[0:3]	// len == 3, capacity == 7, array sottostante == s
s2 := s[3:5]	// len == 2, capacity == 4, array sottostante == s

```

Con l'operatore di slice viene selezionato un intervallo semiaperto che include il primo elemento ma esclude l'ultimo. La seguente espressione crea uno slice che include gli elementi dall'indice 1 al 2.

```go
a := [3]string{"stone", "scissor", "paper"}	//creazione array
s := a[1:3]
```

È possibile omettere il limite superiore o inferiore (o entrambi).

```go
a[:]	// slice contente tutti gli elemnti dell'array a
a[3:]	// slice da elemento di indice 3 di a fino all'ultimo
a[:4]	// slice dal primo elemento di a fino al 4 (indice 3)
```

Fare slicing oltre la capacità dell'array sottostante causa un `panic`, mentre lo slicing oltre la lunghezza dell'array sottostante estende lo slice così che il risultato può essere più grande dell'originale.

```go
s := []int{1, 2, 3, 4, 5, 6, 7}

original := s[2:5]

fmt.Println(original[:20])	// genera un panic
newer := original[:5]		// ok 
```

> **NB:** Dal momento che uno slice contiene un puntatore all'elemento di un array, passando uno slice ad una funzione è possibile modificare gli elementi dell'array sottostante.

L'espressione che inizializza uno slice è differente da quella che inizializza un array. Uno slice letterale è come un array letterale ma la size è omessa. Questo implicitamente crea una array della size corretta (ossia del # di elementi presenti nelle parentesi graffe) e genera uno slice che punta all'array.

```go
s := []int{1, 2, 3, 4, 5, 6, 7}	// dichiarazione slice iniziallizzato con slice letterale
```

 Il "valore zero" di uno tipo slice è `nil` .

A differenza di un array, uno slice non è comparabile. Dunque non è possibile usare l'operatore `==` per verificare se due slice contengono lo stesso numero di elementi. L'unica comparazione di slice legale è con `nil` . 

Un slice con valore `nil` **non ha** un array sottostante , di conseguenza ha *length* e *capacity* uguali a zero, ma ci sono slice `non-nil` (quindi con un array sottostante) che hanno length e capacity uguale a zero come `[]int{}` oppure `make([]int, 3)[3:]`.

La funzione `make` del pkg `builtin` crea uno slice di un tipo specificato di elementi, length e capacity. L'argomento capacity può essere omesso, in tal caso la capacity viene impostata uguale alla length.

```go
make([]T, len, cap)
make([]T, len)	// cap == len 
```

Uno slice può contenere qualsiasi tipo, anche altri slice.

Per inserire un elemento in uno slice i nGO si usa la funzione `append` del pkg `builtin`.

```go
func append(s []T, vs ...T) []T
```

Il primo parametro `s` è uno slice di tipo `T`, e il resto sono 0..n valori di tipo `T` da aggiungere allo slice.

Il risultato della funzione `append` è uno slice contente tutti gli elementi dello slice originale con l'aggiunta dei valori forniti come parametri.

Se l'array sottostante ad s (primo parametro) è troppo piccolo (capacità) per contenere tutti i nuovi valori allora viene allocato un nuovo array. Lo slice ritornato punterà a questo nuovo array.



### Maps

### 







