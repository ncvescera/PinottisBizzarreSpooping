# MST

## Regola Red Blue

* `regola blu`: 
    * Prendi un cammino del grafo senza archi <span style="color: blue">blu</span> e senza cicli (se esiste)
    * Scegli l'arco nel cammino con costo minore e coloralo di <span style="color: blue">blu</span>
* `regola rossa`:
    * Prendi un ciclo semplice (senza sottocicli) senza archi <span style="color: red">rossi</span> (se esiste)
    * Scegli l'arco nel ciclo con costo maggiore e coloralo di <span style="color: red">rosso</span>

La regola Red Blue viene utilizzata in modo non deterministico per costruire un MST.

### Regola Color Invariant

L'insieme di tutti gli archi di colore <span style="color: blue">blu</span> rappresenta un MST.

<span style="color: blue">Blu</span> = accettato
<span style="color: red">Rosso</span> = rigettato


## Algoritmo di Prim

È un algoritmo per la connessione minima: archi pesati e deve essere un albero che riesce a toccare tutti i vertici con il minor costo possibile
Usa la regola blue (se ho un taglio (una partizione dell'insieme dei vertici) inserisco l'arco di costo minimo).

[pdf utile](http://computerscience.unicam.it/merelli/algoritmi-complessita/esercitazione9.pdf)

### Funzionamento

1. Inizializzo tutti i vertici, tranne quello di partenza, a costo `+infinito` e padre `nil`
2. Inizializzo il vertice di partenza a costo `0` e padre `nil`
3. Assegno a `Q` l'isnieme dei vertici appartenenti al grafo
4. Con `extract_min()` estraggo il vertice con costo minore, che all'inizio sarà sempre il vertice di partenza per via della sua inizializzazione
5. Assegna i costi ai vertici adiacenti al vertice di partenza basandosi solo sul costo degli archi in uscita, non tenendo conto del costo dell'intero cammino
6. Continua nel ciclo while estraendo il vertice adiacente con costo minore assegnato al ciclo precedente

### Codice

```javascript
function Prime(G=(V, E): "grafo", r:"Vertice di partenza"){
    for (v in V-{r}) {
        v.pi = nil
        v.d  = inf
    }

    r.pi = nil
    r.d  = 0

    Q = V   //coda con tutti i vertici

    while (Q != 0) {
        //estra il vertice di peso minore
        u = extract_min(Q) 

        for (v in ADJ(u) AND v isin Q) {
            if (v.d > w(u, v)){
                v.d = w(u, v)
                v.pi = u
            }
        }
    }
}
```

### Costo

Il costo del primo for è trascurabile quindi consideriamo solo il while.

Il while viene eseguito V volte e al suo interno `extract_min()` ha costo:
* ![line475](/imgs/O_logV.gif) se la coda è implementata con un **MinHeap** o **Heap di Fibonacci**
* ![line475](/imgs/O_V2.gif) se la coda è implementata con un **Array**

Il costo del secondo for può essere portato fuori dal while approssimando il numero di iterazioni con la somma delle lunghezze di tutte liste di adiacenza (2E). In `relax()` il costo dell'assegnazione `v.d = w(u, v)` corrisponde ad un `decrease_key()` e il costo dipende dalla struttura della coda:
* ![line479](/imgs/O_logV.gif) se viene usato un MinHeap
* ![line480](/imgs/O_1.gif) se viene usato un Heap di Fibonacci

Quindi il costo minimo si avrà utilizzando gli Heap di Fibonacci e sarà:
![line483](/imgs/O_E+VlogV.gif) .

## Algoritmo di Kruscal

### Funzionamento

1. L'insieme A che conterrà gli archi viene inizializzato a vuoto
2. Per ogni Vertice v viene creato un albero a se stante tramite la funzione `makeSet()`
3. Gli archi E vengono ordinati in modo crescente in base al loro peso
4. Per ogni arco appartenente ad E provo a collegarci un altro vertice, se questo collegamento NON forma un cilco allora aggiungo l'arco ad A e unisco i vertici in un unico Albero
5. Ritorna A

### Codice

```javascript
function Kruskal(G=(V, E), w){
    A = 0

    for (v in V) {
        makeSet(v)
    }

    // orgina gli archi E per peso w non decrescente
    sort(E, by=w, order=asc)

    for (arco in E) {
        if (findSet(arco.u) != findSet(arco.v)){
            A = A U {(arco.u, arco.v)}
            union(arco.u, arco.v)
        }
    }

    return A
}
```

### Costo

> ```
> aV + ElogE + E --> O(ElogE)
> E < V^2
> logE < 2logV
> O(logE) = O(logV)
> ```

![line528](/imgs/O_ElogV.gif)