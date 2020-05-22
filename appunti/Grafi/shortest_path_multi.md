# Shortes Path da sorgente Multipla

## Algoritmo Tropicale

<details>
    <summary>(Tropicanal)</summary>
    <img src="/imgs/tropicanal.gif">
</details>

### Versione Lenta

#### Funzionamento

##### Premessa

1. **Non** ci sono loop di costo negativo
2. La lunghezza massima del cammino minimo passante per `n` **vertici** è di `n - 1` **archi**

##### Logica

Per rappresentare il cammino minimo tra tutte le coppie viene utilizzata una matrice di adiacneza `W` e vengono create `n` matrici `L`.

Ognuna viene creata progessivamente considerando la possibilità di passare per un vertice in più della matrice `L` precedente. Così si va a controllare se eiste un camino di costo inferiore al cammino minimo già presente nella matrice `L` precedente.

Per [PREMESSA .2](#premessa) . il `for` viene eseguito n - 1 volte.

#### Codice

```javascript
function tropicale_slow(W: "matrice di adiacenza"){
    n = W.rows
    L<1> = W

    for (m = 2 to n - 1) {
        //Sia L<m> una nuova matrice n*n
        L<m> = Matrix(n, n)
        L<m> = extend_shortest_path(L<m-1>, W)
    }

    return L<n - 1>
}

function extend_shortest_path(L: "matrice su cui iterare", W: "matrice di adiacenza") {
    n = L.rows
    // Sia L^= l^[i, j] una nuova matrice n*n
    L^ = Matrix(n, n)

    for (i = 1 to n) {
        for (j = 1 to n){
            l^[i, j] = inf
            
            for (k = 1 to n){
                l^[i, j] = min( l^[i, j], l[i, k] + w[k, j] )
            }
        }
    }

    return L^
}
```

#### Costo
> n * n^3

![line743_0](/imgs/O_n4.gif)

<hr>

### Versione Veloce

#### Funzionamento

##### Premessa

1. **Non** ci sono loop di costo negativo
2. La lunghezza massima del cammino minimo passante per `n` **vertici** è di `n - 1` **archi**
3. Per ridurre il costo dell'algoritmo [precedente](#versione-lenta), non serve calcolare tutte le matrici, quindi si procede con la *tencica dell'elevazione al quadrato ripetuta*.

![tecnica_segreta](/imgs/tecnica_segereta.png) 

<details style="display: inplace">
    <summary>Nani !?!?</summary>
    <img src="/imgs/okuto.gif">
</details>

<!-- MR GIU approva -->

##### Logica

Il funzionamento è simile a quello dell'algoritmo [precedente](#versione-lenta), tuttavia per ridurre il numero di iterazioni il `for` viene sostituito da un `while` il cui argomento raddoppia ad ogni iterazione, restituendo così un tempo logaritmico.

#### Codice

```javascript
function tropicale_fast(W: "matrice di adiacenza") {
    n = W.rows
    L<1> = W
    m = 1

    while(m < n - 1) {
        // Sia L<2m> una nuova matrice n*n
        L<2m> = Matrix(n, n)
        
        L<2m> = extend_shortest_path(L<m>, L<m>)

        m = 2*m
    }

    return L<m>
}

function extend_shortest_path(L: "matrice su cui iterare", W: "matrice di adiacenza") {
    n = L.rows
    // Sia L^= l^[i, j] una nuova matrice n*n
    L^ = Matrix(n, n)

    for (i = 1 to n) {
        for (j = 1 to n){
            l^[i, j] = inf
            
            for (k = 1 to n){
                l^[i, j] = min( l^[i, j], l[i, k] + w[k, j] )
            }
        }
    }

    return L^
}
```

#### Costo

> logn * n^3

![line816_0](/imgs/O_n=O_V.gif)
perchè n = V

## Algoritmo Floyd Warshll

### Funzionamento

Ad ogni iterazione controlla se passando per il vertice `k` si ottiene un cammino migliore del cammino dell'iterazione precedente il quale non conteneva il vertice `k` presente al suo interno.

Si utilizza la matrice di Adiacenza e una matrice ![line825](/imgs/PI.gif) per memorizzare il padre del vertice di arrivo

![line827_0](/imgs/line827_0.gif)

### Codice

`D<k>` è la matrice al passo `k` e `d<k>[i, j]` è l'elemento i, j della matrice `D` al passo `k`

```javascript
function floydWarshall(W: "Matrice di Adiacenza"){
    n = W.rows
    D<0> = W

    //k è il vertice da aggiungere al cammino
    for (k = 1 to n) {
        //Sia D<k> = d<k>[i,j] una nuova matrice n * n
        for (i = 1 to n) {
            for (j = 0 to n) {
                //il minimo tra il cammino passando per k e il cammino NON passando per k
                d<k>[i, j] = min(d<k-1>[i, j], d<k-1>[i, k] + d<k-1>[k, j])
            }
        }
    }

    return D<n>
}
```

### Costo

**Tempo**: ![line855](/imgs/O_n3.gif)

**Spazio**: ![line857](/imgs/O_n2.gif)

> 2 * n^2 --> O(n^2)

## Chiusura Transitiva

La chiusura transitiva è definita come il grafo `G*=(V, E*)` dove `E*` è un isniseme definito come:

`E*= {(i, j): esiste un cammino in G dal vertice i al vertice j }`

Serve per verificare se esiste un cammino che collega due vertici.

### Funzionamento

1. Setta tutti i cammini a peso 1
2. Si inizializza la Matrice `T<0>` (matrice di Adiacenza)
3. Si utilizza l'algoritmo di FloydWharsall per trovare solo l'esistenza dei cammini non considerando il loro costo.

### Codcie

```javascript
function chiusuraTransitiva(G=(V, E)) {
    n = len(G.V)

    // Sia T<0> una nuova Matrice n*n
    T<0> = Matrix(n, n)

    for (i = 1 to n) {
        for (j = 1 to n) {
            if (i == j or (i,j) in G.E)
                t<0>[i, j] = 1
            else
                t<0>[i, j] = 0
        }
    }

    for (k = 1 to n) {
        // Sia T<k> una nuova matrice n*n
        T<k> = Matrix(n, n)

        for (i = 1 to n) {
            for (j = 1 to n) {
                t<k>[i, j] = t<k-1>[i, j] or ( t<k-1>[i, k] and t<k-1>[k, j] )
            }
        }
    }

    return T<n>
}

```

### Costo

> theta(n^2) + theta(n^3) --> theta(n^3)

![theta_n3](/imgs/theta_n3.gif)

## Algoritmo di Johnson

<!-- Johnson fa un pompino a Dijkstra -->

### Funzionamento

È l'algoritmo più veloce implementato con gli Heap di Fibonacci <!--HEap di FIcobnani (da non rimuovere) -->. Altrimenti è il migliore se si tratta di grafi sparsi.

1. Aggiunge un nuovo vertice `s` e lo collega a tutti gli altri vertici tramite un arco di costo 0. Il nuovo grafo è `G_1`
2. Esegue l'algoritmo di `BelmanFord` per controllare la presenza di cicli a costo negativo. Se ci sono ritorna un errore e termina l'algoritmo
3. Per ogni vertice di `G_1` assegna ad `h(v)` il peso del cammino minimo da `s` a `v` calcolato da `BelmanFord`
4. Per ogni arco di `G_1` ne aggiorna il peso rendendolo positvo in modo da poter utilizzare `Dijkstra`
5. Inizializza la matrice `D`
6. Utilizza `Dijkstra` per ogni verte e aggiorna la matrice con i relativi pesi caloclati
7. Ritorna `D`


### Codice

```javascript
function johnson(G, w) {
    // Aggiunge un nuovo vertice s
    // e lo collega a tutti i Vertici G.V
    // dando peso 0 all'arco
    G_1 = add_vertice(G, s)

    if (bellmanFord(G_1, w, s) == false) {
        print("Il grafo ha un ciclo di peso negativo !")
    
        return -1
    }
    else {
        for (v in G_1.V){
            // Assegna ad h(v) il peso del cammino
            // calcolato da bellmanford
            h(v) = delta(s, v)
        }

        for (arco(u, v) in G_1.E) {
            // Rende ogni arco di costo positivo
            w_^(u, v) = w(u, v) + h(u) - h(v)
        }

        n = len(G.V)
        D = Matrix(n, n)

        for (u in G.V){
            // Serve per calcolare delta_^(u, v)
            // per ogni vertice v
            dijkstra(G, w_^, u)
        
            for (v in G.V) {
                d[u, v] = delta_^(u, v) + h(v) - h(u)
            }
        }

        return D
    }
}

```

### Costo

Se implementato con Heap di Fibonacci il costo complessivo è: 
![costo lungo](/imgs/O_V2_logV+VE.gif)
