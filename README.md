# README.md para o Algoritmo de Dijkstra em uma Matriz

Este documento é um guia passo a passo para entender e utilizar um algoritmo baseado no método de Dijkstra para encontrar o caminho de custo mínimo em uma matriz de pesos. O algoritmo é implementado em C++ e é projetado para trabalhar com matrizes quadradas de tamanho `n x n`.

## Descrição do Problema

Dado uma matriz `n x n` onde cada célula `(i, j)` contém um peso inteiro, o objetivo é encontrar o caminho de custo mínimo do canto superior esquerdo `(0, 0)` até o canto inferior direito `(n-1, n-1)` movendo-se apenas para direita, esquerda, cima ou baixo.

## Estrutura do Algoritmo

O algoritmo utiliza a estratégia de Dijkstra para explorar os caminhos de menor custo em um grafo, onde cada célula da matriz representa um nó e cada movimento possível entre células adjacentes representa uma aresta com peso igual ao valor da célula destino.

### Componentes Principais

1. **Matriz de Distâncias (`dist`):** Armazena a distância mínima de `(0,0)` até cada célula `(i,j)`.

2. **Matriz de Visitados (`visit`):** Indica se uma célula `(i,j)` já foi processada.

3. **Matriz de Pesos (`matriz`):** Armazena os pesos de cada célula, conforme fornecido na entrada.

4. **Fila de Prioridade (`fila`):** Utilizada para obter sempre o próximo nó com o menor custo acumulado para processamento.

### Direções de Movimento

O algoritmo permite movimento em 4 direções:
- Para cima
- Para baixo
- Para a esquerda
- Para a direita

Estas direções são representadas pelos vetores `dx` e `dy`.

### Função `verificaLimites`

Esta função verifica se uma coordenada `(x, y)` está dentro dos limites da matriz, o que é crucial para evitar acessos fora do array.

### Função `djikstra`

Esta é a função principal que implementa o algoritmo de Dijkstra. Ela inicializa as distâncias, processa cada nó usando a fila de prioridade e atualiza as distâncias para cada vizinho não visitado.

## Código Completo

Aqui está o código completo em C++ para o algoritmo descrito:

```cpp
#include<bits/stdc++.h>
using namespace std;

typedef pair<int, int> pii;
int n;
const int maxn = 10000;
const int inf = INT_MAX;

int dist[maxn][maxn];
int visit[maxn][maxn];
int matriz[maxn][maxn];

int dx[] = {1,-1, 0, 0};
int dy[] = {0, 0, 1,-1};

int verificaLimites(int x, int y){
    return x >= 0 && x < n && y >= 0 && y < n;
}

void djikstra(int x, int y){
    for(int i = 0; i < n; i++){
        for(int j = 0; j < n; j++){
            dist[i][j] = inf;
        }
    }
    
    dist[x][y] = 0;
    
    priority_queue<pair<int, pii>, vector<pair<int, pii>>, greater<pair<int, pii>>> fila;
    
    fila.push({0, {x, y}});
    
    while(!fila.empty()){
        int x_coor = fila.top().second.first;
        int y_coor = fila.top().second.second;
        fila.pop();
        
        if(visit[x_coor][y_coor]){
            continue;
        }
        visit[x_coor][y_coor] = 1;
        
        for(int i = 0; i < 4; i++){
            int new_x = x_coor + dx[i];
            int new_y = y_coor + dy[i];
            
            if(verificaLimites(new_x, new_y)){
                int peso = matriz[new_x][new_y];
                if(dist[new_x][new_y] > dist[x_coor][y_coor] + peso){
                    dist[new_x][new_y] = dist[x_coor][y_coor] + peso;
                    fila.push({dist[new_x][new_y], {new_x, new_y}});
                }
            }
        }
    }
}

int main()
{
    cin >> n;
    
    for(int i = 0; i < n; i++){
        for(int j = 0; j < n; j++){
            cin >> matriz[i][j];
        }
    }
    
    djikstra(0, 0);
    
    cout << dist[n - 1][n - 1] << endl;
    return 0;
}
```

