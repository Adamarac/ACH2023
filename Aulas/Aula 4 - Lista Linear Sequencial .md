# Aula 04: Estrutura de Dados - Lista Linear Sequencial (Continuação)

## Introdução
Nesta aula, daremos continuidade ao estudo da lista linear sequencial. Revisamos que utilizamos um arranjo para armazenar os registros e que a inserção era feita na posição indicada pelo usuário. Agora, vamos abordar dois novos aspectos:

1. Otimização da busca por elementos.
2. Mudança na ordem de inserção dos elementos.

## Busca por Elementos
Anteriormente, a busca por elementos na lista linear sequencial era feita da seguinte forma:

### Busca Sequencial (Versão Inicial)
Esta é a versão básica da busca sequencial, onde o usuário fornece uma chave e a função retorna a posição do elemento que possui essa chave, ou `-1` se o elemento não for encontrado.

```c
int buscaSequencial(LISTA* l, TIPOCHAVE ch) {
    int i = 0;
    while (i < l->nroElem) {
        if (ch == l->A[i].chave) return i;
        else i++;
    }
    return -1;
}
```

### Otimização da Busca: Elemento Sentinela
Para otimizar a busca, utilizamos um **elemento sentinela**. Esse elemento é um registro extra que é adicionado ao final da lista e contém a chave do elemento que estamos buscando. Isso elimina a necessidade de fazer duas comparações em cada iteração, garantindo que a chave buscada será encontrada.

**Problema com Lista Cheia:** Se a lista já estiver cheia, não haverá espaço para o sentinela. Portanto, criamos a lista com uma posição extra para garantir esse espaço.

**Versão com Sentinela:**
```c
int buscaSentinela(LISTA* l, TIPOCHAVE ch) {
    int i = 0;
    l->A[l->nroElem].chave = ch;
    while (l->A[i].chave != ch) i++;
    if (i == l->nroElem) return -1;
    else return i;
}
```

## Inserção de Elementos - Ordenada
Para possibilitar a busca binária, precisamos que os elementos da lista estejam ordenados. Para isso, mudamos nossa função de inserção para seguir a lógica do **insertion sort**, inserindo os elementos de forma ordenada.

```c
bool inserirElemListaOrd(LISTA* l, REGISTRO reg) {
    if (l->nroElem >= MAX) return false;
    int pos = l->nroElem;
    while (pos > 0 && l->A[pos-1].chave > reg.chave) {
        l->A[pos] = l->A[pos-1];
        pos--;
    }
    l->A[pos] = reg;
    l->nroElem++;
    return true;
}
```

## Busca Binária
A **busca binária** é mais eficiente do que a busca sequencial, mas requer que as chaves dos elementos estejam ordenadas. Abaixo está a implementação da busca binária na lista linear sequencial:

```c
int buscaBinaria(LISTA* l, TIPOCHAVE ch) {
    int esq, dir, meio;
    esq = 0;
    dir = l->nroElem - 1;
    while (esq <= dir) {
        meio = (esq + dir) / 2;
        if (l->A[meio].chave == ch) return meio;
        else {
            if (l->A[meio].chave < ch) esq = meio + 1;
            else dir = meio - 1;
        }
    }
    return -1;
}
```

## Exclusão de Elementos
A exclusão de elementos também foi adaptada para trabalhar com a lista ordenada. Caso o elemento seja encontrado, todos os elementos subsequentes são deslocados para preencher a lacuna.

### Exclusão de Elemento com Busca Binária
```c
bool excluirElemLista(LISTA* l, TIPOCHAVE ch) {
    int pos, j;
    pos = buscaBinaria(l, ch);
    if (pos == -1) return false;
    for (j = pos; j < l->nroElem - 1; j++) l->A[j] = l->A[j+1];
    l->nroElem--;
    return true;
}
```

## Modelagem da Estrutura Atualizada
A estrutura da lista foi ajustada para acomodar o elemento sentinela, adicionando uma posição extra ao array:

