# Aula 06: Estrutura de Dados - Lista Ligada (Implementação Dinâmica)

## Introdução
Nesta aula, aprenderemos sobre listas ligadas com implementação dinâmica. Diferente da implementação estática que utiliza um arranjo para armazenar os registros, na implementação dinâmica alocamos e desalocamos a memória para os elementos sob demanda. Isso nos permite usar apenas a memória necessária, sem desperdiçar espaço, e elimina a necessidade de gerenciar uma lista de elementos disponíveis.

## Lista Ligada (Implementação Dinâmica)
Na lista ligada dinâmica, cada elemento contém um ponteiro para o próximo elemento, formando uma sequência encadeada. Assim, a ordem lógica dos elementos não corresponde necessariamente à ordem física em memória.

### Modelagem da Estrutura
Abaixo está a modelagem da estrutura de uma lista ligada com implementação dinâmica:

```c
#include <stdio.h>
#include <malloc.h>

typedef int bool;
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
    PONT inicio;
} LISTA;
```

- `ELEMENTO`: Representa um nó da lista, que contém um registro e um ponteiro (`prox`) para o próximo elemento.
- `LISTA`: Estrutura que contém um ponteiro (`inicio`) que aponta para o primeiro elemento da lista.

## Funções de Gerenciamento
### Inicializar a Lista
Para inicializar a lista, definimos o ponteiro `inicio` como `NULL`:

```c
void inicializarLista(LISTA* l) {
    l->inicio = NULL;
}
```

### Retornar Número de Elementos
Como não temos um campo que armazena o número de elementos na lista, precisamos percorrer todos os nós para contar quantos são:

```c
int tamanho(LISTA* l) {
    PONT end = l->inicio;
    int tam = 0;
    while (end != NULL) {
        tam++;
        end = end->prox;
    }
    return tam;
}
```

### Exibir a Lista
Para exibir os elementos da lista, iteramos sobre todos os elementos e imprimimos suas chaves:

```c
void exibirLista(LISTA* l) {
    PONT end = l->inicio;
    printf("Lista: \" ");
    while (end != NULL) {
        printf("%i ", end->reg.chave);
        end = end->prox;
    }
    printf("\"\n");
}
```

### Busca Sequencial
A função de busca percorre a lista e retorna o endereço do elemento que contém a chave, ou `NULL` se o elemento não for encontrado:

```c
PONT buscaSequencial(LISTA* l, TIPOCHAVE ch) {
    PONT pos = l->inicio;
    while (pos != NULL) {
        if (pos->reg.chave == ch) return pos;
        pos = pos->prox;
    }
    return NULL;
}
```

### Inserção Ordenada
Para inserir um elemento de forma ordenada, alocamos memória dinamicamente para o novo nó e ajustamos os ponteiros:

```c
bool inserirElemListaOrd(LISTA* l, REGISTRO reg) {
    TIPOCHAVE ch = reg.chave;
    PONT ant = NULL, i = l->inicio;

    // Encontrar a posição de inserção
    while (i != NULL && i->reg.chave < ch) {
        ant = i;
        i = i->prox;
    }

    // Verificar se o elemento já existe
    if (i != NULL && i->reg.chave == ch) return false;

    // Alocar memória para o novo elemento
    PONT novo = (PONT) malloc(sizeof(ELEMENTO));
    novo->reg = reg;

    // Inserir o novo elemento na lista
    if (ant == NULL) { // Inserção no início
        novo->prox = l->inicio;
        l->inicio = novo;
    } else { // Inserção no meio ou final
        novo->prox = ant->prox;
        ant->prox = novo;
    }
    return true;
}
```

### Exclusão de um Elemento
Para excluir um elemento, ajustamos os ponteiros e liberamos a memória do nó:

```c
bool excluirElemLista(LISTA* l, TIPOCHAVE ch) {
    PONT ant = NULL, i = l->inicio;

    // Encontrar o elemento a ser excluído
    while (i != NULL && i->reg.chave < ch) {
        ant = i;
        i = i->prox;
    }

    // Verificar se o elemento foi encontrado
    if (i == NULL || i->reg.chave != ch) return false;

    // Ajustar os ponteiros para excluir o elemento
    if (ant == NULL) {
        l->inicio = i->prox;
    } else {
        ant->prox = i->prox;
    }
    free(i);
    return true;
}
```

