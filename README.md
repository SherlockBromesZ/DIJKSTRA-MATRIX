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

### Passo a Passo da Execução

###Inicialização:
   - `n = 6`
   - `matriz` é preenchida com os valores fornecidos.
   - `dist` é inicializada com `inf` (infinito) para todas as células.
   - `visit` é inicializada com `0` (não visitado) para todas as células.
   - `dist[0][0] = 0` (custo inicial é zero).
   - A fila de prioridade (`fila`) é inicializada com `{0, {0, 0}}`.

###Primeira Iteração:
   - `fila` contém `{0, {0, 0}}`.
   - `x_coor = 0`, `y_coor = 0`.
   - Marca `(0, 0)` como visitado.
   - Verifica os vizinhos:
     - `(1, 0)`: `dist[1][0] = 0 + 1 = 1`, adiciona `{1, {1, 0}}` à fila.
     - `(0, 1)`: `dist[0][1] = 0 + 1 = 1`, adiciona `{1, {0, 1}}` à fila.

###Segunda Iteração:
   - `fila` contém `{1, {1, 0}}, {1, {0, 1}}`.
   - `x_coor = 0`, `y_coor = 1`.
   - Marca `(0, 1)` como visitado.
   - Verifica os vizinhos:
     - `(1, 1)`: `dist[1][1] = 1 + 1 = 2`, adiciona `{2, {1, 1}}` à fila.
     - `(0, 2)`: `dist[0][2] = 1 + 0 = 1`, adiciona `{1, {0, 2}}` à fila.

**Terceira Iteração:
   - `fila` contém `{1, {1, 0}}, {1, {0, 2}}, {2, {1, 1}}`.
   - `x_coor = 1`, `y_coor = 0`.
   - Marca `(1, 0)` como visitado.
   - Verifica os vizinhos:
     - `(2, 0)`: `dist[2][0] = 1 + 1 = 2`, adiciona `{2, {2, 0}}` à fila.
     - `(1, 1)`: já visitado.

**Quarta Iteração:
   - `fila` contém `{1, {0, 2}}, {2, {1, 1}}, {2, {2, 0}}`.
   - `x_coor = 0`, `y_coor = 2`.
   - Marca `(0, 2)` como visitado.
   - Verifica os vizinhos:
     - `(1, 2)`: `dist[1][2] = 1 + 0 = 1`, adiciona `{1, {1, 2}}` à fila.
     - `(0, 3)`: `dist[0][3] = 1 + 0 = 1`, adiciona `{1, {0, 3}}` à fila.

###Quinta Iteração:
   - `fila` contém `{1, {1, 2}}, {1, {0, 3}}, {2, {1, 1}}, {2, {2, 0}}`.
   - `x_coor = 1`, `y_coor = 2`.
   - Marca `(1, 2)` como visitado.
   - Verifica os vizinhos:
     - `(2, 2)`: `dist[2][2] = 1 + 1 = 2`, adiciona `{2, {2, 2}}` à fila.
     - `(1, 3)`: `dist[1][3] = 1 + 1 = 2`, adiciona `{2, {1, 3}}` à fila.

###Sexta Iteração:
   - `fila` contém `{1, {0, 3}}, {2, {1, 1}}, {2, {2, 0}}, {2, {2, 2}}, {2, {1, 3}}`.
   - `x_coor = 0`, `y_coor = 3`.
   - Marca `(0, 3)` como visitado.
   - Verifica os vizinhos:
     - `(1, 3)`: já visitado.
     - `(0, 4)`: `dist[0][4] = 1 + 0 = 1`, adiciona `{1, {0, 4}}` à fila.

###Sétima Iteração:
   - `fila` contém `{1, {0, 4}}, {2, {1, 1}}, {2, {2, 0}}, {2, {2, 2}}, {2, {1, 3}}`.
   - `x_coor = 0`, `y_coor = 4`.
   - Marca `(0, 4)` como visitado.
   - Verifica os vizinhos:
     - `(1, 4)`: `dist[1][4] = 1 + 1 = 2`, adiciona `{2, {1, 4}}` à fila.
     - `(0, 5)`: `dist[0][5] = 1 + 0 = 1`, adiciona `{1, {0, 5}}` à fila.

