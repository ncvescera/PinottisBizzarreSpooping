# Grafi

## Potenza di un Grafo

La `matrice di adiacenza` di un Grafo è una matrice che ha per righe i vertici di partenza e per colonna i vertici di arrivo, nella posizione `[i, j]` ha `1` se i nodi i e j sono adiacenti, altrimenti `0`.

L'`elevamento a potenza` di un grafo serve per vedere se c'è un cammino di lunghezza n che unisce i nodi i e j.<br>
L'`elevametno a potenza` di un grafo equivale ad elevare a potenza la matrice di adiacenza per poi controllare se nella posizione `i, j` della matrice c'è un `1` o uno `0`: 

* se c'è `1` vuol dire che esiste un cammino di lunghezza n (esponente della potenza) che collega i nodi i e j
* se `0` allora non è presente un cammino che collega i nodi i e j.

## Liste di Adiacenza

Una lista di adiacenza consiste in un array di `|V|` liste. Ogni elemento dell'array rappresenta un vertice del Grafo (testa della lista) e ogni lista contiene tutti i vertici adiacenti alla testa della lista.

![liste_adiacenza](/imgs/liste_adiacenza.png)

## Componenti Fortemente Connesse

Una `componente connessa` è l'insieme di Vertici connessi da almeno un cammino.<br>
(Due Vertici si dicono `connessi` se esiste un cammino che li unisce.)

Una `componente fortemente connessa` è un insieme di vertici (in un grafo orientato) dove ogni vertice è muotuamente raggiungibile.

### Funzionamento

Per trovare le componenti fortemente connesse di un grafo:

1. Viene effettuata una `DFS` del Grafo `G` per calcolare i tempi di fine `u.f`.
2. Si calocla il grafto trasposto `G_trasposto`.
3. Viene effettuata una `DFS` sul grafo traposto `G_trasposto` considerando tutti i vertici in ordine decrescente ripsetto a `u.f` (calcolati al passo 1).
4. Ritorna in output le `componenti fortemente connesse`.

## Sort Topologico

Viene detto anche `Ordinamento` Lineare di un Grafo.

Serve a rendere più chiara la rappresentazione di Grafi che schematizzano una serie di azioni che devono essere eseguite secondo un ordine preciso.

### DAG

Un `DAG` è un Grafo Aciclico Orientato.

### Funzionamento

Viene effettuato sui `DAG`.<br>
Tramite una `DFS` si calcolano i tempi di fine visita di ogni Vertice, ogni volta che un Vertice diventa nero si aggiunge alla testa di una lista concatenata. Alla fine la lista concatenata sarà ordinata topologicamente. Ritorna la lista concatenata.

### Costo

![theta_V+E](/imgs/theta_V+E.gif)