### Reinicializar a Lista
Para reinicializar a lista, precisamos liberar a memória de todos os elementos e definir `inicio` como `NULL`:

```c
void reinicializarLista(LISTA* l) {
    PONT end = l->inicio;
    while (end != NULL) {
        PONT apagar = end;
        end = end->prox;
        free(apagar);
    }
    l->inicio = NULL;
}
```

## Exemplos de Uso
Abaixo estão exemplos de como as funções podem ser utilizadas na função `main`.

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
    PONT inicio;
} LISTA;

void inicializarLista(LISTA* l) {
    l->inicio = NULL;
}

int tamanho(LISTA* l) {
    PONT end = l->inicio;
    int tam = 0;
    while (end != NULL) {
        tam++;
        end = end->prox;
    }
    return tam;
}

void exibirLista(LISTA* l) {
    PONT end = l->inicio;
    printf("Lista: \" ");
    while (end != NULL) {
        printf("%i ", end->reg.chave);
        end = end->prox;
    }
    printf("\"\n");
}

PONT buscaSequencial(LISTA* l, TIPOCHAVE ch) {
    PONT pos = l->inicio;
    while (pos != NULL) {
        if (pos->reg.chave == ch) return pos;
        pos = pos->prox;
    }
    return NULL;
}

bool inserirElemListaOrd(LISTA* l, REGISTRO reg) {
    TIPOCHAVE ch = reg.chave;
    PONT ant = NULL, i = l->inicio;

    // Encontrar a posição de inserção
    while (i != NULL && i->reg.chave < ch) {
        ant = i;
        i = i->prox;
    }

    // Verificar se o elemento já existe
    if (i != NULL && i->reg.chave == ch) return false;

    // Alocar memória para o novo elemento
    PONT novo = (PONT) malloc(sizeof(ELEMENTO));
    novo->reg = reg;

    // Inserir o novo elemento na lista
    if (ant == NULL) { // Inserção no início
        novo->prox = l->inicio;
        l->inicio = novo;
    } else { // Inserção no meio ou final
        novo->prox = ant->prox;
        ant->prox = novo;
    }
    return true;
}

bool excluirElemLista(LISTA* l, TIPOCHAVE ch) {
    PONT ant = NULL, i = l->inicio;

    // Encontrar o elemento a ser excluído
    while (i != NULL && i->reg.chave < ch) {
        ant = i;
        i = i->prox;
    }

    // Verificar se o elemento foi encontrado
    if (i == NULL || i->reg.chave != ch) return false;

    // Ajustar os ponteiros para excluir o elemento
    if (ant == NULL) {
        l->inicio = i->prox;
    } else {
        ant->prox = i->prox;
    }
    free(i);
    return true;
}

void reinicializarLista(LISTA* l) {
    PONT end = l->inicio;
    while (end != NULL) {
        PONT apagar = end;
        end = end->prox;
        free(apagar);
    }
    l->inicio = NULL;
}

int main() {
    LISTA lista;
    inicializarLista(&lista);

    REGISTRO reg1 = {10};
    REGISTRO reg2 = {20};
    REGISTRO reg3 = {15};

    // Inserindo elementos na lista
    inserirElemListaOrd(&lista, reg1);  // Inserir 10
    inserirElemListaOrd(&lista, reg2);  // Inserir 20
    inserirElemListaOrd(&lista, reg3);  // Inserir 15 (entre 10 e 20)

    // Exibindo a lista
    exibirLista(&lista);  // Saída: Lista: " 10 15 20 "

    // Retornando o número de elementos
    printf("Número de elementos: %d\n", tamanho(&lista));  // Saída: Número de elementos: 3

    // Excluindo um elemento
    excluirElemLista(&lista, 15);  // Excluir elemento 15
    exibirLista(&lista);  // Saída: Lista: " 10 20 "

    return 0;
}
```

**Explicação dos Exemplos:**
1. **Inicializar a Lista:** A função `inicializarLista` é chamada para definir o ponteiro `inicio` como `NULL`.
2. **Inserção Ordenada:** Utilizamos `inserirElemListaOrd` para adicionar elementos em ordem crescente.
3. **Exibir a Lista:** `exibirLista` mostra os elementos atuais da lista.
4. **Excluir um Elemento:** `excluirElemLista` remove um elemento específico da lista.

Esses exemplos mostram como utilizar as funções para manipular a lista ligada de forma eficiente, considerando a alocação dinâmica de memória.