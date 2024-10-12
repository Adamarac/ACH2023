# Aula 03: Estrutura de Dados - Lista Linear Sequencial

## Introdução à Lista Linear
Uma lista linear é uma estrutura de dados onde cada elemento tem um predecessor e um sucessor, exceto o primeiro (sem predecessor) e o último (sem sucessor). A lista linear mantém uma ordem específica para os elementos, que pode ser a ordem de inclusão ou uma ordem definida por uma chave.

## Lista Linear Sequencial
Na lista linear sequencial, a ordem lógica dos elementos é a mesma ordem física em memória. Isso significa que os elementos vizinhos na lista estão armazenados em posições consecutivas na memória.

## Modelagem
Para modelar uma lista linear sequencial, utilizamos um arranjo de registros. Abaixo está a definição da estrutura:

```c
#define MAX 50

typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
    // outros campos...
} REGISTRO;

typedef struct {
    REGISTRO A[MAX];
    int nroElem;  // Número de elementos atuais
} LISTA;
```

- `MAX`: Define o tamanho máximo da lista.
- `TIPOCHAVE`: Define o tipo da chave do registro.
- `REGISTRO`: Estrutura que contém a chave e outros campos de interesse.
- `LISTA`: Estrutura que contém um arranjo de registros e um campo para contar quantos elementos estão na lista.

## Funções de Gerenciamento
Foram implementadas funções para gerenciar a lista sequencial. Abaixo estão as principais funções e suas implementações:

### Inicializar a Lista
Para inicializar a lista, colocamos o número de elementos (`nroElem`) como 0. A função pode ser implementada da seguinte forma:

```c
void inicializarLista(LISTA* l) {
    l->nroElem = 0;
}
```

**Diferença entre passagem por valor e por referência:** A implementação correta é passar o ponteiro da lista (`LISTA* l`), pois, caso contrário, a função manipularia uma cópia da estrutura e as alterações não seriam refletidas fora da função.

### Retornar Número de Elementos
Para retornar o número de elementos da lista, basta acessar o valor de `nroElem`:

```c
int tamanho(LISTA* l) {
    return l->nroElem;
}
```

### Exibir a Lista
Para exibir os elementos da lista, iteramos sobre os elementos válidos e imprimimos suas chaves:

```c
void exibirLista(LISTA* l) {
    int i;
    printf("Lista: \" ");
    for (i = 0; i < l->nroElem; i++)
        printf("%i ", l->A[i].chave);
    printf("\"\n");
}
```

**Saída Exemplo:**
```
Lista: " 21 9 55 "
```

### Busca Sequencial
A busca sequencial procura por um elemento na lista e retorna sua posição, ou `-1` se não for encontrado:

```c
int buscaSequencial(LISTA* l, TIPOCHAVE ch) {
    int i = 0;
    while (i < l->nroElem) {
        if (ch == l->A[i].chave)
            return i;
        else
            i++;
    }
    return -1;
}
```

### Inserção de um Elemento
Podemos inserir um elemento em uma posição específica, desde que a lista não esteja cheia e o índice seja válido. A função abaixo realiza a inserção, deslocando os elementos para abrir espaço:

```c
bool inserirElemLista(LISTA* l, REGISTRO reg, int i) {
    int j;
    if ((l->nroElem == MAX) || (i < 0) || (i > l->nroElem))
        return false;

    for (j = l->nroElem; j > i; j--)
        l->A[j] = l->A[j-1];

    l->A[i] = reg;
    l->nroElem++;
    return true;
}
```

### Exclusão de um Elemento
Para excluir um elemento, utilizamos a função de busca para encontrá-lo. Caso o elemento seja encontrado, os elementos subsequentes são deslocados para "preencher" o espaço deixado:

```c
bool excluirElemLista(TIPOCHAVE ch, LISTA* l) {
    int pos, j;
    pos = buscaSequencial(l, ch);
    if (pos == -1)
        return false;

    for (j = pos; j < l->nroElem - 1; j++)
        l->A[j] = l->A[j+1];

    l->nroElem--;
    return true;
}
```

### Reinicializar a Lista
Para reinicializar a lista, basta definir o número de elementos como 0:

```c
void reinicializarLista(LISTA* l) {
    l->nroElem = 0;
}
```