```c
#define MAX 50

typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
    // outros campos...
} REGISTRO;

typedef struct {
    REGISTRO A[MAX + 1];  // Uma posição extra para o sentinela
    int nroElem;
} LISTA;
```

## Exemplos de Uso
Abaixo estão exemplos de como utilizar as funções discutidas nesta aula, dentro de uma função `main`.

```c
#include <stdio.h>
#include <stdbool.h>

#define MAX 50

typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
    // outros campos...
} REGISTRO;

typedef struct {
    REGISTRO A[MAX + 1];  // Uma posição extra para o sentinela
    int nroElem;
} LISTA;

void inicializarLista(LISTA* l) {
    l->nroElem = 0;
}

bool inserirElemListaOrd(LISTA* l, REGISTRO reg) {
    if (l->nroElem >= MAX) return false;
    int pos = l->nroElem;
    while (pos > 0 && l->A[pos-1].chave > reg.chave) {
        l->A[pos] = l->A[pos-1];
        pos--;
    }
    l->A[pos] = reg;
    l->nroElem++;
    return true;
}

int buscaSentinela(LISTA* l, TIPOCHAVE ch) {
    int i = 0;
    l->A[l->nroElem].chave = ch;
    while (l->A[i].chave != ch) i++;
    if (i == l->nroElem) return -1;
    else return i;
}

int buscaBinaria(LISTA* l, TIPOCHAVE ch) {
    int esq, dir, meio;
    esq = 0;
    dir = l->nroElem - 1;
    while (esq <= dir) {
        meio = (esq + dir) / 2;
        if (l->A[meio].chave == ch) return meio;
        else {
            if (l->A[meio].chave < ch) esq = meio + 1;
            else dir = meio - 1;
        }
    }
    return -1;
}

bool excluirElemLista(LISTA* l, TIPOCHAVE ch) {
    int pos, j;
    pos = buscaBinaria(l, ch);
    if (pos == -1) return false;
    for (j = pos; j < l->nroElem - 1; j++) l->A[j] = l->A[j+1];
    l->nroElem--;
    return true;
}

int main() {
    LISTA lista;
    inicializarLista(&lista);

    REGISTRO reg1 = {15};
    REGISTRO reg2 = {7};
    REGISTRO reg3 = {30};

    // Inserindo elementos na lista (de forma ordenada)
    inserirElemListaOrd(&lista, reg1);  // Inserir 15
    inserirElemListaOrd(&lista, reg2);  // Inserir 7
    inserirElemListaOrd(&lista, reg3);  // Inserir 30

    // Exibindo a lista após inserções
    printf("Lista após inserções: \" ");
    for (int i = 0; i < lista.nroElem; i++) {
        printf("%d ", lista.A[i].chave);
    }
    printf("\"\n");

    // Buscando elemento usando busca binária
    int pos = buscaBinaria(&lista, 15);
    if (pos != -1) {
        printf("Elemento 15 encontrado na posição: %d\n", pos);
    } else {
        printf("Elemento 15 não encontrado\n");
    }

    // Excluindo um elemento
    excluirElemLista(&lista, 7);  // Excluir elemento 7
    printf("Lista após exclusão: \" ");
    for (int i = 0; i < lista.nroElem; i++) {
        printf("%d ", lista.A[i].chave);
    }
    printf("\"\n");

    return 0;
}
```

**Explicação dos Exemplos:**
1. **Inicializar a Lista:** A função `inicializarLista` é chamada para definir `nroElem` como `0`.
2. **Inserção Ordenada:** Utiliza `inserirElemListaOrd` para inserir elementos de forma ordenada.
3. **Busca Binária:** A função `buscaBinaria` é usada para encontrar um elemento na lista ordenada.
4. **Exclusão de Elemento:** `excluirElemLista` remove um elemento específico da lista.

Esses exemplos mostram como as funções implementadas podem ser utilizadas para manipular a lista linear sequencial de maneira eficiente, considerando a otimização da busca e a ordenação dos elementos.