### Entrada
```
6
0 1 0 0 0 0
1 1 0 0 1 1
1 0 1 1 1 1
0 0 0 1 1 0
0 0 1 1 1 0
0 1 0 0 0 0
```

Claro, vamos detalhar todas as interações até a conclusão do algoritmo Dijkstra. 

### Leitura da Entrada:

```cpp
int n = 6;
int matriz[6][6] = {
    {0, 1, 0, 0, 0, 0},
    {1, 1, 0, 0, 1, 1},
    {1, 0, 1, 1, 1, 1},
    {0, 0, 0, 1, 1, 0},
    {0, 0, 1, 1, 1, 0},
    {0, 1, 0, 0, 0, 0}
};
```

### Inicialização:

- `dist` (distâncias) é inicializada com infinito (`inf`):
  ```cpp
  int dist[6][6] = {
      {inf, inf, inf, inf, inf, inf},
      {inf, inf, inf, inf, inf, inf},
      {inf, inf, inf, inf, inf, inf},
      {inf, inf, inf, inf, inf, inf},
      {inf, inf, inf, inf, inf, inf},
      {inf, inf, inf, inf, inf, inf}
  };
  ```

- `visit` (visitados) é inicializada com 0 (não visitado):
  ```cpp
  int visit[6][6] = {
      {0, 0, 0, 0, 0, 0},
      {0, 0, 0, 0, 0, 0},
      {0, 0, 0, 0, 0, 0},
      {0, 0, 0, 0, 0, 0},
      {0, 0, 0, 0, 0, 0},
      {0, 0, 0, 0, 0, 0}
  };
  ```

- A fila de prioridade (`fila`) é inicializada com a célula `(0, 0)` com custo `0`.

### Execução do Algoritmo Dijkstra:

1. **Processa `(0, 0)`, custo `0`:**
   - Atualiza `dist` para vizinhos válidos:
     - `(1, 0)`: `dist = 1`
     - `(0, 1)`: `dist = 1`
   - Adiciona `(1, 0)` e `(0, 1)` à fila com custos respectivos.
   - `dist`:
     ```
     0   inf inf inf inf inf
     1   inf inf inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     ```
   - `visit`:
     ```
     1 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     ```

2. **Processa `(1, 0)`, custo `1`:**
   - Atualiza `dist` para vizinhos válidos:
     - `(2, 0)`: `dist = 2`
     - `(1, 1)`: `dist = 2`
   - Adiciona `(2, 0)` e `(1, 1)` à fila com custos respectivos.
   - `dist`:
     ```
     0   inf inf inf inf inf
     1   2   inf inf inf inf
     2   inf inf inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     ```
   - `visit`:
     ```
     1 0 0 0 0 0
     1 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     ```

3. **Processa `(0, 1)`, custo `1`:**
   - Atualiza `dist` para vizinhos válidos:
     - `(0, 2)`: `dist = 1`
     - `(1, 1)`: `dist = 2` (já está na fila com o mesmo custo)
   - Adiciona `(0, 2)` à fila com custo `1`.
   - `dist`:
     ```
     0   1   1   inf inf inf
     1   2   inf inf inf inf
     2   inf inf inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     ```
   - `visit`:
     ```
     1 1 0 0 0 0
     1 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     ```

4. **Processa `(0, 2)`, custo `1`:**
   - Atualiza `dist` para vizinhos válidos:
     - `(0, 3)`: `dist = 1`
     - `(1, 2)`: `dist = 1`
   - Adiciona `(0, 3)` e `(1, 2)` à fila com custos respectivos.
   - `dist`:
     ```
     0   1   1   1   inf inf
     1   2   1   inf inf inf
     2   inf inf inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     ```
   - `visit`:
     ```
     1 1 1 0 0 0
     1 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     ```