###Oitava Iteração:
   - `fila` contém `{1, {0, 5}}, {2, {1, 1}}, {2, {2, 0}}, {2, {2, 2}}, {2, {1, 3}}, {2, {1, 4}}`.
   - `x_coor = 0`, `y_coor = 5`.
   - Marca `(0, 5)` como visitado.
   - Verifica os vizinhos:
     - `(1, 5)`: `dist[1][5] = 1 + 1 = 2`, adiciona `{2, {1, 5}}` à fila.

###Nona Iteração:
    - `fila` contém `{2, {1, 1}}, {2, {2, 0}}, {2, {2, 2}}, {2, {1, 3}}, {2, {1, 4}}, {2, {1, 5}}`.
    - `x_coor = 1`, `y_coor = 1`.
    - Marca `(1, 1)` como visitado.
    - Verifica os vizinhos:
      - `(2, 1)`: `dist[2][1] = 2 + 0 = 2`, adiciona `{2, {2, 1}}` à fila.

###Décima Iteração:
    - `fila` contém `{2, {2, 0}}, {2, {2, 2}}, {2, {1, 3}}, {2, {1, 4}}, {2, {1, 5}}, {2, {2, 1}}`.
    - `x_coor = 2`, `y_coor = 0`.
    - Marca `(2, 0)` como visitado.
    - Verifica os vizinhos:
      - `(3, 0)`: `dist[3][0] = 2 + 0 = 2`, adiciona `{2, {3, 0}}` à fila.

###Décima Primeira Iteração:
    - `fila` contém `{2, {2, 2}}, {2, {1, 3}}, {2, {1, 4}}, {2, {1, 5}}, {2, {2, 1}}, {2, {3, 0}}`.
    - `x_coor = 2`, `y_coor = 2`.
    - Marca `(2, 2)` como visitado.
    - Verifica os vizinhos:
      - `(3, 2)`: `dist[3][2] = 2 + 0 = 2`, adiciona `{2, {3, 2}}` à fila.
      - `(2, 3)`: `dist[2][3] = 2 + 1 = 3`, adiciona `{3, {2, 3}}` à fila.

### Décima Segunda Iteração:
- `fila` contém `{2, {1, 3}}, {2, {1, 4}}, {2, {1, 5}}, {2, {2, 1}}, {2, {3, 0}}, {2, {3, 2}}, {3, {2, 3}}`.
- `x_coor = 1`, `y_coor = 3`.
- Marca `(1, 3)` como visitado.
- Verifica os vizinhos:
  - `(2, 3)`: já visitado.
  - `(1, 4)`: já visitado.
  - `(0, 3)`: já visitado.
  - `(1, 2)`: já visitado.

### Décima Terceira Iteração:
- `fila` contém `{2, {1, 4}}, {2, {1, 5}}, {2, {2, 1}}, {2, {3, 0}}, {2, {3, 2}}, {3, {2, 3}}`.
- `x_coor = 1`, `y_coor = 4`.
- Marca `(1, 4)` como visitado.
- Verifica os vizinhos:
  - `(2, 4)`: `dist[2][4] = 2 + 1 = 3`, adiciona `{3, {2, 4}}` à fila.
  - `(1, 5)`: já visitado.
  - `(0, 4)`: já visitado.
  - `(1, 3)`: já visitado.

### Décima Quarta Iteração:
- `fila` contém `{2, {1, 5}}, {2, {2, 1}}, {2, {3, 0}}, {2, {3, 2}}, {3, {2, 3}}, {3, {2, 4}}`.
- `x_coor = 1`, `y_coor = 5`.
- Marca `(1, 5)` como visitado.
- Verifica os vizinhos:
  - `(2, 5)`: `dist[2][5] = 2 + 1 = 3`, adiciona `{3, {2, 5}}` à fila.
  - `(0, 5)`: já visitado.
  - `(1, 4)`: já visitado.

