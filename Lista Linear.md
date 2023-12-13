# Lista Linear

Estrutura de dados na qual cada elemento é precedido por um elemento e sucedido por outro

> exceto o primeiro que não tem predecessor e o último que não tem sucessor.

Os elementos estão em uma dada ordem (por exemplo, a ordem de inclusão ou ordenados por uma chave).

# Lista Linear Sequecial

é uma lista linear na qual a ordem lógica dos elementos (a ordem "Vista" pelo usuario) é a mesma ordem física (em memória principal) dos elementos.
isto é, elementos vizinhos na lista estarão em posições vizinhas de memória.

## Modelagem

Modelaremos usando um `arranjo` de registro;
`Registros` conterão as informações de interesse do usuário;
Nosso arranjo terá um `tamanho fixo` e controlaremos o número de elementos com uma `variável adicional`.

```c
#define MAX 50

typedef int TIPOCHAVE;

typedef struct {
    TIPOCHAVE chave;
    // outros campos
} REGISTRO;

typedef struct {
    Registro A[MAX];
    int nroElem;
} LISTA;
```

**Implementaremos funções para:**

- Inicializar a estrutura
- Retornar a quantidade de elementos válidos
- Exibir os elementos da estrutura
- Buscar um elemento na estrutura
- Inserir um elemento na estrutura
- Excluir um elemento da estrutura
- Reinicializar a estrutura

## Inicialização

para incializar uma estrutura qualquer, precisamos pensar nos valores adequadados para cada um dos campos de nossa estrutura.
Para inicializar uma lista sequencial já criada pelo usuário, só precisamos colocar o valor 0 (zero) no número de elementos válidos.

```c
void inicializarLista(LISTA l) {
    l.nroElem = 0;
}
```

Há algum problema com este código?
Qual a diferença entre os dois códigos abaixo?

```c
void inicializarLista(LISTA* l) {
    l->nroElem = 0;
}
```

A diferença é que no primeiro caso, estamos passando uma cópia da estrutura para a função, enquanto no segundo caso, estamos passando o endereço da estrutura.

## Retornar número de elementos

para retornar o número de elementos válidos, basta retornar o valor do campo `nroElem`.

```c
int tamanho(LISTA* l) {
    return l->nroElem;
}
```

## Exibir elementos

para exibir os elementos, basta percorrer o arranjo e exibir os `elementos` válidos.

```c
void exibirLista(LISTA* l) {
    int i;
    printf("Lista: \" ");
    for (i = 0; i < l->nroElem; i++) {
        printf("%i ", l->A[i].chave);
    }
    printf("\"\n");
}
```

## Buscar elemento

A função de busca deverá:

- Receber uma chave do usuário
- Retornar a posição em que este elemento se encontra na lista (caso seja encontrado)
- Retornar -1 caso o elemento não seja encontrado

```c
int buscaSequencial(LISTA* l, TIPOCHAVE ch) {
    int i = 0;
    while (i < l->nroElem) {
        if (l->A[i].chave == ch) return i;
        else i++;
    }
    return -1;
}
```

## Inserir elemento

O usuário passa como parâmetro o elemento a ser inserido na lista.
Há diferentes possibilidades de inserção:

- No início da lista
- No final da lista
- Ordenada pela chave
- Numa posição indicada pelo usuário

Vamos utilizar o último caso.

## Como inserir?

Se a lista não estiver cheia e o índice passado pelo usuário for válido, devemos: `desloca` todos os elementos posteriores uma posição para a direita;
`insere` o elemento na posição deseja, `soma um` no campo nroElem e retorna true
Caso contrário retorna false

```c
bool inserirElemLista(LISTA* l, REGISTRO reg, int i) {
    int j;
    if((l->nrmElm == MAX) || (i < 0) || (i > l-> nrmLem)) return false;
    for (j = l->nroElem; j > i; j--) l->A[j] = l->A[j-1]
    l->A[i] = reg;
    l->nrmElem++;
    return true;
}
```

## Exclusão de um elemento

O usuário passa a chave do elemento que ele quer excluir

- Se houver um elemento com esta chave na lista, `"exclui este elemento"`,`descloca` todos os elementos posteriores uma posição para a esquerda, `subtrai um` do campo nroElem e retorna true
- Caso contrário retorna false

```c
bool excluirElemLista(LISTA* l, TIPOCHAVE ch) {
    int pos, j;
    pos = buscaSequencial(l, ch);
    if (pos == -1) return false;
    for (j = pos; j < l->nroElem-1; j++) l->A[j] = l->A[j+1];
    l->nroElem--;
    return true;
}
```