5. **Processa `(1, 2)`, custo `1`:**
   - Atualiza `dist` para vizinhos válidos:
     - `(2, 2)`: `dist = 2`
     - `(1, 3)`: `dist = 1`
   - Adiciona `(2, 2)` e `(1, 3)` à fila com custos respectivos.
   - `dist`:
     ```
     0   1   1   1   inf inf
     1   2   1   1   inf inf
     2   inf 2   inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     ```
   - `visit`:
     ```
     1 1 1 0 0 0
     1 0 1 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     ```

6. **Processa `(1, 3)`, custo `1`:**
   - Atualiza `dist` para vizinhos válidos:
     - `(2, 3)`: `dist = 2`
     - `(1, 4)`: `dist = 2`
   - Adiciona `(2, 3)` e `(1, 4)` à fila com custos respectivos.
   - `dist`:
     ```
     0   1   1   1   inf inf
     1   2   1   1   2   inf
     2   inf 2   2   inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     ```
   - `visit`:
     ```
     1 1 1 1 0 0
     1 0 1 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     ```

7. **Processa `(0, 3)`, custo `1`:**
   - Atualiza `dist` para vizinhos válidos:
     - `(0, 4)`: `dist = 1`
   - Adiciona `(0, 4)` à fila com custo `1`.
   - `

dist`:
     ```
     0   1   1   1   1   inf
     1   2   1   1   2   inf
     2   inf 2   2   inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     ```
   - `visit`:
     ```
     1 1 1 1 0 0
     1 0 1 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     ```

8. **Processa `(1, 4)`, custo `2`:**
   - Atualiza `dist` para vizinhos válidos:
     - `(2, 4)`: `dist = 3`
   - Adiciona `(2, 4)` à fila com custo `3`.
   - `dist`:
     ```
     0   1   1   1   1   inf
     1   2   1   1   2   inf
     2   inf 2   2   3   inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     ```
   - `visit`:
     ```
     1 1 1 1 1 0
     1 0 1 1 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     ```

9. **Processa `(2, 4)`, custo `3`:**
   - Atualiza `dist` para vizinhos válidos:
     - `(3, 4)`: `dist = 4`
     - `(2, 5)`: `dist = 4`
   - Adiciona `(3, 4)` e `(2, 5)` à fila com custos respectivos.
   - `dist`:
     ```
     0   1   1   1   1   inf
     1   2   1   1   2   inf
     2   inf 2   2   3   4
     inf inf inf inf 4   inf
     inf inf inf inf inf inf
     inf inf inf inf inf inf
     ```
   - `visit`:
     ```
     1 1 1 1 1 0
     1 0 1 1 0 0
     0 0 0 0 1 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     0 0 0 0 0 0
     ```