### Décima Quinta Iteração:
- `fila` contém `{2, {2, 1}}, {2, {3, 0}}, {2, {3, 2}}, {3, {2, 3}}, {3, {2, 4}}, {3, {2, 5}}`.
- `x_coor = 2`, `y_coor = 1`.
- Marca `(2, 1)` como visitado.
- Verifica os vizinhos:
  - `(3, 1)`: `dist[3][1] = 2 + 0 = 2`, adiciona `{2, {3, 1}}` à fila.
  - `(1, 1)`: já visitado.
  - `(2, 0)`: já visitado.
  - `(2, 2)`: já visitado.

### Décima Sexta Iteração:
- `fila` contém `{2, {3, 0}}, {2, {3, 2}}, {2, {3, 1}}, {3, {2, 3}}, {3, {2, 4}}, {3, {2, 5}}`.
- `x_coor = 3`, `y_coor = 0`.
- Marca `(3, 0)` como visitado.
- Verifica os vizinhos:
  - `(4, 0)`: `dist[4][0] = 2 + 0 = 2`, adiciona `{2, {4, 0}}` à fila.
  - `(2, 0)`: já visitado.
  - `(3, 1)`: já visitado.

### Décima Sétima Iteração:
- `fila` contém `{2, {3, 2}}, {2, {3, 1}}, {2, {4, 0}}, {3, {2, 3}}, {3, {2, 4}}, {3, {2, 5}}`.
- `x_coor = 3`, `y_coor = 2`.
- Marca `(3, 2)` como visitado.
- Verifica os vizinhos:
  - `(4, 2)`: `dist[4][2] = 2 + 1 = 3`, adiciona `{3, {4, 2}}` à fila.
  - `(2, 2)`: já visitado.
  - `(3, 1)`: já visitado.
  - `(3, 3)`: `dist[3][3] = 2 + 1 = 3`, adiciona `{3, {3, 3}}` à fila.

### Décima Oitava Iteração:
- `fila` contém `{2, {3, 1}}, {2, {4, 0}}, {3, {2, 3}}, {3, {2, 4}}, {3, {2, 5}}, {3, {4, 2}}, {3, {3, 3}}`.
- `x_coor = 3`, `y_coor = 1`.
- Marca `(3, 1)` como visitado.
- Verifica os vizinhos:
  - `(4, 1)`: `dist[4][1] = 2 + 0 = 2`, adiciona `{2, {4, 1}}` à fila.
  - `(2, 1)`: já visitado.
  - `(3, 0)`: já visitado.
  - `(3, 2)`: já visitado.

### Décima Nona Iteração:
- `fila` contém `{2, {4, 0}}, {2, {4, 1}}, {3, {2, 3}}, {3, {2, 4}}, {3, {2, 5}}, {3, {4, 2}}, {3, {3, 3}}`.
- `x_coor = 4`, `y_coor = 0`.
- Marca `(4, 0)` como visitado.
- Verifica os vizinhos:
  - `(5, 0)`: `dist[5][0] = 2 + 0 = 2`, adiciona `{2, {5, 0}}` à fila.
  - `(3, 0)`: já visitado.
  - `(4, 1)`: já visitado.

### Vigésima Iteração:
- `fila` contém `{2, {4, 1}}, {2, {5, 0}}, {3, {2, 3}}, {3, {2, 4}}, {3, {2, 5}}, {3, {4, 2}}, {3, {3, 3}}`.
- `x_coor = 4`, `y_coor = 1`.
- Marca `(4, 1)` como visitado.
- Verifica os vizinhos:
  - `(5, 1)`: `dist[5][1] = 2 + 1 = 3`, adiciona `{3, {5, 1}}` à fila.
  - `(3, 1)`: já visitado.
  - `(4, 0)`: já visitado.
  - `(4, 2)`: já visitado.

### Vigésima Primeira Iteração:
- `fila` contém `{2, {5, 0}}, {3, {2, 3}}, {3, {2, 4}}, {3, {2, 5}}, {3, {4, 2}}, {3, {3, 3}}, {3, {5, 1}}`.
- `x_coor = 5`, `y_coor = 0`.
- Marca `(5, 0)` como visitado.
- Verifica os vizinhos:
  - `(5, 1)`: já visitado.
  - `(4, 0)`: já visitado.

