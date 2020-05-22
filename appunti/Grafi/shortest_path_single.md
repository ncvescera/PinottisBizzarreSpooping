# Shortes Path da sorgente Unica

## Algoritmo di Belman-Ford

### Funzionamento

1. Inizializza tutti i vertici del grafo a peso `+infito` e padre `nil`. Assegna il peso di `s` (il nodo di partenza) a `0` e padre `nil`
2. In ogni iterazione del ciclo `for` (con contatore i) guarda tutti gli archi e ne aggiorna il peso tramite la funzione `relax()`. Ad ogni iterazione se la funzione `relax()` trova un valore di costo minore per raggiungere un determinato vertice allora aggiorna il peso e il padre.
3. Controlla se ci sono cicli di peso negativo, in caso ritorna `false` altrimenti `true`.

### Codice

```javascript
function belmanFord(G=(V, E), s){
    for (v in V) {
        v.pi = nil
        v.d  = inf
    }

    s.d = 0
    s.pi = nil

    for (i = 1 to (V - 1)) {
        for (arco(u, v) in E) {
            relax(u, v, w(u, v))
        }
    }

    for (arco(u, v) in E) {
        if (v.d > u.d + w(u, v)){
            return false
        }
    }
    
    return true
}

function relax(u: "vertice corrente", v: "vertice adiacente a u", w(u, v): "peso dell'arco tra u e v"){
    if (v.d > u.d + w(u, v)){
        v.d = u.d + w(u, v)
        v.pi = u
    }

}
```

### Costo

> `V + V*E + E --> O(VE)`

![line580](/imgs/O_VE.gif)

## Algoritmo di Dijkstra

### Funzionamento

1. Inizializza i vertici del grafo a padre `nil` e peso `+infinito`. Inizializza `s` a padre `nil` e peso `0`
2. Inizializza l'insieme delle soluzioni `S` a insieme vuoto
3. Inizilizza la coda di **min-priorità** `Q` con tutti i vertici del grafo. Viene gestito con un Heap di Fibonacci
4. Fin quando la coda non è vuota estrae il vertice di peso minimo (la prima volta sarà sempre `s`), lo aggiunge all'insieme delle soluzioni
5. Rilassa i nodi adiacenti

<!--
<details>
    <summary>SPOILER</summary>
    Romani Gay
</details>
-->

### Codice

```javascript
function dijkstra(G=(V, E), s){
    for (v in V) {
        v.d = inf
        v.pi = nil
    }

    s.pi = nil
    s.d = 0

    S = []  //insieme delle soluzioni
    Q = V   //Heap di Fibonacci

    while (Q != 0) {
        u = extractMin(Q)
        S = S U {u}

        for (v in Adj(u)) {
            relax(u, v, w(u, v))
        }
    }
}

function relax(u: "vertice corrente", v: "vertice adiacente a u", w(u, v): "peso dell'arco tra u e v"){
    if (v.d > u.d + w(u, v)){
        v.d = u.d + w(u, v)
        v.pi = u
    }

}
```

### Costo

**Per gli Heap di Fibonacci**: 

* `extractMin()` ha costo ![O_1](/imgs/O_logV.gif)
* Il ciclo for dentro il while costa `E` perchè in totale controlla tutti gli archi una singola volta
* All'interno della `relax()` viene effettuato un `decrease_key()` con costo ![O_1](/imgs/O_1.gif)

> `V + V(logV) + E`

![line638](/imgs/O_VlogV+E.gif)


## Algoritmo DAG-Shortest-Path

### Funzionamento

1. Ordina topologicamente il grafo G
2. Inizializza i vertici del grafo a padre `nil` e peso `+infinito`. Inizalizza `s` a padre nill e pseo `0`
3. Per ogni vertice ordinato topologicamente, guarda ogni arco adiacente e ne fa la `relax()`

### Codice

```javascript
function dagShortestPath(G=(V, E), s){
    G = sortTopologico(G)

    for (v in V - {s}) {
        v.d = inf
        v.pi = nil
    }

    s.d = 0
    s.pi = nil

    // I vertici vengono presi in ordine Topologico
    // ( perchè sono già stati ordinati in modto Topologico prima)
    for (u in V) {
        for (v in Adj(u)) {
            relax(u, v, w(u, v))
        }
    }
}

```

### Costo

> `theta(V + E) + V + theta(V) --> theta(V + E)`

![line677](/imgs/theta_V+E.gif)