# Ordinamento

## Tabella Riassuntiva

Algoritmo |Complessità
-------------|----
SelectionSort|![line37](/imgs/theta_n2.gif)
InsertionSort|![line38_0](/imgs/omega_n.gif)<br>![line38_1](/imgs/O_n2.gif)
MergeSort|![line39](/imgs/theta_nlogn.gif)
QuickSort|![line40](/imgs/omega_nlogn.gif)<br>![line40_1](/imgs/O_n2.gif)
CountingSort|![line41_0](/imgs/O_n.gif)
HeapSort|![line42](/imgs/theta_nlogn.gif)

## MergeSort

**Complessità in tempo:** nel caso peggiore ![line46](/imgs/in_O_nlogn.gif)

Dividere sempre per 2 è la scelta migliore. L'algoritmo sfrutta della memoria aggiuntiva nella funzione Merge.

**Codice:**

```javascript
/*
 * A: Array da ordinare             [1:n]
 * p: Indice iniziale dell'array    (int)
 * r: Indice finale dell'arrya      (int)
 */
function MergeSort(A, p, r){
    if(p < r){
        q = int( (p+r)/2 )
        MergeSort(A, p, q)
        MergeSort(A, q+1, r)
        Merge(A, p, q, r)
    }
}

function Merge(A, p, q, r){
    n1 = q - p + 1
    n2 = (r - p + 1) - n1

    L = [1:n1+1]
    for (i= 1 to n1){
        L[i] = A[p+i-1]
    }
    L[n1+1] = inf

    R = [1:n2+1]
    for (i = 1 to n2){
        R[i] = A[q+i]
    }
    R[n2+1] = inf

    i = 1
    j = 1
    for (k = p to r){
        if(L[i] < R[j]){
            A[k] = L[i]
            i++
        }
        else {
            A[k] = R[j]
            j++
        }
    }
}
```

## QuickSort

**Complessità in tempo:** dipende dalla scelta del pivot:

- Nel caso pessimo pivot = max, ![line102](/imgs/in_O_n2.gif)
- Nel caso pessimo in cui il pivot si alterna con il massimo e il minimo, ![line103](/imgs/in_O_n2.gif)

Generalmente non capita molto spesso di imbattersi nei casi peggiori e la complessità in tempo nel caso medio è:

![line107](/imgs/T_n_O_nlogn.gif)
<br><br>
![line109](/imgs/t_n_lunga.gif)
non ho idea di cosa sia.

<br> **Codice:**

```javascript
/*
 * A: Array da ordinare                 [1:n]
 * p: Indice di partenza dell'array     (int)
 * r: Indice di fine dell'arry          (int)
*/
function QuickSort(A, p, r){
    q = Partition(A, p, r)
    QuickSort(A, p, q - 1)
    QuickSort(A, q + 1, r)
}

function Partition(A, p, r){
    x = A[r]
    i = p - 1
    for j = p to (r - 1){
        if (A[j] <= x){
            i = i + 1
            scambia(A[i], A[j])
        }
    }
    scambia(A[i+1], A[r])
    return i+1
}
```

`partition(...)` ![line140_0](/imgs/in_O_n.gif)<br>
`QuickSort(A, p, q-1)` ![line141](/imgs/in_T_q-p.gif)<br>
`QuickSort(A, q+1, r)` ![line142](/imgs/in_T_r-q.gif)<br>

## CountingSort

**Complessità in tempo:**  Richiede due array supplementari: B[1, …, n] per l’output ordinato e C[1, …, k] per la memoria di lavoro temporanea. Per eseguire il CountingSort si presume che l’array di partenza A sia un array di interi del tipo A[1, …, n] (indicano gli indici). Si assume anche che il contenuto di A varia tra 0 e k.

![line148](/imgs/T_n_O_k_n.gif)<br>

Il CountingSort non essendo un algoritmo di ordinamento per confronti ha limite inferiore ![line150_0](/imgs/omega_n.gif) (se ![line150_1](/imgs/k_O_n.gif)) e non ![line150_2](/imgs/omega_nlogn.gif) come gli algoritmi per confronti. È un algoritmo Stabile: elementi con lo stesso valore compaiono nell’array di output con lo stesso ordine che avevano in quello di input.

**Codice:**

```javascript
/*
 * A: Array di interi                   [1:n]
 * k: Valore massimo contenuto in A     (int)
 * n: Lunghezza di A                    (int)
 */
function CountingSort(A, k) {
    C = [0:k]   //Memoria temporanea di lavoro
    for (i = 0 to k){
        C[i] = 0
    }

    //C in una posizione generica i conterrà il numero degli elementi uguali ad i
    for (j = 1 to length(A)){
        C[A[j]] = C[A[j]] + 1
    }

    //C[i] contiene il numero degli elementi <= i
    for (i = 1 to k){
        C[i] = C[i] + C[i-1]
    }

    B = [1:length(A)]
    for (j = length(A) to 1){
        B[C[A[j]]] = A[j]
        C[A[j]] = C[A[j]] - 1
    }

    return(B)
}
```

## RadixSort

**Complessità in tempo:** L'algoritmo viene usato per ordinare numeri interi con d cifre in base b. Il costo è: 

![line190](/imgs/T_n_d_O_n.gif)<br>

L'algoritmo è ottimo quando ![line192](/imgs/b_d2.gif)<br>

**Codice:**

```javascript
function RadixSort(A, d){
    for(i = 1 to d){
        qualcosa di non ben definito sulla cifra i
    }
}
```

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