10. **Processa `(0, 4)`, custo `1`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(0, 5)`: `dist = 1`
    - Adiciona `(0, 5)` à fila com custo `1`.
    - `dist`:
      ```
      0   1   1   1   1   1
      1   2   1   1   2   inf
      2   inf 2   2   3   4
      inf inf inf inf 4   inf
      inf inf inf inf inf inf
      inf inf inf inf inf inf
      ```
    - `visit`:
      ```
      1 1 1 1 1 0
      1 0 1 1 0 0
      0 0 0 0 1 0
      0 0 0 0 0 0
      0 0 0 0 0 0
      0 0 0 0 0 0
      ```

11. **Processa `(0, 5)`, custo `1`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(1, 5)`: `dist = 2`
    - Adiciona `(1, 5)` à fila com custo `2`.
    - `dist`:
      ```
      0   1   1   1   1   1
      1   2   1   1   2   2
      2   inf 2   2   3   4
      inf inf inf inf 4   inf
      inf inf inf inf inf inf
      inf inf inf inf inf inf
      ```
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0 1 1 0 0
      0 0 0 0 1 0
      0 0 0 0 0 0
      0 0 0 0 0 0
      0 0 0 0 0 0
      ```

12. **Processa `(1, 5)`, custo `2`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(2, 5)`: `dist = 3` (já está na fila com maior custo)
      - `(1, 4)`: `dist = 3` (já foi processado)
    - `dist` permanece igual.
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0 1 1 1 0
      0 0 0 0 1 0
      0 0 0 0 0 0
      0 0 0 0 0 0
      0 0 0 0 0 0
      ```

13. **Processa `(2, 5)`, custo `4`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(3, 5)`: `dist = 3`
    - Adiciona `(3, 5)` à fila com custo `3`.
    - `dist`:
      ```
      0   1   1   1   1   1
      1   2   1   1   2   2
      2   inf 2   2   3   3
      inf inf inf inf 4   3
      inf inf inf inf inf inf
      inf inf inf inf inf inf
      ```
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0 1 1 1 0
      0 0 0 0 1 1
      0 0 0 0 0 0
      0 0 0 0 0 0
      0 0 0 0 0 0
      ```

14. **Processa `(3, 5)`, custo `3`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(4, 5)`: `dist = 3`
    - Adiciona `(4, 5)` à fila com custo `3`.
    - `dist`:
      ```
      0   1   1   1   1   1
      1   2   1   1   2   2
      2   inf 2   2   3   3
      inf inf inf inf 4   3
      inf inf inf inf inf 3
      inf inf inf inf inf inf
      ```
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0 1 1 1 0
      0 0 0 0 1 1
      0 0 0 0 0 1
      0 0 0 0 0 0
      0 0 0 0 0 0
      ```

15. **Processa `(4, 5)`, custo `3`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(5, 5)`: `dist = 3`
    - Adiciona `(5, 5)` à fila com custo `3`.
    - `dist`:
      ```
      0   1   1   1   1   1
      1   2   1   1   2   2
      2   inf 2   2   3   3
      inf inf inf inf 4   3
      inf inf inf inf inf 3
      inf inf inf inf inf 3
      ```
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0

 1 1 1 0
      0 0 0 0 1 1
      0 0 0 0 0 1
      0 0 0 0 0 1
      0 0 0 0 0 0
      ```

16. **Processa `(5, 5)`, custo `3`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(4, 5)`: `dist = 4` (já foi processado)
      - `(5, 4)`: `dist = 4`
    - Adiciona `(5, 4)` à fila com custo `4`.
    - `dist`:
      ```
      0   1   1   1   1   1
      1   2   1   1   2   2
      2   inf 2   2   3   3
      inf inf inf inf 4   3
      inf inf inf inf inf 3
      inf inf inf inf 4   3
      ```
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0 1 1 1 0
      0 0 0 0 1 1
      0 0 0 0 0 1
      0 0 0 0 0 1
      0 0 0 0 0 1
      ```

17. **Processa `(5, 4)`, custo `4`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(5, 3)`: `dist = 4`
    - Adiciona `(5, 3)` à fila com custo `4`.
    - `dist`:
      ```
      0   1   1   1   1   1
      1   2   1   1   2   2
      2   inf 2   2   3   3
      inf inf inf inf 4   3
      inf inf inf inf inf 3
      inf inf inf 4   4   3
      ```
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0 1 1 1 0
      0 0 0 0 1 1
      0 0 0 0 0 1
      0 0 0 0 0 1
      0 0 0 0 0 1
      ```

18. **Processa `(5, 3)`, custo `4`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(5, 2)`: `dist = 4`
    - Adiciona `(5, 2)` à fila com custo `4`.
    - `dist`:
      ```
      0   1   1   1   1   1
      1   2   1   1   2   2
      2   inf 2   2   3   3
      inf inf inf inf 4   3
      inf inf inf inf inf 3
      inf inf 4   4   4   3
      ```
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0 1 1 1 0
      0 0 0 0 1 1
      0 0 0 0 0 1
      0 0 0 0 0 1
      0 0 0 0 0 1
      ```

19. **Processa `(5, 2)`, custo `4`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(4, 2)`: `dist = 5`
    - Adiciona `(4, 2)` à fila com custo `5`.
    - `dist`:
      ```
      0   1   1   1   1   1
      1   2   1   1   2   2
      2   inf 2   2   3   3
      inf inf inf inf 4   3
      inf inf 5   inf inf 3
      inf inf 4   4   4   3
      ```
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0 1 1 1 0
      0 0 0 0 1 1
      0 0 0 0 0 1
      0 0 0 0 0 1
      0 0 0 0 0 1
      ```

20. **Processa `(4, 2)`, custo `5`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(3, 2)`: `dist = 5`
      - `(4, 3)`: `dist = 6`
    - Adiciona `(3, 2)` e `(4, 3)` à fila com custos respectivos.
    - `dist`:
      ```
      0   1   1   1   1   1
      1   2   1   1   2   2
      2   inf 2   2   3   3
      inf inf 5   inf 4   3
      inf inf 5   6   inf 3
      inf inf 4   4   4   3
      ```
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0 1 1 1 0
      0 0 0 0 1 1
      0 0 0 0 0 1
      0 0 1 0 0 1
      0 0 0 0 0 1
      ```

21. **Processa `(3, 2)`, custo `5`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(3, 3)`: `dist = 6`
    - Adiciona `(3, 3)` à fila com custo `6`.
    - `dist`:
      ```
      0   1   1   1   1   1
      1   2   1   1   2   2
      2   inf 2   2   3   3
      inf inf 5   6   4   3
      inf inf 5   6   inf 3
      inf inf 4   4   4   3
      ```
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0 1 1 1 0
      0 0 0 0 1 1
      0 0 1 0 0 1
      0 0 1 0 0 1
      0 0 0 0 0 1
      ```

22. **Processa `(4, 3)`, custo `6`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(4, 4)`: `dist = 7`
    - Adiciona `(4, 4)` à fila com custo `7`.
    - `dist`:
      ```
      0   1   1   1   1   1
      1   2   1   1   2   2
      2   inf 2   2   3   3
      inf inf 5   6   4   3
      inf inf 5   6   7   3
      inf inf 4   4   4   3
      ```
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0 1 1 1 0
      0 0 0 0 1 1
      0 0 1 1 0 1
      0 0 1 1 0 1
      0 0 0 0 0 1
      ```

23. **Processa `(3, 3)`, custo `6`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(3, 4)`: `dist = 6` (já está na fila com maior custo)
    - `dist` permanece igual.
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0 1 1 1 0
      0 0 0 0 1 1


      0 0 1 1 0 1
      0 0 1 1 0 1
      0 0 0 0 0 1
      ```

24. **Processa `(4, 4)`, custo `7`:**
    - Atualiza `dist` para vizinhos válidos:
      - `(5, 4)`: `dist = 7` (já foi processado)
    - `dist` permanece igual.
    - `visit`:
      ```
      1 1 1 1 1 1
      1 0 1 1 1 0
      0 0 0 0 1 1
      0 0 1 1 0 1
      0 0 1 1 1 1
      0 0 0 0 0 1
      ```

25. **FIM**: A fila está vazia.

O valor final de `dist` é:
```
0   1   1   1   1   1
1   2   1   1   2   2
2   inf 2   2   3   3
inf inf 5   6   4   3
inf inf 5   6   7   3
inf inf 4   4   4   3
```

Portanto, a menor distância de `(0, 0)` até `(5, 5)` é `3`.

### Resumo da Simulação
A simulação completa do algoritmo de Dijkstra usando a matriz fornecida resultou no custo mínimo de `3` para ir do canto superior esquerdo `(0, 0)` ao canto inferior direito `(5, 5)`.

3. **Entrada:** Primeiro, digite o valor de `n` (tamanho da matriz). Em seguida, digite os pesos de cada célula da matriz `n x n`.

4. **Saída:** O programa imprimirá o custo mínimo para ir de `(0,0)` a `(n-1,n-1)`.

## Conclusão

Este algoritmo é uma aplicação eficiente do método de Dijkstra para encontrar caminhos de custo mínimo em matrizes de pesos, útil em várias aplicações de grafos e otimização de rotas.
