# Tipi di Visita

## BFS

Viene utilizzata su Grafi **non orientati** e **connessi**.<br>
È la base per gli algoritmi di Prim (per gli MST) e Dijkstra (per gli Shortest Path).

### Codice
```javascript
function BFS(G(V,E), s){
   // prepara il grafo per l'esplorazione
   for (v in (G.V - {s})){ // per ogni elemento di V tranne s
      v.color = white
      v.d = infinity
      v.pi = nil
   }
   
   // setta i parametri di s come nodo di partenza
   s.d = 0
   s.pi = nil
   s.color = grey
   
   enqueue(Q, s)  // aggiunge s alla coda
   
   while(Q != 0){
      u = dequeue(Q) // estrae il primo elemento aggiunto alla coda
      
      for (v in G.adj[u]){ // per ogni elemento adiacente ad u
         if(v.color == white){
            enqueue(Q, v)
            
            v.d = u.d + 1
            v.pi = u
            v.color = grey
         }
      }
      
      u.color = black
   }
}
```

### Costo

![O_V+E](/imgs/O_V+E.gif)

## DFS

Viene utilizzata soprattutto per controllare se un grafo è **aciclico** (un albero) e per la **catalogazione** degli **archi**.

Viene applicata soprattutto a **grafi orientati**.

### Tipi di archi

* `archi d'albero`: archi facenti parte dell'albero
* `archi all'indietro`: arco che raggiunge un nodo già visitato in precedenza. `v.color == gray` 
* `archi in avanti`: arco che collega un nodo ad un suo discendente che non gli è adiacente. `u.d < v.d`
* `archi trasversali`: arco che collega un vertice ad uno già visto in precedenza. `u.d > v.d` dove `u` è il nodo che viene visitato e `v` è un nodo già visitato

Dato un arco `arco(u,v)` con `u` nodo di partenza e `v` nodo di arrivo: 
* se `v.color == white` è un arco d'albero
* se `v.color == gray` è un arco all'indietro
* se `v.color == black` :
    * se `u.d < v.d` è un arco in avanti
    * se `u.d > v.d` è un arco trasversale


### Codice

```javascript
function DFS(G(V, E)){
   for (v in G.V){
      v.color = white
      v.pi = nil
   }
   
   time = 0
   
   for (v in G.V){
      if(v.color == white){
         DFS-visit(G, v)
      }
   }
}

function DFS-visit(G(V, E), u){
   u.color = grey
   time ++
   u.d = time
   
   for (v in G.adj[u]){
      if(v.color == white){
         v.pi = u
         DFS-visit(G, v)
      }
   }
   
   u.color = black
   time ++
   u.f = time
}
```

### Costo

![theta_V+E](/imgs/theta_V+E.gif)
