# Heap

È un albero binario completo fino al penultimo livello e nell'ultimo le foglie sono addossate a sinistra.
Ha una forma ben definita e può essere:

- un **max-heap**: ha come chiave della radice il massimo

- un **min-heap**: ha come chiave della radice il minimo

Questa struttura permette di fare un algoritmo di ordinamento sul posto con costo ![O_nlogn](/imgs/O_nlogn.gif) e permette di calcolare il massimo e il minimo con costo inferiore. L'heap viene rappresentato tramite un array.

![line215](/imgs/line215_0.gif)

I vari elementi hanno i seguenti indici:

- `left(i) = 2 * i`
- `right(i) = 2 * i + 1`
- `parent(i) = int( i/2 )`

**Codice:**

```javascript
function BuildMaxHeap(A, n){
    for (i = int(n/2) to 1){
        MaxHeapify(A, i, n)
    }
}

// Ha costo O(logn)
function MaxHeapify(A, i, n){
    largest = A[i]
    t = i

    if(2*i <= n) && (A[i] < A[2*i]){
        t = 2*i
        largest = A[t]
    }
    if(2*i+1 <= n) && (A[2*i+1] > largest){
        t = 2*i+1
        largest = A[t]
    }

    if(i != t){
        scambia(A[t], A[i])
        MaxHeapify(A, t, n)
    }
}
```

La funzione `BuildMaxHeap(A, n)` ha costo ![O_nlogn](/imgs/O_nlogn.gif).<br>
La funzione `MaxHeapify(A, i, n)`  ha costo ![O_logn](/imgs/O_logn.gif).<br>È utile per la ricerca del massimo o del minimo.

## HeapSort

**Complessità in tempo:** È un algoritmo in-place ma non stable.
![line258](/imgs/T_n_O_nlogn.gif)

**Codice:**

```javascript
function HeapSort(A, n){
    BuildMaxHeap(A, n)

    for (i = n to 2){
        scambia(A[1], A[i])
        MaxHeapify(A, 1, i-1)
    }
}
```

Con o senza `BuildMaxHeap(A, n)` il costo della funzione rimane invariato.

## Operazioni sugli Heap

### Insertion-key

Ha costo ![O_logn](/imgs/O_logn.gif). Aggiunge un nuovo nodo all'Heap e ne ricalcola il posto.

**Codice:**

```javascript
function Insertion-key(A, n, k){
    n = n + 1
    A[n] = k
    i = n

    while(A[int(i/2)] < A[i] && int(i/2) >= 1){
        scambia(A[i], A[int(i/2)])
        i = int(i/2)
    }
}
```

### Extract-max

Ha costo ![line298](/imgs/O_logn.gif).
Elimina il massimo (la radice) e ricostruisce l'Heap.

**Codice:**

```javascript
function Extract-max(A, n){
    max = A[1]
    A[1] = A[n]
    n = n - 1

    MaxHeapify(A, 1, n)

    return max
}
```

### Extract-key

Ha costo ![line317](/imgs/O_logn.gif).
Elimina l'elemento alla posizione data e ricostruisce l'Heap.

**Codice:**

```javascript
function Extrac-key(A, n, index){
    tmp = A[index]
    A[index] = A[n]

    tmp2 = A[n]
    n = n - 1

    MaxHeapify(A, index, n)

    if(A[index] == tmp2){
        i = index

        while(A[int(i/2)] < A[i] && int(i/2) >= 1){
            scambia(A[int(i/2)], A[i])
            i = int(i/2)
        }
    }
}
```