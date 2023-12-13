# Uso de Memória

## O que é memória?

Memória é um dispositivo que armazena dados e programas (instruções) temporariamente ou permanentemente. A memória é dividida em células de memória, cada uma com um endereço único.

## Uso de memória em Java:

```java
public class PesoAltura {
    int peso; // peso em quilogramas
    int altura; // altura em centímetros
}

public class EstruturaSimples {
    public static final int alturaMaxima = 225;
    public static void main(String[] args) {
        PesoAltura pessoa1 = new PesoAltura();
        pessoa1.peso = 80;
        pessoa1.altura = 185;
        System.out.println("Peso: " + pessoa1.peso + ", Altura: " + pessoa1.altura);
        if(pessoa1.altura > alturaMaxima) {
            System.out.println("Altura acima da máxima.");
        } else {
            System.out.println("Altura abaixo da máxima.");
        }
    }
}
```

## Uso de memória em C:

```c
#include <stdio.h>
#define alturaMaxima 225

typedef struct {
    int peso; // peso em quilogramas
    int altura; // altura em centímetros
} PesoAltura;

int main() {
    PesoAltura pessoa1;
    pessoa1.peso = 80;
    pessoa1.altura = 185;
    printf("Peso: %i, Altura: %i. \n", pessoa1.peso, pessoa1.altura);
    if(pessoa1.altura > alturaMaxima) {
        printf("Altura acima da máxima. \n");
    } else {
        printf("Altura abaixo da máxima. \n");
    }
    return 0;
}
```

### Por que o uso de memória foi tão diferente entre as duas linguagens?

| Java         | --- | ---    | --- |
| ------------ | --- | ------ | --- |
| alturaMaxima | 225 | ---    | --- |
| ---          | --- | 9742   | --- |
| args         | --- | Peso   | 80  |
| pessoa1      | --- | altura | 185 |

| C       | --- | ---    | --- |
| ------- | --- | ------ | --- |
| pessoa1 | --- | peso   | 80  |
| ---     | --- | altura | 185 |

As duas implentações não são exatamente a mesma, por que em Java é possivel fazer uma alocação na memoria a parte e variavel endereço guardar o endereço dessa alocação, enquanto em C o endereço é o proprio nome da variavel.

## Ponteiros e alocação de memória

em C há uma disntição bastante explícita entre um tipo (ou uma estrutura) e um endereço:
`int x;` significa que x é uma variavel do tipo `inteiro`
`int* y;` significa que y é uma variável do `tipo endereço para inteiro`

> obs: O asterisco (\*) após a palavra int indica que estamos declarando um ponteiro para um inteiro, e não um inteiro.

```c
#include <stdio.h>
int main() {
    int x = 25;
    int* y = &x;
    *y = 30;
    printf("Valor atual de x: %i \n", x); // $ Valor atual de x: 30
    return 0;
}
```

a variavel x é inicializada com valor 25.
a variavel y é inicializada com o endereço de x.

> Coloca-se o valor 30 na posição de memória referenciada (apontada) pelo endereço armazenado em y.

| C    | --- | ---  |
| ---- | --- | ---- |
| 0940 | x   | 25   |
| 0936 | y   | 0940 |

Em C há uma função para alocação de memória:
alloc (memory allocation)

- recebe como parâmetro o `número de bytes` que se deseja alocar
- retorna o `endereço` da primeira posição do bloco de memória alocado esse retorno é do tipo `void*`
- existe um operador chamando `sizeof` que recebe como parâmetro um tipo e retorna o número de bytes que esse tipo ocupa na memória

```c
#include <stdio.h>
#include <stdlib.h>
int main () {
    int* y = (int*) malloc(sizeof(int));
    *y = 20;
    int z = sizeof(int);
    printf("*y=%i z=%i\n", *y, z); // $ *y=20 z=4
    return 0;
}
```

| C    | --- | ---  | --- | ---- | --- |
| ---- | --- | ---- | --- | ---- | --- |
| 0940 | y   | 2200 |     | 2200 | 20  |
| 0936 | z   | 4    |     |      |     |

## Nossa segunda implementação

```c
#include <stdio.h>
#include <stdlib.h>
#define alturaMaxima 225

typedef struct {
    int peso; // peso em quilogramas
    int altura; // altura em centímetros
} PesoAltura;

int main() {
    PesoAltura* pessoa1 = (PesoAltura*) malloc(sizeof(PesoAltura));

    pessoa1->peso = 80;
    pessoa1->altura = 185;

    printf("Peso: %i, Altura: %i. \n", pessoa1->peso, pessoa1->altura); // $ Peso: 80, Altura: 185.

    if(pessoa1->altura > alturaMaxima) {
        printf("Altura acima da máxima. \n");
    } else {
        printf("Altura abaixo da máxima. \n");
    }
    return 0;
}
```

| C       | ---  | --- | ---    | ---- | --- |
| ------- | ---- | --- | ------ | ---- | --- |
| pessoa1 | 7408 |     |        |      |     |
|         |      |     |        |      |     |
| 7408    | peso | 80  | altura | 185  |     |
|         |      |     |        |      |     |
