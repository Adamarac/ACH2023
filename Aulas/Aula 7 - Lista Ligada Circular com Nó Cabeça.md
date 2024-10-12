# Aula 07: Estrutura de Dados - Lista Ligada Circular com Nó Cabeça

## Introdução
Nesta aula, aprendemos sobre listas ligadas circulares com nó cabeça. Este tipo de lista adiciona duas características importantes às listas ligadas:

1. **Lista Circular**: O último elemento aponta para o primeiro, formando um ciclo.
2. **Nó Cabeça**: Há um elemento especial, o nó cabeça, que sempre encabeça a lista e facilita algumas operações.

Essas características permitem maior flexibilidade e simplificam a lógica para certas operações, como a inserção e a exclusão de elementos.

## Modelagem da Estrutura
Abaixo está a modelagem da estrutura de uma lista ligada circular com nó cabeça:

```c
#include <stdio.h>
#include <malloc.h>

typedef int bool;
typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
    // outros campos...
} REGISTRO;

typedef struct tempRegistro {
    REGISTRO reg;
    struct tempRegistro* prox;
} ELEMENTO;

typedef ELEMENTO* PONT;

typedef struct {
    PONT cabeca;
} LISTA;
```

- `ELEMENTO`: Representa um nó da lista, que contém um registro e um ponteiro (`prox`) para o próximo elemento.
- `LISTA`: Estrutura que contém um ponteiro (`cabeca`) que aponta para o nó cabeça.

## Funções de Gerenciamento
### Inicializar a Lista
Para inicializar a lista, precisamos criar o nó cabeça, fazer com que `cabeca` aponte para ele, e que o nó cabeça aponte para ele mesmo como próximo:

```c
void inicializarLista(LISTA* l) {
    l->cabeca = (PONT) malloc(sizeof(ELEMENTO));
    l->cabeca->prox = l->cabeca;
}
```

### Retornar Número de Elementos
Como não armazenamos explicitamente o número de elementos, precisamos percorrer todos os nós para contá-los:

```c
int tamanho(LISTA* l) {
    PONT end = l->cabeca->prox;
    int tam = 0;
    while (end != l->cabeca) {
        tam++;
        end = end->prox;
    }
    return tam;
}
```

### Exibir a Lista
Para exibir os elementos da lista, iteramos sobre todos os elementos válidos (ignorando o nó cabeça) e imprimimos suas chaves:

```c
void exibirLista(LISTA* l) {
    PONT end = l->cabeca->prox;
    printf("Lista: \" ");
    while (end != l->cabeca) {
        printf("%i ", end->reg.chave);
        end = end->prox;
    }
    printf("\"\n");
}
```

### Busca Sequencial
A função de busca percorre a lista e retorna o endereço do elemento que contém a chave, ou `NULL` se o elemento não for encontrado. Utilizamos o nó cabeça como sentinela:

```c
PONT buscaSentinela(LISTA* l, TIPOCHAVE ch) {
    PONT pos = l->cabeca->prox;
    l->cabeca->reg.chave = ch;
    while (pos->reg.chave != ch) pos = pos->prox;
    if (pos != l->cabeca) return pos;
    return NULL;
}
```

### Inserção Ordenada
Para inserir um elemento de forma ordenada, precisamos localizar o ponto correto de inserção e alocar memória para o novo nó:

```c
bool inserirElemListaOrd(LISTA* l, REGISTRO reg) {
    PONT ant, i;
    i = buscaSeqExc(l, reg.chave, &ant);
    if (i != NULL) return false;
    i = (PONT) malloc(sizeof(ELEMENTO));
    i->reg = reg;
    i->prox = ant->prox;
    ant->prox = i;
    return true;
}
```

### Exclusão de um Elemento
Para excluir um elemento, precisamos ajustar os ponteiros e liberar a memória do nó correspondente:

```c
bool excluirElemLista(LISTA* l, TIPOCHAVE ch) {
    PONT ant, i;
    i = buscaSeqExc(l, ch, &ant);
    if (i == NULL) return false;
    ant->prox = i->prox;
    free(i);
    return true;
}
```

### Reinicializar a Lista
Para reinicializar a lista, precisamos excluir todos os elementos válidos e fazer com que o nó cabeça aponte para si mesmo:

```c
void reinicializarLista(LISTA* l) {
    PONT end = l->cabeca->prox;
    while (end != l->cabeca) {
        PONT apagar = end;
        end = end->prox;
        free(apagar);
    }
    l->cabeca->prox = l->cabeca;
}
```

## Exemplos de Uso
Abaixo estão exemplos de como as funções podem ser chamadas a partir da função `main`.

```c
#include <stdio.h>
#include <stdlib.h>

typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
} REGISTRO;

typedef struct tempRegistro {
    REGISTRO reg;
    struct tempRegistro* prox;
} ELEMENTO;

typedef ELEMENTO* PONT;

typedef struct {
    PONT cabeca;
} LISTA;

void inicializarLista(LISTA* l) {
    l->cabeca = (PONT) malloc(sizeof(ELEMENTO));
    l->cabeca->prox = l->cabeca;
}

int tamanho(LISTA* l) {
    PONT end = l->cabeca->prox;
    int tam = 0;
    while (end != l->cabeca) {
        tam++;
        end = end->prox;
    }
    return tam;
}

void exibirLista(LISTA* l) {
    PONT end = l->cabeca->prox;
    printf("Lista: \" ");
    while (end != l->cabeca) {
        printf("%i ", end->reg.chave);
        end = end->prox;
    }
    printf("\"\n");
}

PONT buscaSentinela(LISTA* l, TIPOCHAVE ch) {
    PONT pos = l->cabeca->prox;
    l->cabeca->reg.chave = ch;
    while (pos->reg.chave != ch) pos = pos->prox;
    if (pos != l->cabeca) return pos;
    return NULL;
}

bool inserirElemListaOrd(LISTA* l, REGISTRO reg) {
    PONT ant, i;
    i = buscaSeqExc(l, reg.chave, &ant);
    if (i != NULL) return false;
    i = (PONT) malloc(sizeof(ELEMENTO));
    i->reg = reg;
    i->prox = ant->prox;
    ant->prox = i;
    return true;
}

bool excluirElemLista(LISTA* l, TIPOCHAVE ch) {
    PONT ant, i;
    i = buscaSeqExc(l, ch, &ant);
    if (i == NULL) return false;
    ant->prox = i->prox;
    free(i);
    return true;
}

void reinicializarLista(LISTA* l) {
    PONT end = l->cabeca->prox;
    while (end != l->cabeca) {
        PONT apagar = end;
        end = end->prox;
        free(apagar);
    }
    l->cabeca->prox = l->cabeca;
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
1. **Inicializar a Lista:** A função `inicializarLista` é chamada para definir o nó cabeça e inicializar a lista como circular.
2. **Inserção Ordenada:** Utilizamos `inserirElemListaOrd` para adicionar elementos em ordem crescente.
3. **Exibir a Lista:** `exibirLista` mostra os elementos da lista, ignorando o nó cabeça.
4. **Excluir um Elemento:** `excluirElemLista` remove um elemento específico da lista.

Esses exemplos mostram como utilizar as funções para manipular a lista ligada circular com nó cabeça de forma eficiente.