### Vigésima Segunda Iteração:
- `fila` contém `{3, {2, 3}}, {3, {2, 4}}, {3, {2, 5}}, {3, {4, 2}}, {3, {3, 3}}, {3, {5, 1}}`.
- `x_coor = 2`, `y_coor = 3`.
- Marca `(2, 3)` como visitado.
- Verifica os vizinhos:
  - `(3, 3)`: já visitado.
  - `(1, 3)`: já visitado.
  - `(2, 2)`: já visitado.
  - `(2, 4)`: já visitado.

### Vigésima Terceira Iteração:
- `fila` contém `{3, {2, 4}}, {3, {2, 5}}, {3, {4, 2}}, {3, {3, 3}}, {3, {5, 1}}`.
- `x_coor = 2`, `y_coor = 4`.
- Marca `(2, 4)` como visitado.
- Verifica os vizinhos:
  - `(3, 4)`: `dist[3][4] = 3 + 1 = 4`, adiciona `{4, {3, 4}}` à fila.
  - `(1, 4)`: já visitado.
  - `(2, 3)`: já visitado.
  - `(2, 5)`: já visitado.

### Vigésima Quarta Iteração:
- `fila` contém `{3, {2, 5}}, {3, {4, 2}}, {3, {3, 3}}, {3, {5, 1}}, {4, {3, 4}}`.
- `x_coor = 2`, `y_coor = 5`.
- Marca `(2, 5)` como visitado.
- Verifica os vizinhos:
  - `(3, 5)`: `dist[3][5] = 3 + 0 = 3`, adiciona `{3, {3, 5}}` à fila.
  - `(1, 5)`: já visitado.
  - `(2, 4)`: já visitado.

### Vigésima Quinta Iteração:
- `fila` contém `{3, {4, 2}}, {3, {3, 3}}, {3, {5, 1}}, {3, {3, 5}}, {4, {3, 4}}`.
- `x_coor = 4`, `y_coor = 2`.
- Marca `(4, 2)` como visitado.
- Verifica os vizinhos:
  - `(5, 2)`: `dist[5][2] = 3 + 0 = 3`, adiciona `{3, {5, 2}}` à fila.
  - `(3, 2)`: já visitado.
  - `(4, 1)`: já visitado.
  - `(4, 3)`: `dist[4][3] = 3 + 1 = 4`, adiciona `{4, {4, 3}}` à fila.

### Vigésima Sexta Iteração:
- `fila` contém `{3, {3, 3}}, {3, {5, 1}}, {3, {3, 5}}, {3, {5, 2}}, {4, {3, 4}}, {4, {4, 3}}`.
- `x_coor = 3`, `y_coor = 3`.
- Marca `(3, 3)` como visitado.
- Verifica os vizinhos:
  - `(4, 3)`: já visitado.
  - `(2, 3)`: já visitado.
  - `(3, 2)`: já visitado.
  - `(3, 4)`: já visitado.

### Vigésima Sétima Iteração:
- `fila` contém `{3, {5, 1}}, {3, {3, 5}}, {3, {5, 2}}, {4, {3, 4}}, {4, {4, 3}}`.
- `x_coor = 5`, `y_coor = 1`.
- Marca `(5, 1)` como visitado.
- Verifica os vizinhos:
  - `(5, 2)`: já visitado.
  - `(4, 1)`: já visitado.
  - `(5, 0)`: já visitado.

### Vigésima Oitava Iteração:
- `fila` contém `{3, {3, 5}}, {3, {5, 2}}, {4, {3, 4}}, {4, {4, 3}}`.
- `x_coor = 3`, `y_coor = 5`.
- Marca `(3, 5)` como visitado.
- Verifica os vizinhos:
  - `(4, 5)`: `dist[4][5] = 3 + 0 = 3`, adiciona `{3, {4, 5}}` à fila.
  - `(2, 5)`: já visitado.
  - `(3, 4)`: já visitado.

### Vigésima Nona Iteração:
- `fila` contém `{3, {5, 2}}, {3, {4, 5}}, {4, {3, 4}}, {4, {4, 3}}`.
- `x_coor = 5`, `y_coor = 2`.
- Marca `(5, 2)` como visitado.
- Verifica os vizinhos:
  - `(5, 3)`: `dist[5][3] = 3 + 0 = 3`, adiciona `{3, {5, 3}}` à fila.
  - `(4, 2)`: já visitado.
  - `(5, 1)`: já visitado.

