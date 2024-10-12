# Aula 09: Estrutura de Dados - Pilha (Implementação Dinâmica)

## Introdução
Nesta aula, veremos a implementação de uma estrutura de dados chamada **Pilha** utilizando uma abordagem dinâmica. A pilha é uma estrutura linear onde:
- As inserções ocorrem no topo da pilha;
- As exclusões ocorrem no topo da pilha.

Esta abordagem utiliza a lógica **LIFO** (Last In, First Out), similar a uma pilha de papéis. Na implementação dinâmica, alocamos e desalocamos a memória para os elementos sob demanda, garantindo um uso eficiente da memória.

## Pilha (Implementação Dinâmica)
Na implementação dinâmica da pilha, cada elemento é alocado conforme necessário e contém um ponteiro para o próximo elemento, formando uma estrutura encadeada.

### Modelagem da Estrutura
Abaixo está a modelagem da estrutura de uma pilha com implementação dinâmica:

```c
#include <stdio.h>
#include <malloc.h>

typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
    // outros campos...
} REGISTRO;

typedef struct aux {
    REGISTRO reg;
    struct aux* prox;
} ELEMENTO;

typedef ELEMENTO* PONT;

typedef struct {
    PONT topo;
} PILHA;
```

- `ELEMENTO`: Representa um nó da pilha, contendo um registro (`reg`) e um ponteiro (`prox`) para o próximo elemento abaixo dele na pilha.
- `PILHA`: Estrutura que contém um ponteiro (`topo`) que aponta para o elemento no topo da pilha.

## Funções de Gerenciamento
### Inicialização da Pilha
Para inicializar uma pilha, definimos o valor do campo `topo` como `NULL`, indicando que a pilha está vazia:

```c
void inicializarPilha(PILHA* p) {
    p->topo = NULL;
}
```

### Retornar Número de Elementos
Como não mantemos um campo com o número de elementos na pilha, precisamos percorrer todos os elementos para contar quantos são:

```c
int tamanho(PILHA* p) {
    PONT end = p->topo;
    int tam = 0;
    while (end != NULL) {
        tam++;
        end = end->prox;
    }
    return tam;
}
```

### Verificar se a Pilha Está Vazia
Podemos verificar se a pilha está vazia verificando se o ponteiro `topo` é `NULL`:

```c
bool estaVazia(PILHA* p) {
    return (p->topo == NULL);
}
```

### Exibir a Pilha
Para exibir os elementos da pilha, iteramos a partir do topo até o final, imprimindo as chaves dos elementos:

```c
void exibirPilha(PILHA* p) {
    PONT end = p->topo;
    printf("Pilha: \" ");
    while (end != NULL) {
        printf("%i ", end->reg.chave);
        end = end->prox;
    }
    printf("\"\n");
}
```

**Exemplo de saída:**
```
Pilha: " 5 9 7 "
```

### Inserção de um Elemento (Push)
Para inserir um elemento no topo da pilha, alocamos memória para um novo nó e ajustamos os ponteiros:

```c
bool inserirElemPilha(PILHA* p, REGISTRO reg) {
    PONT novo = (PONT) malloc(sizeof(ELEMENTO));
    if (novo == NULL) return false; // Falha na alocação
    novo->reg = reg;
    novo->prox = p->topo;
    p->topo = novo;
    return true;
}
```

### Exclusão de um Elemento (Pop)
Para excluir o elemento do topo da pilha, verificamos se a pilha não está vazia e, em seguida, ajustamos o ponteiro `topo` e liberamos a memória do nó removido:

```c
bool excluirElemPilha(PILHA* p, REGISTRO* reg) {
    if (p->topo == NULL) return false;
    *reg = p->topo->reg;
    PONT apagar = p->topo;
    p->topo = p->topo->prox;
    free(apagar);
    return true;
}
```

