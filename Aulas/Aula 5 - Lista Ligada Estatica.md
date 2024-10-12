# Aula 05: Estrutura de Dados - Lista Ligada (Implementação Estática)

## Introdução
Nesta aula, aprendemos sobre listas ligadas com implementação estática. Diferente das listas lineares sequenciais, a lista ligada é uma estrutura que evita o deslocamento de elementos durante as operações de inserção e exclusão, pois cada elemento aponta para o próximo na estrutura.

## Lista Ligada (Implementação Estática)
A lista ligada é uma estrutura linear onde cada elemento possui, no máximo, um predecessor e um sucessor. A ordem lógica dos elementos não é a mesma ordem física na memória, pois cada elemento contém um campo que indica a posição do seu sucessor.

Na implementação estática, os registros são armazenados em um arranjo, e cada elemento contém um campo para indicar o índice do próximo elemento. Essa abordagem nos permite evitar o custo de deslocar elementos durante as operações de inserção e exclusão.

## Modelagem da Estrutura
Abaixo está a modelagem da estrutura de uma lista ligada com implementação estática:

```c
#define MAX 50
#define INVALIDO -1

typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
    // outros campos...
} REGISTRO;

typedef struct {
    REGISTRO reg;
    int prox;
} ELEMENTO;

typedef struct {
    ELEMENTO A[MAX];
    int inicio;
    int dispo;
} LISTA;
```

- `MAX`: Define o tamanho máximo da lista.
- `INVALIDO`: Define um valor especial para indicar que não há próximo elemento.
- `ELEMENTO`: Cada elemento contém um registro e o índice do próximo elemento na lista.
- `LISTA`: Estrutura que contém um array de elementos (`A`), além dos índices `inicio` e `dispo` para indicar o início da lista e a posição do primeiro elemento disponível, respectivamente.

## Funções de Gerenciamento
As funções principais para gerenciar a lista ligada com implementação estática incluem:

### Inicializar a Lista
Para inicializar a lista, precisamos definir todos os elementos como disponíveis e acertar as variáveis `inicio` e `dispo`:

```c
void inicializarLista(LISTA* l) {
    int i;
    for (i = 0; i < MAX - 1; i++)
        l->A[i].prox = i + 1;
    l->A[MAX - 1].prox = INVALIDO;
    l->inicio = INVALIDO;
    l->dispo = 0;
}
```

### Retornar Número de Elementos
Para contar o número de elementos na lista, percorremos todos os elementos válidos:

```c
int tamanho(LISTA* l) {
    int i = l->inicio;
    int tam = 0;
    while (i != INVALIDO) {
        tam++;
        i = l->A[i].prox;
    }
    return tam;
}
```

### Exibir a Lista
Para exibir os elementos, iteramos sobre todos os elementos válidos e imprimimos suas chaves:

```c
void exibirLista(LISTA* l) {
    int i = l->inicio;
    printf("Lista: \" ");
    while (i != INVALIDO) {
        printf("%i ", l->A[i].reg.chave);
        i = l->A[i].prox;
    }
    printf("\"\n");
}
```

### Busca Sequencial Ordenada
A busca sequencial em uma lista ordenada é realizada para encontrar um elemento ou determinar sua posição de inserção:

```c
int buscaSequencialOrd(LISTA* l, TIPOCHAVE ch) {
    int i = l->inicio;
    while (i != INVALIDO && l->A[i].reg.chave < ch)
        i = l->A[i].prox;
    if (i != INVALIDO && l->A[i].reg.chave == ch)
        return i;
    else
        return INVALIDO;
}
```

### Inserção Ordenada
Para inserir um elemento de forma ordenada, utilizamos uma função auxiliar para obter o primeiro elemento disponível e realizamos os ajustes necessários nos ponteiros:

```c
int obterNo(LISTA* l) {
    int resultado = l->dispo;
    if (l->dispo != INVALIDO)
        l->dispo = l->A[l->dispo].prox;
    return resultado;
}

bool inserirElemListaOrd(LISTA* l, REGISTRO reg) {
    if (l->dispo == INVALIDO) return false;

    int ant = INVALIDO;
    int i = l->inicio;
    TIPOCHAVE ch = reg.chave;

    while ((i != INVALIDO) && (l->A[i].reg.chave < ch)) {
        ant = i;
        i = l->A[i].prox;
    }

    if (i != INVALIDO && l->A[i].reg.chave == ch) return false;

    i = obterNo(l);
    l->A[i].reg = reg;

    if (ant == INVALIDO) {
        l->A[i].prox = l->inicio;
        l->inicio = i;
    } else {
        l->A[i].prox = l->A[ant].prox;
        l->A[ant].prox = i;
    }
    return true;
}
```

### Exclusão de um Elemento
Para excluir um elemento, precisamos ajustar os ponteiros e devolver o nó para a lista de disponíveis:

```c
void devolverNo(LISTA* l, int j) {
    l->A[j].prox = l->dispo;
    l->dispo = j;
}

bool excluirElemLista(LISTA* l, TIPOCHAVE ch) {
    int ant = INVALIDO;
    int i = l->inicio;

    while ((i != INVALIDO) && (l->A[i].reg.chave < ch)) {
        ant = i;
        i = l->A[i].prox;
    }

    if (i == INVALIDO || l->A[i].reg.chave != ch) return false;

    if (ant == INVALIDO)
        l->inicio = l->A[i].prox;
    else
        l->A[ant].prox = l->A[i].prox;

    devolverNo(l, i);
    return true;
}
```

### Reinicializar a Lista
Para reinicializar a lista, basta chamar a função de inicialização:

```c
void reinicializarLista(LISTA* l) {
    inicializarLista(l);
}
```

## Exemplos de Uso
Abaixo estão alguns exemplos de como as funções podem ser chamadas a partir da função `main`, como os parâmetros são passados e os retornos das funções.

```c
#include <stdio.h>
#include <stdbool.h>

#define MAX 50
#define INVALIDO -1

typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
    // outros campos...
} REGISTRO;

typedef struct {
    REGISTRO reg;
    int prox;
} ELEMENTO;

typedef struct {
    ELEMENTO A[MAX];
    int inicio;
    int dispo;
} LISTA;

void inicializarLista(LISTA* l) {
    int i;
    for (i = 0; i < MAX - 1; i++)
        l->A[i].prox = i + 1;
    l->A[MAX - 1].prox = INVALIDO;
    l->inicio = INVALIDO;
    l->dispo = 0;
}

int tamanho(LISTA* l) {
    int i = l->inicio;
    int tam = 0;
    while (i != INVALIDO) {
        tam++;
        i = l->A[i].prox;
    }
    return tam;
}

void exibirLista(LISTA* l) {
    int i = l->inicio;
    printf("Lista: \" ");
    while (i != INVALIDO) {
        printf("%i ", l->A[i].reg.chave);
        i = l->A[i].prox;
    }
    printf("\"\n");
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
1. **Inicializar a Lista:** A função `inicializarLista` é chamada para definir `nroElem` como `0` e inicializar os ponteiros dos elementos.
2. **Inserção Ordenada:** `inserirElemListaOrd` é usada para adicionar elementos em ordem crescente.
3. **Exibir a Lista:** `exibirLista` mostra os elementos atuais da lista.
4. **Excluir um Elemento:** `excluirElemLista` remove um elemento específico da lista.

Esses exemplos mostram como utilizar as funções para manipular a lista ligada de forma eficiente, considerando a implementação estática e o uso de ponteiros para controlar a estrutura.