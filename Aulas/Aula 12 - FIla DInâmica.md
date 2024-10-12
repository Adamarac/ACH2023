# Aula 12: Estrutura de Dados - Fila (Implementação Dinâmica)

## Introdução
Nesta aula, veremos a implementação de uma estrutura de dados chamada **Fila** utilizando uma abordagem dinâmica. A fila é uma estrutura linear onde:
- As inserções ocorrem no final da fila;
- As exclusões ocorrem no início da fila.

Ela segue o princípio **FIFO** (First In, First Out), semelhante a uma fila de pessoas, onde o primeiro a entrar é o primeiro a sair.

## Fila (Implementação Dinâmica)
Na implementação dinâmica da fila, alocamos e desalocamos memória para os elementos conforme necessário. Cada elemento contém um ponteiro para o próximo, e a estrutura da fila mantém ponteiros para o início e para o fim da fila.

### Modelagem da Estrutura
Abaixo está a modelagem da estrutura de uma fila com implementação dinâmica:

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
} ELEMENTO, *PONT;

typedef struct {
    PONT inicio;
    PONT fim;
} FILA;
```

- `ELEMENTO`: Representa um nó da fila, contendo um registro (`reg`) e um ponteiro (`prox`) para o próximo elemento.
- `FILA`: Estrutura que contém ponteiros (`inicio` e `fim`) para o início e o fim da fila, facilitando as operações de inserção e exclusão.

## Funções de Gerenciamento
### Inicialização da Fila
Para inicializar a fila, precisamos definir os ponteiros `inicio` e `fim` como `NULL`, indicando que a fila está vazia:

```c
void inicializarFila(FILA* f) {
    f->inicio = NULL;
    f->fim = NULL;
}
```

### Retornar Número de Elementos
Como não mantemos um campo com o número de elementos na fila, precisamos percorrer todos os nós para contar quantos são:

```c
int tamanho(FILA* f) {
    PONT end = f->inicio;
    int tam = 0;
    while (end != NULL) {
        tam++;
        end = end->prox;
    }
    return tam;
}
```

### Exibir a Fila
Para exibir os elementos da fila, percorremos do início até o fim, imprimindo as chaves dos elementos:

```c
void exibirFila(FILA* f) {
    PONT end = f->inicio;
    printf("Fila: \" ");
    while (end != NULL) {
        printf("%i ", end->reg.chave);
        end = end->prox;
    }
    printf("\"\n");
}
```

**Exemplo de saída:**
```
Fila: " 10 20 30 "
```

### Inserção de um Elemento
Para inserir um elemento no final da fila, alocamos memória para o novo nó e ajustamos os ponteiros envolvidos:

```c
bool inserirNaFila(FILA* f, REGISTRO reg) {
    PONT novo = (PONT) malloc(sizeof(ELEMENTO));
    if (novo == NULL) return false; // Falha na alocação
    novo->reg = reg;
    novo->prox = NULL;
    if (f->inicio == NULL) {
        f->inicio = novo;
    } else {
        f->fim->prox = novo;
    }
    f->fim = novo;
    return true;
}
```

### Exclusão de um Elemento
Para excluir um elemento do início da fila, verificamos se a fila não está vazia e ajustamos os ponteiros para remover o elemento:

```c
bool excluirDaFila(FILA* f, REGISTRO* reg) {
    if (f->inicio == NULL) return false; // Fila vazia
    *reg = f->inicio->reg;
    PONT apagar = f->inicio;
    f->inicio = f->inicio->prox;
    if (f->inicio == NULL) f->fim = NULL; // Fila ficou vazia
    free(apagar);
    return true;
}
```

### Reinicialização da Fila
Para reinicializar a fila, precisamos excluir todos os elementos e definir os ponteiros `inicio` e `fim` como `NULL`:

```c
void reinicializarFila(FILA* f) {
    PONT end = f->inicio;
    while (end != NULL) {
        PONT apagar = end;
        end = end->prox;
        free(apagar);
    }
    f->inicio = NULL;
    f->fim = NULL;
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
} ELEMENTO, *PONT;

typedef struct {
    PONT inicio;
    PONT fim;
} FILA;

void inicializarFila(FILA* f) {
    f->inicio = NULL;
    f->fim = NULL;
}

int tamanho(FILA* f) {
    PONT end = f->inicio;
    int tam = 0;
    while (end != NULL) {
        tam++;
        end = end->prox;
    }
    return tam;
}

void exibirFila(FILA* f) {
    PONT end = f->inicio;
    printf("Fila: \" ");
    while (end != NULL) {
        printf("%i ", end->reg.chave);
        end = end->prox;
    }
    printf("\"\n");
}

bool inserirNaFila(FILA* f, REGISTRO reg) {
    PONT novo = (PONT) malloc(sizeof(ELEMENTO));
    if (novo == NULL) return false;
    novo->reg = reg;
    novo->prox = NULL;
    if (f->inicio == NULL) {
        f->inicio = novo;
    } else {
        f->fim->prox = novo;
    }
    f->fim = novo;
    return true;
}

bool excluirDaFila(FILA* f, REGISTRO* reg) {
    if (f->inicio == NULL) return false;
    *reg = f->inicio->reg;
    PONT apagar = f->inicio;
    f->inicio = f->inicio->prox;
    if (f->inicio == NULL) f->fim = NULL;
    free(apagar);
    return true;
}

void reinicializarFila(FILA* f) {
    PONT end = f->inicio;
    while (end != NULL) {
        PONT apagar = end;
        end = end->prox;
        free(apagar);
    }
    f->inicio = NULL;
    f->fim = NULL;
}

int main() {
    FILA fila;
    inicializarFila(&fila);

    REGISTRO reg1 = {10};
    REGISTRO reg2 = {20};
    REGISTRO reg3 = {30};

    // Inserindo elementos na fila
    inserirNaFila(&fila, reg1);  // Inserir 10
    inserirNaFila(&fila, reg2);  // Inserir 20
    inserirNaFila(&fila, reg3);  // Inserir 30

    // Exibindo a fila
    exibirFila(&fila);  // Saída: Fila: " 10 20 30 "

    // Retornando o número de elementos
    printf("Número de elementos: %d\n", tamanho(&fila));  // Saída: Número de elementos: 3

    // Excluindo um elemento
    REGISTRO regExcluido;
    excluirDaFila(&fila, &regExcluido);  // Excluir elemento do início (10)
    printf("Elemento excluído: %d\n", regExcluido.chave);  // Saída: Elemento excluído: 10

    // Exibindo a fila após exclusão
    exibirFila(&fila);  // Saída: Fila: " 20 30 "

    return 0;
}
```

**Explicação dos Exemplos:**
1. **Inicializar a Fila:** A função `inicializarFila` é chamada para definir os ponteiros `inicio` e `fim` como `NULL`.
2. **Inserção de Elementos:** `inserirNaFila` adiciona elementos no final da fila.
3. **Exibir a Fila:** `exibirFila` mostra os elementos atuais da fila.
4. **Exclusão de Elemento:** `excluirDaFila` remove o elemento do início da fila e armazena o valor excluído.

Esses exemplos demonstram como manipular uma fila utilizando a implementação dinâmica, com operações básicas de inserção, exclusão e exibição.