## Explicações Sobre a Estrutura e Lógica
- **Lista Linear Sequencial**: É uma lista onde a ordem dos elementos em memória reflete a ordem lógica vista pelo usuário.
- **Controle do Tamanho**: A variável `nroElem` é usada para controlar o número de elementos na lista, facilitando operações como inserção, exclusão e busca.
- **Deslocamento de Elementos**: Em operações de inserção e exclusão, pode ser necessário deslocar elementos para garantir que os elementos ocupem posições contíguas em memória, mantendo a ordem correta.

## Explicações Sobre a Sintaxe
- `typedef`: Usado para definir novos tipos. Por exemplo, `TIPOCHAVE` é definido como `int` e `REGISTRO` é uma estrutura com um campo `chave`.
- `struct`: Define uma estrutura de dados, como `REGISTRO` e `LISTA`.
- `bool`: Tipo de dado que representa verdadeiro ou falso, usado para retornar o sucesso ou falha de uma operação.
- `*`: O operador asterisco (`*`) é utilizado para indicar ponteiros, permitindo manipular diretamente a memória da estrutura passada.

## Exemplos de Uso
Abaixo estão alguns exemplos de como as funções podem ser chamadas a partir da função `main`, como os parâmetros são passados e os retornos das funções.

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
    REGISTRO A[MAX];
    int nroElem;  // Número de elementos atuais
} LISTA;

void inicializarLista(LISTA* l) {
    l->nroElem = 0;
}

bool inserirElemLista(LISTA* l, REGISTRO reg, int i) {
    int j;
    if ((l->nroElem == MAX) || (i < 0) || (i > l->nroElem))
        return false;

    for (j = l->nroElem; j > i; j--)
        l->A[j] = l->A[j-1];

    l->A[i] = reg;
    l->nroElem++;
    return true;
}

int tamanho(LISTA* l) {
    return l->nroElem;
}

void exibirLista(LISTA* l) {
    int i;
    printf("Lista: \" ");
    for (i = 0; i < l->nroElem; i++)
        printf("%i ", l->A[i].chave);
    printf("\"\n");
}

int buscaSequencial(LISTA* l, TIPOCHAVE ch) {
    int i = 0;
    while (i < l->nroElem) {
        if (ch == l->A[i].chave)
            return i;
        else
            i++;
    }
    return -1;
}

bool excluirElemLista(TIPOCHAVE ch, LISTA* l) {
    int pos, j;
    pos = buscaSequencial(l, ch);
    if (pos == -1)
        return false;

    for (j = pos; j < l->nroElem - 1; j++)
        l->A[j] = l->A[j+1];

    l->nroElem--;
    return true;
}

int main() {
    LISTA lista;
    inicializarLista(&lista);

    REGISTRO reg1 = {21};
    REGISTRO reg2 = {9};
    REGISTRO reg3 = {55};

    // Inserindo elementos na lista
    inserirElemLista(&lista, reg1, 0);  // Inserir 21 na posição 0
    inserirElemLista(&lista, reg2, 1);  // Inserir 9 na posição 1
    inserirElemLista(&lista, reg3, 1);  // Inserir 55 na posição 1 (desloca 9)

    // Exibindo a lista
    exibirLista(&lista);  // Saída: Lista: " 21 55 9 "

    // Retornando o número de elementos
    printf("Número de elementos: %d\n", tamanho(&lista));  // Saída: Número de elementos: 3

    // Buscando um elemento
    int pos = buscaSequencial(&lista, 9);
    if (pos != -1) {
        printf("Elemento 9 encontrado na posição: %d\n", pos);
    } else {
        printf("Elemento 9 não encontrado\n");
    }

    // Excluindo um elemento
    excluirElemLista(55, &lista);  // Excluir elemento 55
    exibirLista(&lista);  // Saída: Lista: " 21 9 "

    return 0;
}
```

**Explicação dos Exemplos:**
1. **Inicializar a Lista:** A função `inicializarLista` é chamada para definir `nroElem` como `0`.
2. **Inserção de Elementos:** `inserirElemLista` é usada para adicionar elementos nas posições específicas.
3. **Exibir a Lista:** `exibirLista` mostra os elementos atuais da lista.
4. **Buscar um Elemento:** `buscaSequencial` é chamada para encontrar a posição do elemento `9`.
5. **Excluir um Elemento:** `excluirElemLista` remove o elemento `55` da lista.

Os exemplos fornecem uma visão clara de como cada função pode ser utilizada e como os parâmetros são passados para manipular a estrutura de dados da lista sequencial.