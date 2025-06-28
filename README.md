# Tecniche di programmazione

Riassunto teorico del corso - Federico Dutto - Politecnico di Torino (A.A. 2024/25)

## 1. Analisi della complessità

* O(n): **al massimo** vengono fatte n operazioni;
* Ω(n): **al minimo** vengono fatte n operazioni;
* Θ(n): vengono fatte **esattamente** n operazioni.

```c
for (int i = 0; i < n; i++) 
        if (arr[i] == 1) 
            return true;
return false;

// T(n) ∈ O(n), T(n) ∈ Ω(n), T(n) ∉ Θ(n)
```

## 2. Online connectivity

- ### Quick-find

Rappresento con un vettore ```id``` l'insieme delle coppie connesse. Se p e q sono connessi, ``` id[p] = id[q] ```. Se ``` id[i] = i ```, non c'è nessuna connessione.

- #### Algoritmo:
 
Per ogni coppia (p, q), se è connessa passo alla coppia successiva, altrimenti **connetto la coppia**, cambiando tutti i valori ``` id[p] ``` in ``` id[q] ```.

<div align="center">
    <img src = "foto\quickfind.png" width = 400 px>
</div><br>

⚠️ in breve: per unire due elementi p e q, si sostituiscono tutti gli elementi del vettore id che hanno valore uguale a ``` id[p] ``` con il valore ``` id[q] ```.

- #### Analisi complessità:

1. Per ogni coppia:
    - find: semplice riferimento a una cella del vettore -> O(1)
    - union: il ciclo scandisce sempre tutto il vettore -> Θ(n)
    - sono necessarie 2 find e 1 union -> Θ(n)

2. Caso peggiore per d coppie:
    - find: ogni coppia richiede 2 find -> 2dΘ(1)
    - union: tutte le d coppie richiedono una union -> Θ(dn)
    - complessità totale -> Θ(dn + 2d)

3. Caso medio per d coppie:
    - find: ogni coppia richiede 2 find -> 2dΘ(1)
    - union: tutte le d coppie richiedono una union -> O(dn)
    - complessità totale -> O(dn + 2d)

***

- ### Quick-union

È una **versione migliorata di Quick-find**, in cui ogni insieme viene rappresentato come un albero. Ogni elemento punta al suo genitore, e la radice dell’albero è il rappresentante del suo insieme.

Indico con ``` (id[i])* = id[id[id[… id[i]]]] ```, se gli oggetti i e j sono connessi -> ``` (id[i])* = (id[j])* ```.

- #### Algoritmo:

Per ogni coppia (p, q), se è connessa (``` (id[i])* = (id[j])* ```) passo alla coppia successiva, altrimenti connetto la coppia, cambiando tutti i valori ``` id[(id[p])*] ``` in ``` (id[q])* ```.

<br>
<div align="center">
    <img src = "foto\quickunion.png" width = 470px
    >
</div><br>

⚠️ in breve: metto in **posizione dell'elemento radice del primo valore l'elemento radice del secondo valore**.

- #### Analisi complessità:

1. Per ogni coppia:
    - find: percorrimento di una catena di oggetti -> O(N) caso peggiore, Θ(n) caso medio
    - union: basta far sì che un oggetto punti all’altro -> Θ(1)

2. Caso peggiore per d coppie:
    - find: ogni coppia richiede 2 find e il percorrimento di 2 catene -> 2dΘ(n)
    - union: tutte le d coppie richiedono una union -> Θ(d1)
    - complessità totale -> Θ(2dn + d)

3. Caso medio per d coppie:
    - find: ogni coppia richiede 2 find e il percorrimento di 2 catene -> 2dO(1)
    - union: non tutte le d coppie richiedono una union -> O(d)
    - complessità totale -> O(2dn + d)

<br>
<div align="center">
    <img src = "foto\esempio_quick_union.png" width = 470px>
</div><br> 

***

- ### Quick-union ottimizzata

Per ridurre la lunghezza della catena, si mantiene traccia del numero di elementi di ciascun albero (array sz) e **si collega l’albero più piccolo a quello più grande**.

<br>
<div align="center">
    <img src = "foto\w_quick_union.png" width = 400px>
</div><br> 

- #### Analisi complessità:

1. Per ogni coppia:
    - find: percorrimento di una catena di oggetti -> Θ(log(n)) caso peggiore, O(log(n)) caso medio
    - union: basta far sì che un oggetto punti all’altro -> O(1)

