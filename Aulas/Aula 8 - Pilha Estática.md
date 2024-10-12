# Aula 08: Estrutura de Dados - Pilha (Implementação Estática)

## Introdução
Nesta aula, veremos a implementação de uma estrutura de dados chamada **Pilha**. A pilha é uma estrutura linear na qual as inserções e exclusões ocorrem sempre no topo, utilizando a mesma lógica de uma pilha de papéis. Isso significa que o último elemento inserido é o primeiro a ser removido, seguindo o princípio **LIFO** (Last In, First Out).

## Pilha (Implementação Estática)
Na implementação estática da pilha, utilizamos um arranjo de tamanho predefinido para armazenar os elementos e um campo `topo` que indica a posição do elemento que está no topo da pilha.

### Modelagem da Estrutura
Abaixo está a modelagem da estrutura de uma pilha com implementação estática:

```c
#include <stdio.h>

#define MAX 50
#define true 1
#define false 0

typedef int bool;
typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
} REGISTRO;

typedef struct {
    REGISTRO A[MAX];
    int topo;
} PILHA;
```

- `MAX`: Define o tamanho máximo da pilha.
- `REGISTRO`: Representa os elementos da pilha.
- `PILHA`: Contém um arranjo de registros (`A`) e um inteiro (`topo`) que indica a posição do elemento no topo da pilha.

## Funções de Gerenciamento
### Inicialização da Pilha
Para inicializar uma pilha já criada pelo usuário, precisamos apenas acertar o valor do campo `topo` como `-1`, indicando que a pilha está vazia:

```c
void inicializarPilha(PILHA* p) {
    p->topo = -1;
}
```

### Retornar Número de Elementos
Já que o campo `topo` contém a posição no arranjo do elemento no topo da pilha, o número de elementos é igual a `topo + 1`. Para a pilha vazia, `topo` será `-1`, o que torna a lógica consistente:

```c
int tamanhoPilha(PILHA* p) {
    return p->topo + 1;
}
```

### Exibição dos Elementos
Para exibir os elementos da pilha, iteramos do topo até o início, imprimindo as chaves dos elementos:

```c
void exibirPilha(PILHA* p) {
    printf("Pilha: \" ");
    for (int i = p->topo; i >= 0; i--) {
        printf("%i ", p->A[i].chave);
    }
    printf("\"\n");
}
```

**Exemplo de saída:**
```
Pilha: " 7 2 5 "
```

### Inserção de um Elemento (Push)
O usuário passa como parâmetro um registro a ser inserido na pilha. Se a pilha não estiver cheia, o elemento será inserido no topo da pilha:

```c
bool inserirElementoPilha(PILHA* p, REGISTRO reg) {
    if (p->topo >= MAX - 1) return false;
    p->topo = p->topo + 1;
    p->A[p->topo] = reg;
    return true;
}
```

### Exclusão de um Elemento (Pop)
Para excluir um elemento do topo da pilha, verificamos se a pilha não está vazia. Se não estiver, removemos o elemento do topo e retornamos o registro excluído:

```c
bool excluirElementoPilha(PILHA* p, REGISTRO* reg) {
    if (p->topo == -1) return false;
    *reg = p->A[p->topo];
    p->topo = p->topo - 1;
    return true;
}
```

### Reinicialização da Pilha
Para reinicializar a pilha, basta definir o valor do campo `topo` como `-1`:

```c
void reinicializarPilha(PILHA* p) {
    p->topo = -1;
}
```

## Exemplos de Uso
Abaixo estão exemplos de como as funções podem ser chamadas a partir da função `main`:

```c
#include <stdio.h>
#define MAX 50
#define true 1
#define false 0

typedef int bool;
typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
} REGISTRO;

typedef struct {
    REGISTRO A[MAX];
    int topo;
} PILHA;

void inicializarPilha(PILHA* p) {
    p->topo = -1;
}

int tamanhoPilha(PILHA* p) {
    return p->topo + 1;
}

void exibirPilha(PILHA* p) {
    printf("Pilha: \" ");
    for (int i = p->topo; i >= 0; i--) {
        printf("%i ", p->A[i].chave);
    }
    printf("\"\n");
}

bool inserirElementoPilha(PILHA* p, REGISTRO reg) {
    if (p->topo >= MAX - 1) return false;
    p->topo = p->topo + 1;
    p->A[p->topo] = reg;
    return true;
}

bool excluirElementoPilha(PILHA* p, REGISTRO* reg) {
    if (p->topo == -1) return false;
    *reg = p->A[p->topo];
    p->topo = p->topo - 1;
    return true;
}

void reinicializarPilha(PILHA* p) {
    p->topo = -1;
}

int main() {
    PILHA pilha;
    inicializarPilha(&pilha);

    REGISTRO reg1 = {7};
    REGISTRO reg2 = {2};
    REGISTRO reg3 = {5};

    // Inserindo elementos na pilha
    inserirElementoPilha(&pilha, reg1);  // Inserir 7
    inserirElementoPilha(&pilha, reg2);  // Inserir 2
    inserirElementoPilha(&pilha, reg3);  // Inserir 5

    // Exibindo a pilha
    exibirPilha(&pilha);  // Saída: Pilha: " 7 2 5 "

    // Retornando o número de elementos
    printf("Número de elementos: %d\n", tamanhoPilha(&pilha));  // Saída: Número de elementos: 3

    // Excluindo um elemento
    REGISTRO regExcluido;
    excluirElementoPilha(&pilha, &regExcluido);  // Excluir elemento do topo (5)
    printf("Elemento excluído: %d\n", regExcluido.chave);  // Saída: Elemento excluído: 5

    // Exibindo a pilha após exclusão
    exibirPilha(&pilha);  // Saída: Pilha: " 7 2 "

    return 0;
}
```

**Explicação dos Exemplos:**
1. **Inicializar a Pilha:** A função `inicializarPilha` é chamada para definir o valor de `topo` como `-1`.
2. **Inserção de Elementos (Push):** `inserirElementoPilha` adiciona elementos no topo da pilha.
3. **Exibir a Pilha:** `exibirPilha` mostra os elementos atuais da pilha.
4. **Exclusão de Elemento (Pop):** `excluirElementoPilha` remove o elemento do topo da pilha e armazena o valor excluído.

Esses exemplos demonstram como manipular uma pilha utilizando implementação estática, com operações básicas de inserção, exclusão e exibição.