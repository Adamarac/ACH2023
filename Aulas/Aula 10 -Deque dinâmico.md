# Aula 10: Estrutura de Dados - Deque (Implementação Dinâmica)

## Introdução
Nesta aula, aprenderemos sobre a estrutura de dados **Deque** (Double-Ended Queue). O Deque é uma estrutura de dados que permite inserções e remoções em ambas as extremidades (início e fim). Utilizaremos uma implementação duplamente ligada (ou duplamente encadeada), onde cada elemento possui ponteiros para o seu antecessor e sucessor. Também utilizaremos um nó cabeça para facilitar o gerenciamento da estrutura.

## Deque (Implementação Dinâmica)
Na implementação dinâmica do deque, cada elemento é um nó que contém o registro e dois ponteiros: um para o elemento anterior e outro para o próximo. O nó cabeça é um elemento especial que facilita a inserção e remoção de elementos.

### Modelagem da Estrutura
Abaixo está a modelagem da estrutura de um deque com implementação dinâmica:

```c
#include <stdio.h>
#include <malloc.h>

typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
    // outros campos...
} REGISTRO;

typedef struct auxElem {
    REGISTRO reg;
    struct auxElem* ant;
    struct auxElem* prox;
} ELEMENTO;

typedef ELEMENTO* PONT;

typedef struct {
    PONT cabeca;
} DEQUE;
```

- `ELEMENTO`: Representa um nó do deque, contendo um registro (`reg`), um ponteiro para o elemento anterior (`ant`) e um ponteiro para o próximo (`prox`).
- `DEQUE`: Estrutura que contém um ponteiro (`cabeca`) para o nó cabeça, que facilita o gerenciamento da estrutura.

## Funções de Gerenciamento
### Inicialização do Deque
Para inicializar o deque, precisamos criar o nó cabeça, fazer com que o ponteiro `cabeca` aponte para ele, e que o nó cabeça aponte para ele mesmo como anterior e próximo:

```c
void inicializarDeque(DEQUE* d) {
    d->cabeca = (PONT) malloc(sizeof(ELEMENTO));
    d->cabeca->prox = d->cabeca;
    d->cabeca->ant = d->cabeca;
}
```

### Retornar Número de Elementos
Para retornar o número de elementos, percorremos todos os nós válidos do deque (ignorando o nó cabeça):

```c
int tamanho(DEQUE* d) {
    PONT end = d->cabeca->prox;
    int tam = 0;
    while (end != d->cabeca) {
        tam++;
        end = end->prox;
    }
    return tam;
}
```

### Exibir o Deque
Para exibir os elementos do deque, podemos percorrer os elementos do início para o fim ou do fim para o início, ignorando o nó cabeça:

```c
void exibirDequeFim(DEQUE* d) {
    PONT end = d->cabeca->ant;
    printf("Deque partindo do fim: \" ");
    while (end != d->cabeca) {
        printf("%i ", end->reg.chave);
        end = end->ant;
    }
    printf("\"\n");
}
```

### Inserção de um Elemento no Fim
Para inserir um elemento no fim do deque, alocamos memória para o novo nó e ajustamos os ponteiros envolvidos:

```c
bool inserirDequeFim(DEQUE* d, REGISTRO reg) {
    PONT novo = (PONT) malloc(sizeof(ELEMENTO));
    if (novo == NULL) return false;
    novo->reg = reg;
    novo->prox = d->cabeca;
    novo->ant = d->cabeca->ant;
    d->cabeca->ant->prox = novo;
    d->cabeca->ant = novo;
    return true;
}
```

### Exclusão de um Elemento no Início
Para excluir um elemento do início do deque, verificamos se o deque não está vazio e ajustamos os ponteiros para remover o elemento:

```c
bool excluirElemDequeInicio(DEQUE* d, REGISTRO* reg) {
    if (d->cabeca->prox == d->cabeca) return false; // Deque vazio
    PONT apagar = d->cabeca->prox;
    *reg = apagar->reg;
    d->cabeca->prox = apagar->prox;
    apagar->prox->ant = d->cabeca;
    free(apagar);
    return true;
}
```

### Reinicialização do Deque
Para reinicializar o deque, precisamos excluir todos os elementos e ajustar os ponteiros do nó cabeça:

```c
void reinicializarDeque(DEQUE* d) {
    PONT end = d->cabeca->prox;
    while (end != d->cabeca) {
        PONT apagar = end;
        end = end->prox;
        free(apagar);
    }
    d->cabeca->prox = d->cabeca;
    d->cabeca->ant = d->cabeca;
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

typedef struct auxElem {
    REGISTRO reg;
    struct auxElem* ant;
    struct auxElem* prox;
} ELEMENTO;

typedef ELEMENTO* PONT;

typedef struct {
    PONT cabeca;
} DEQUE;

void inicializarDeque(DEQUE* d) {
    d->cabeca = (PONT) malloc(sizeof(ELEMENTO));
    d->cabeca->prox = d->cabeca;
    d->cabeca->ant = d->cabeca;
}

int tamanho(DEQUE* d) {
    PONT end = d->cabeca->prox;
    int tam = 0;
    while (end != d->cabeca) {
        tam++;
        end = end->prox;
    }
    return tam;
}

void exibirDequeFim(DEQUE* d) {
    PONT end = d->cabeca->ant;
    printf("Deque partindo do fim: \" ");
    while (end != d->cabeca) {
        printf("%i ", end->reg.chave);
        end = end->ant;
    }
    printf("\"\n");
}

bool inserirDequeFim(DEQUE* d, REGISTRO reg) {
    PONT novo = (PONT) malloc(sizeof(ELEMENTO));
    if (novo == NULL) return false;
    novo->reg = reg;
    novo->prox = d->cabeca;
    novo->ant = d->cabeca->ant;
    d->cabeca->ant->prox = novo;
    d->cabeca->ant = novo;
    return true;
}

bool excluirElemDequeInicio(DEQUE* d, REGISTRO* reg) {
    if (d->cabeca->prox == d->cabeca) return false;
    PONT apagar = d->cabeca->prox;
    *reg = apagar->reg;
    d->cabeca->prox = apagar->prox;
    apagar->prox->ant = d->cabeca;
    free(apagar);
    return true;
}

void reinicializarDeque(DEQUE* d) {
    PONT end = d->cabeca->prox;
    while (end != d->cabeca) {
        PONT apagar = end;
        end = end->prox;
        free(apagar);
    }
    d->cabeca->prox = d->cabeca;
    d->cabeca->ant = d->cabeca;
}

int main() {
    DEQUE deque;
    inicializarDeque(&deque);

    REGISTRO reg1 = {10};
    REGISTRO reg2 = {20};
    REGISTRO reg3 = {30};

    // Inserindo elementos no fim do deque
    inserirDequeFim(&deque, reg1);  // Inserir 10
    inserirDequeFim(&deque, reg2);  // Inserir 20
    inserirDequeFim(&deque, reg3);  // Inserir 30

    // Exibindo o deque a partir do fim
    exibirDequeFim(&deque);  // Saída: Deque partindo do fim: " 30 20 10 "

    // Retornando o número de elementos
    printf("Número de elementos: %d\n", tamanho(&deque));  // Saída: Número de elementos: 3

    // Excluindo um elemento do início
    REGISTRO regExcluido;
    excluirElemDequeInicio(&deque, &regExcluido);  // Excluir elemento do início (10)
    printf("Elemento excluído: %d\n", regExcluido.chave);  // Saída: Elemento excluído: 10

    // Exibindo o deque após exclusão
    exibirDequeFim(&deque);  // Saída: Deque partindo do fim: " 30 20 "

    return 0;
}
```

**Explicação dos Exemplos:**
1. **Inicializar o Deque:** A função `inicializarDeque` é chamada para definir o nó cabeça e inicializar o deque como vazio.
2. **Inserção no Fim:** Utilizamos `inserirDequeFim` para adicionar elementos no final do deque.
3. **Exibir o Deque:** `exibirDequeFim` mostra os elementos atuais do deque a partir do fim.
4. **Exclusão no Início:** `excluirElemDequeInicio` remove o elemento do início do deque e armazena o valor excluído.

Esses exemplos mostram como manipular o deque utilizando a implementação dinâmica, com operações básicas de inserção, exclusão e exibição.