2. Caso peggiore per d coppie:
    - find: ogni coppia richiede 2 find e il percorrimento di 2 catene -> 2dΘ(log(n))
    - union: tutte le d coppie richiedono una union -> dΘ(1)
    - complessità totale -> Θ(2dlog(n) + d)

3. Caso medio per d coppie:
    - find: ogni coppia richiede 2 find e il percorrimento di 2 catene -> 2dO(log(n))
    - union: non tutte le d coppie richiedono una union -> O(d)
    - complessità totale -> O(2dlog(n) + d)


***

- ### Riassunto complessità online connectivity

<br>
<div align="center">
    <img src = "foto\complessita_online_conn.png" width = 700px>
</div><br> 

## 3. Algoritmi di ordinamento quadratici

- ### Insertion sort

Ad ogni passo **si espande il sottovettore** sinistro già ordinato inserendovi un elemento preso dal sottovettore destro ancora disordinato. Viene utilizzata una variabile ``` x ``` come variabile temporanea per salvare il dato da inserire successivamente nella posizione corretta.

<br>
<div align="center">
    <img src = "foto\insertion_sort.png" width = 450px>
</div><br> 

Caratteristiche:
- In loco: oltre al vettore A si usa solo la variabile ``` x ```;
- Stabile: se l’elemento da inserire è una chiave duplicata, non può mai scavalcare a sinistra un’occorrenza precedente della stessa chiave.

***

- ### Bubble sort

Concettualmente il vettore A è diviso in 2 sottovettori: quello di destra, il quale è già ordinato; quello di sinistra, ancora disordinato. Dal vettore di sinistra si seleziona l'**elemento maggiore**, partendo a scambiare dalla prima posizione a sinistra, il quale sarà scambiato con l'elemento alla sua destra in caso questo sia minore dell'elemento selezionato. Questa operazione prosegue fino al raggiungimento del sottovettore destro.

<br>
<div align="center">
    <img src = "foto\bubble_sort.png" width = 450px>
</div><br> 

Caratteristiche:
- In loco: oltre al vettore A si usa solo la variabile ``` temp ```;
- Stabile: tra più chiavi duplicate quella più a destra prende la posizione più a destra e non viene mai scavalcata a destra da un’altra chiave uguale.

### Versione ottimizzata:

Viene utilizzato un flag che termina l'esecuzione **se non ci sono più stati scambi**: in questo caso il vettore è **già ordinato**. La complessità del caso peggiore passa da Θ(n^2) a O(n^2).

***

- ### Selection sort

Concettualmente il vettore A è diviso in 2 sottovettori: quello di sinistra, il quale è già ordinato; quello di destra, ancora disordinato. Per ogni sottovettore destro, si identifica il **valore minimo** e lo si scambia con quello più a sinistra del vettore destro.

<br>
<div align="center">
    <img src = "foto\selection_sort.png" width = 450px>
</div><br> 

Caratteristiche:
- In loco: oltre al vettore A si usa solo la variabile ``` temp ```;
- Non stabile: uno scambio tra elementi lontani può far sì che un’occorrenza di una chiave duplicata si sposti a sinistra di un’occorrenza precedente della stessa chiave, scavalcandola.

***

- ### Shell sort

Nello Shell sort si considerano le sottosequenze formate dagli elementi del vettore i cui indici **distano h**. Se le sottosequenze formate da elementi a distanza h sono ordinate,il vettore si dice h-ordinato. Per ognuna delle sottosequenze si applica l’**insertion sort**.

<br>
<div align="center">
    <img src = "foto\shell_sort.png" width = 600px>
</div><br>

Per scegliere una sequenza, ci si rifà a sequenze predefinite:
* Sequenza di Shell: 1 2 4 8 16 ... O(n^2) [2^(i-1)]
* Sequenza di Hibbard: 1 3 5 7 15 31 ... O(n^(3/2)) [2^i - 1]
* Sequenza di Pratt: 1 2 3 4 6 8 9 12 ... O(n * log(n)^2) [2^p * 3^q]
* Sequenza di **Knuth**: **1 4 13 40 121** ... O(n^(3/2)) [3 * h(i-1) + 1]
* Sequenza di Sedgewick: 1 5 19 41 109 209 ... O(n^(4/2))