### Trigésima Iteração:
- `fila` contém `{3, {4, 5}}, {3, {5, 3}}, {4, {3, 4}}, {4, {4, 3}}`.
- `x_coor = 4`, `y_coor = 5`.
- Marca `(4, 5)` como visitado.
- Verifica os vizinhos:
  - `(5, 5)`: `dist[5][5] = 3 + 0 = 3`, adiciona `{3, {5, 5}}` à fila.
  - `(3, 5)`: já visitado.
  - `(4, 4)`: `dist[4][4] = 3 + 1 = 4`, adiciona `{4, {4, 4}}` à fila.

### Trigésima Primeira Iteração:
- `fila` contém `{3, {5, 3}}, {3, {5, 5}}, {4, {3, 4}}, {4, {4, 3}}, {4, {4, 4}}`.
- `x_coor = 5`, `y_coor = 3`.
- Marca `(5, 3)` como visitado.
- Verifica os vizinhos:
  - `(5, 4)`: `dist[5][4] = 3 + 0 = 3`, adiciona `{3, {5, 4}}` à fila.
  - `(4, 3)`: já visitado.
  - `(5, 2)`: já visitado.

### Trigésima Segunda Iteração:
- `fila` contém `{3, {5, 5}}, {3, {5, 4}}, {4, {3, 4}}, {4, {4, 3}}, {4, {4, 4}}`.
- `x_coor = 5`, `y_coor = 5`.
- Marca `(5, 5)` como visitado.
- Verifica os vizinhos:
  - `(4, 5)`: já visitado.
  - `(5, 4)`: já visitado.

### Trigésima Terceira Iteração:
- `fila` contém `{3, {5, 4}}, {4, {3, 4}}, {4, {4, 3}}, {4, {4, 4}}`.
- `x_coor = 5`, `y_coor = 4`.
- Marca `(5, 4)` como visitado.
- Verifica os vizinhos:
  - `(4, 4)`: já visitado.
  - `(5, 3)`: já visitado.
  - `(5, 5)`: já visitado.

### Trigésima Quarta Iteração:
- `fila` contém `{4, {3, 4}}, {4, {4, 3}}, {4, {4, 4}}`.
- `x_coor = 3`, `y_coor = 4`.
- Marca `(3, 4)` como visitado.
- Verifica os vizinhos:
  - `(4, 4)`: já visitado.
  - `(2, 4)`: já visitado.
  - `(3, 3)`: já visitado.
  - `(3, 5)`: já visitado.

### Trigésima Quinta Iteração:
- `fila` contém `{4, {4, 4}}`.
- `x_coor = 4`, `y_coor = 3`.
- Marca `(4, 3)` como visitado.
- Verifica os vizinhos:
  - `(5, 3)`: já visitado.
  - `(3, 3)`: já visitado.
  - `(4, 2)`: já visitado.
  - `(4, 4)`: já visitado.

### Trigésima Sexta Iteração:
- `fila` contém `{4, {4, 4}}`.
- `x_coor = 4`, `y_coor = 4`.
- Marca `(4, 4)` como visitado.
- Verifica os vizinhos:
  - `(5, 4)`: já visitado.
  - `(3, 4)`: já visitado.
  - `(4, 3)`: já visitado.
  - `(4, 5)`: já visitado.

### Saída Final
- `dist[5][5] = 3`
- O custo mínimo para ir de `(0, 0)` a `(5, 5)` é `3`.

### Resumo da Simulação
A simulação completa do algoritmo de Dijkstra usando a matriz fornecida resultou no custo mínimo de `3` para ir do canto superior esquerdo `(0, 0)` ao canto inferior direito `(5, 5)`.

3. **Entrada:** Primeiro, digite o valor de `n` (tamanho da matriz). Em seguida, digite os pesos de cada célula da matriz `n x n`.

4. **Saída:** O programa imprimirá o custo mínimo para ir de `(0,0)` a `(n-1,n-1)`.

## Conclusão

Este algoritmo é uma aplicação eficiente do método de Dijkstra para encontrar caminhos de custo mínimo em matrizes de pesos, útil em várias aplicações de grafos e otimização de rotas.
