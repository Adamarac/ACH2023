# Aula 11: Estrutura de Dados - Fila (Implementação Estática)

## Introdução
Nesta aula, aprenderemos sobre a estrutura de dados **Fila** utilizando uma abordagem de implementação estática. A fila é uma estrutura linear que segue o princípio **FIFO** (First In, First Out), onde:

- As inserções ocorrem no final da fila;
- As exclusões ocorrem no início da fila.

Ela utiliza a mesma lógica de uma fila de pessoas, onde o primeiro a entrar é o primeiro a sair.

## Fila (Implementação Estática)
Na implementação estática da fila, utilizamos um arranjo de elementos de tamanho predefinido e controlamos a posição do elemento que está no início da fila, bem como o número de elementos presentes na fila.

### Modelagem da Estrutura
Abaixo está a modelagem da estrutura de uma fila com implementação estática:

```c
#include <stdio.h>

#define MAX 50

typedef int bool;
typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
} REGISTRO;

typedef struct {
    REGISTRO A[MAX];
    int inicio;
    int nroElem;
} FILA;
```

- `MAX`: Define o tamanho máximo da fila.
- `REGISTRO`: Representa os elementos da fila.
- `FILA`: Contém um arranjo de registros (`A`), um inteiro (`inicio`) que indica a posição do primeiro elemento da fila e um inteiro (`nroElem`) que indica o número de elementos na fila.

## Funções de Gerenciamento
### Inicialização da Fila
Para inicializar a fila, precisamos definir o valor de `inicio` como `0` e `nroElem` como `0`, indicando que a fila está vazia:

```c
void inicializarFila(FILA* f) {
    f->inicio = 0;
    f->nroElem = 0;
}
```

### Retornar Número de Elementos
Para retornar o número de elementos da fila, basta retornar o valor do campo `nroElem`:

```c
int tamanhoFila(FILA* f) {
    return f->nroElem;
}
```

### Exibir a Fila
Para exibir os elementos da fila, iteramos pelos elementos válidos, considerando que o arranjo é tratado como circular:

```c
void exibirFila(FILA* f) {
    printf("Fila: \" ");
    int i = f->inicio;
    for (int temp = 0; temp < f->nroElem; temp++) {
        printf("%i ", f->A[i].chave);
        i = (i + 1) % MAX;
    }
    printf("\"\n");
}
```

**Exemplo de saída:**
```
Fila: " 5 7 2 8 "
```

### Inserção de um Elemento
Para inserir um elemento no final da fila, verificamos se a fila não está cheia e então identificamos a posição no arranjo onde o registro será inserido:

```c
bool inserirElementoFila(FILA* f, REGISTRO reg) {
    if (f->nroElem >= MAX) return false;
    int posicao = (f->inicio + f->nroElem) % MAX;
    f->A[posicao] = reg;
    f->nroElem++;
    return true;
}
```

### Exclusão de um Elemento
Para excluir um elemento do início da fila, verificamos se a fila não está vazia e ajustamos o valor de `inicio` e `nroElem`:

```c
bool excluirElementoFila(FILA* f, REGISTRO* reg) {
    if (f->nroElem == 0) return false;
    *reg = f->A[f->inicio];
    f->inicio = (f->inicio + 1) % MAX;
    f->nroElem--;
    return true;
}
```

### Reinicialização da Fila
Para reinicializar a fila, podemos simplesmente chamar a função de inicialização novamente ou executar os mesmos comandos:

```c
void reinicializarFila(FILA* f) {
    f->inicio = 0;
    f->nroElem = 0;
}
```

## Exemplos de Uso
Abaixo estão exemplos de como as funções podem ser chamadas a partir da função `main`:

```c
#include <stdio.h>
#define MAX 50

typedef int bool;
typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
} REGISTRO;

typedef struct {
    REGISTRO A[MAX];
    int inicio;
    int nroElem;
} FILA;

void inicializarFila(FILA* f) {
    f->inicio = 0;
    f->nroElem = 0;
}

int tamanhoFila(FILA* f) {
    return f->nroElem;
}

void exibirFila(FILA* f) {
    printf("Fila: \" ");
    int i = f->inicio;
    for (int temp = 0; temp < f->nroElem; temp++) {
        printf("%i ", f->A[i].chave);
        i = (i + 1) % MAX;
    }
    printf("\"\n");
}

bool inserirElementoFila(FILA* f, REGISTRO reg) {
    if (f->nroElem >= MAX) return false;
    int posicao = (f->inicio + f->nroElem) % MAX;
    f->A[posicao] = reg;
    f->nroElem++;
    return true;
}

bool excluirElementoFila(FILA* f, REGISTRO* reg) {
    if (f->nroElem == 0) return false;
    *reg = f->A[f->inicio];
    f->inicio = (f->inicio + 1) % MAX;
    f->nroElem--;
    return true;
}

void reinicializarFila(FILA* f) {
    f->inicio = 0;
    f->nroElem = 0;
}

int main() {
    FILA fila;
    inicializarFila(&fila);

    REGISTRO reg1 = {5};
    REGISTRO reg2 = {7};
    REGISTRO reg3 = {2};
    REGISTRO reg4 = {8};

    // Inserindo elementos na fila
    inserirElementoFila(&fila, reg1);  // Inserir 5
    inserirElementoFila(&fila, reg2);  // Inserir 7
    inserirElementoFila(&fila, reg3);  // Inserir 2
    inserirElementoFila(&fila, reg4);  // Inserir 8

    // Exibindo a fila
    exibirFila(&fila);  // Saída: Fila: " 5 7 2 8 "

    // Retornando o número de elementos
    printf("Número de elementos: %d\n", tamanhoFila(&fila));  // Saída: Número de elementos: 4

    // Excluindo um elemento
    REGISTRO regExcluido;
    excluirElementoFila(&fila, &regExcluido);  // Excluir elemento do início (5)
    printf("Elemento excluído: %d\n", regExcluido.chave);  // Saída: Elemento excluído: 5

    // Exibindo a fila após exclusão
    exibirFila(&fila);  // Saída: Fila: " 7 2 8 "

    return 0;
}
```

**Explicação dos Exemplos:**
1. **Inicializar a Fila:** A função `inicializarFila` é chamada para definir os valores iniciais de `inicio` e `nroElem`.
2. **Inserção de Elementos:** `inserirElementoFila` adiciona elementos no final da fila.
3. **Exibir a Fila:** `exibirFila` mostra os elementos atuais da fila.
4. **Exclusão de Elemento:** `excluirElementoFila` remove o elemento do início da fila e armazena o valor excluído.

Esses exemplos demonstram como manipular uma fila utilizando a implementação estática, com operações básicas de inserção, exclusão e exibição.