Caratteristiche:
- In loco: oltre al vettore A si usa solo la variabile ``` x ```;
- Non stabile: uno scambio tra elementi lontani può far sì che un’occorrenza di una chiave duplicata si sposti a sinistra di un’occorrenza precedente della stessa chiave, scavalcandola.
<br>
(Possiede le stesse caratteristiche della selection sort)


## 4. Algoritmi di ordinamento lineari

Caratteristiche:
* La posizione di un elemento nell’ordinamento non si determinatramite confronti, bensì calcolandola;
* Non vale più la complessità di caso peggiore;
* Ci sono limitazioni all'uso.

***

- ### Counting sort

Abbiamo 3 vettori:
- Vettore di input: A[0, ..., n-1] di n interi;
- Vettore risultato: B[0, ..., n-1] di n interi;
- Vettore delle occorrenze semplici/multiple C di k interi se i dati sono nell’intervallo [0, ..., k-1].

Procedimento:
1. Calcolo delle **occorrenze semplici**: si conta **quante volte** un determinato valore **compare**, incrementando la cella del vettore C in cui l'indice corrisponde al valore;

<br>
<div align="center">
    <img src = "foto\occorrenze_semplici.png" width = 600px>
</div><br>

2. Calcolo delle **occorrenze multiple**: calcolo il **numero di dati che precedono i**, e si trovano in C[i - 1]. Viene calcolata come C[i] + C[i - 1]; 

<br>
<div align="center">
    <img src = "foto\occorrenze_multiple.png" width = 550px>
</div><br>

3. Calcolo delle **posizioni finali**: scorrendo A partendo dal fondo, si va a inserire il valore corrente di A nella posizione indicata da C (dove il valore di A corrisponde all'indice, *meno 1*), decrementando la casella di C. In questo modo si costruisce il vettore B risultante.

<br>
<div align="center">
    <img src = "foto\posizioni_finali.png" width = 450px>
</div><br>

Caratteristiche:
- Non in loco: oltre al vettore A si usano anche i vettori B e C;
- Stabile: la stabilità è garantita dalla scansione da destra a sinistra del vettore A quando si posizionano gli elementi: in caso di chiavi duplicate, la prima a essere posizionata è l’ultima e finisce il più a destra possibile. Le altre chiavi duplicate non potranno mai scavalcarla visto che si decrementa la cella corrispondente del vettore delle occorrenze multiple. Se la scansione fosse stata da sinistra verso destra non si sarebbe garantita la stabilità dell’algoritmo.
<br>

***

- ### Radix sort

Viene utilizzato per ordinare dati composti da campi, per gli interi si basa sulla **posizionabilità delle cifre**. Si ordina secondo la colonna di cifre **più a destra**, poi secondo la colonna immediatamente a sinistra, fino ad ordinare secondo la colonna più a sinistra. Per ordinare, si applicano **d passi di Counting sort**.

<br>
<div align="center">
    <img src = "foto\radix_sort.png" width = 500px>
</div><br>

Caratteristiche:
- Non in loco: oltre al vettore A si usano anche i vettori B, C e D;
- Stabile: la stabilità è garantita dall’uso di un algoritmo stabile come il Counting sort per ciascuno dei passi.

## 5. Algoritmi di ordinamento linearitmici

- ### Bottom-up Merge sort

Si basa sul principio di fondere **due sottovettori ordinati** in un **vettore ordinato** la cui dimensione è la somma delle dimensioni dei 2 sottovettori di partenza.

<br>
<div align="center">
    <img src = "foto\merge_sort.png" width = 400px>
</div><br>

Per ordinare i due sottovettori (2-way merge):
1. scorrere i sottovettori sinistro e destro mediante gli indici i e j e il vettore B mediante l’indice k;
2. se ``` A[i] ≤ A[j] ```, ricopiare ``` A[i] ``` in B e avanzare i e j resta invariato, altrimenti ricopiare ``` A[j] ``` in B e avanzare j, con i che resta invariato.

<br>
<div align="center">
    <img src = "foto\merge2_sort.png" width = 500px>
</div><br>

Caratteristiche:
- Non in loco, in quanto usa il vettore ausiliario B di dimensione N;
- Stabile: in quanto la funzione Merge prende dal sottovettore sinistro in caso di chiavi uguali.


## 6. Riassunto complessità algoritmi

<br>
<div align="center">
    <img src = "foto\complessita_sort.png" width = 700px>
</div><br>

## 7. Riassunto caratteristiche algoritmi

<br>
<div align="center">
    <img src = "foto\caratteristiche_sort.png" width = 700px>
</div><br>
