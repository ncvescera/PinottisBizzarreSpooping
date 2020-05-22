# UnionFind

## Operazioni

* `make-set(x: "un numero qualunque")`: Crea un nuovo insieme avente un unico elemento che fa anche da rappresentante

* `union(x: "elemento di una lista", y: "elemento di una lista")`: unisce due insiemi contenenti `x` e `y`. Crea un nuovo insieme distruggendo gli insiemi precedenti e prende come rappresentante uno dei due rappresentati.
    >Si presume che gli insiemi siano disgiunti prima dell'operazione

* `find-set(x: "elemento di una lista")`: restituisce un puntatore all'insieme contenente `x`. In questo caso `x` non è il rappresentante dell'insieme ma un elemento contenuto

## Struttura degli insiemi

### Con Liste Concatenate 

[:couplekiss_man_man:](https://www.youtube.com/watch?v=nixax3WDISM "Romani Gay")

Gli insiemi sono rappresentati tramite una lista concatenata con la seguente forma:
* `head`: puntatore al rappresentante (la testa della coda)
* `value`: elemento dell'insieme
* `next`: puntatore all'elemento successivo nella lista

![liste_unionfind](/imgs/liste_unionfind.png)

#### Costo

* `make-set(x)`: ![O_1](/imgs/O_1.gif)
* `union(x, y)`: ![O_n](/imgs/O_n.gif) (n = numero di elementi dell'insieme con più elementi)
* `find-set(x)`: ![O_1](/imgs/O_1.gif)

#### Erustica

* `unione pesata`: Nella funzione `union(x, y)` si unisce sempre la lista di elementi minori a quella con elementi maggiori così da ottenere un lower bound di ![omega_n](/imgs/omega_n.gif)

**Costo totale**: ![O_m+nlogn](/imgs/O_m+nlogn.gif) se si fanno `m` operazioni `union(x, y)`, `make-set(x)` e `find-set(x)` di cui `n` `make-set(x)`

### Con Alberi

Gli insiemi disgiunti vengono rappresentati tramite un albero:

* Nodo:
    * `padre`
    * `rango`: il numero di archi da percorrere per arrivare alla foglia più distante in profondità
    * `key`: elemento dell'insieme

![alberi_unionfind](/imgs/alberi_unionfind.png)

Il padre della radice punta a se stesso e ha rango maggiore di tutto l'albero. Il rango della radice determina il rango di tutto l'albero.

#### Euristiche

* `unione per rango`: durante la procedura `union(x, y)` la radice delll'albero con rango minore viene attaccato a quella di rango maggiore (come con la realizzazione con le liste)
* `compressione del cammino`: durante la procedura `find-set(x)` tutti i nodi vengono attaccati alla radice conservando inalterato il loro rango.

#### Costo

* `make-set(x)`: ![O_1](/imgs/O_1.gif)
* `union(x, y)`: ![O_1](/imgs/O_1.gif)
* `find-set(x)`: ![O_logn](/imgs/O_logn.gif) [approssimato per eccesso a `logn`]

**Costo totale**: ![O_aplha_n](/imgs/O_alpha_n.gif) dove ![alpha](/imgs/alpha.gif) è una funzione quasi lineare

#### Codice

```javascript
function make-set(x) {
    x.p = x
    x.rank = 0
}

function union(x, y) {
    link(find-set(x), find-set(y))
}

function link(x, y) {
    if (x.rank > y.rank) {
        y.p = x
    }
    else {
        x.p = y

        if (x.rank == y.rank) {
            y.rank ++
        }
    }
}

function find-set(x) {
    if (x != x.p) {
        x.p = find-set(x.p)
    }

    return x.p
}
```