### Reinicialização da Pilha
Para reinicializar a pilha, precisamos excluir todos os elementos e definir o ponteiro `topo` como `NULL`:

```c
void reinicializarPilha(PILHA* p) {
    PONT apagar;
    PONT posicao = p->topo;
    while (posicao != NULL) {
        apagar = posicao;
        posicao = posicao->prox;
        free(apagar);
    }
    p->topo = NULL;
}
```

## Exemplos de Uso
Abaixo estão exemplos de como as funções podem ser chamadas a partir da função `main`:

```c
#include <stdio.h>
#include <stdlib.h>

typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
} REGISTRO;

typedef struct aux {
    REGISTRO reg;
    struct aux* prox;
} ELEMENTO;

typedef ELEMENTO* PONT;

typedef struct {
    PONT topo;
} PILHA;

void inicializarPilha(PILHA* p) {
    p->topo = NULL;
}

int tamanho(PILHA* p) {
    PONT end = p->topo;
    int tam = 0;
    while (end != NULL) {
        tam++;
        end = end->prox;
    }
    return tam;
}

bool estaVazia(PILHA* p) {
    return (p->topo == NULL);
}

void exibirPilha(PILHA* p) {
    PONT end = p->topo;
    printf("Pilha: \" ");
    while (end != NULL) {
        printf("%i ", end->reg.chave);
        end = end->prox;
    }
    printf("\"\n");
}

bool inserirElemPilha(PILHA* p, REGISTRO reg) {
    PONT novo = (PONT) malloc(sizeof(ELEMENTO));
    if (novo == NULL) return false;
    novo->reg = reg;
    novo->prox = p->topo;
    p->topo = novo;
    return true;
}

bool excluirElemPilha(PILHA* p, REGISTRO* reg) {
    if (p->topo == NULL) return false;
    *reg = p->topo->reg;
    PONT apagar = p->topo;
    p->topo = p->topo->prox;
    free(apagar);
    return true;
}

void reinicializarPilha(PILHA* p) {
    PONT apagar;
    PONT posicao = p->topo;
    while (posicao != NULL) {
        apagar = posicao;
        posicao = posicao->prox;
        free(apagar);
    }
    p->topo = NULL;
}

int main() {
    PILHA pilha;
    inicializarPilha(&pilha);

    REGISTRO reg1 = {5};
    REGISTRO reg2 = {9};
    REGISTRO reg3 = {7};

    // Inserindo elementos na pilha
    inserirElemPilha(&pilha, reg1);  // Inserir 5
    inserirElemPilha(&pilha, reg2);  // Inserir 9
    inserirElemPilha(&pilha, reg3);  // Inserir 7

    // Exibindo a pilha
    exibirPilha(&pilha);  // Saída: Pilha: " 7 9 5 "

    // Retornando o número de elementos
    printf("Número de elementos: %d\n", tamanho(&pilha));  // Saída: Número de elementos: 3

    // Excluindo um elemento
    REGISTRO regExcluido;
    excluirElemPilha(&pilha, &regExcluido);  // Excluir elemento do topo (7)
    printf("Elemento excluído: %d\n", regExcluido.chave);  // Saída: Elemento excluído: 7

    // Exibindo a pilha após exclusão
    exibirPilha(&pilha);  // Saída: Pilha: " 9 5 "

    return 0;
}
```

**Explicação dos Exemplos:**
1. **Inicializar a Pilha:** A função `inicializarPilha` é chamada para definir o valor de `topo` como `NULL`.
2. **Inserção de Elementos (Push):** `inserirElemPilha` adiciona elementos no topo da pilha.
3. **Exibir a Pilha:** `exibirPilha` mostra os elementos atuais da pilha.
4. **Exclusão de Elemento (Pop):** `excluirElemPilha` remove o elemento do topo da pilha e armazena o valor excluído.

Esses exemplos demonstram como manipular uma pilha utilizando a implementação dinâmica, com operações básicas de inserção, exclusão e exibição.