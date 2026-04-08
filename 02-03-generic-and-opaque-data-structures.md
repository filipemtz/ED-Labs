
# Tipos Opacos e Estruturas de Dados Genéricas

Neste laboratório, vamos criar uma mini-loja virtual usando o conceito de tipos opacos e estruturas de dados genéricas. Para isto, vamos realizar as seguintes atividades:

* Criar um tipo opaco Produto e funções para manipular seus dados.

* Realizar algumas atividades para relembrar como usar ponteiros de funções como argumentos de funções (callbacks) e como atributos de estruturas.

* Tornar o tipo Vector genérico para que ele seja capaz de armazenar qualquer tipo de dado.

Para cada atividade, envie a resposta no sistema [Testr](http://200.137.66.73:8080/).

## Revisão: Tipos Opacos

Em C, um tipo opaco é um tipo de dado cuja estrutura interna é ocultada do usuário. Isso significa que o código que usa o tipo opaco não precisa saber como os dados são armazenados ou manipulados internamente.

Vantagens:
* Encapsulamento: Permite ocultar detalhes de implementação e fornecer uma interface mais simples e segura para o usuário.
* Reuso: Facilita o reuso de código, pois o código que usa o tipo opaco não precisa ser modificado se a estrutura interna do tipo for alterada.
* Manutenabilidade: Torna o código mais fácil de manter, pois os detalhes de implementação são ocultados em um único local.

Implementação:
Em C, os tipos opacos podem ser implementados usando typedefs. Um typedef é um alias para um tipo de dado existente. A título de exemplo, vamos implementar um tipo opaco para representar uma pessoa com nome, idade e altura. O arquivo pessoa.h define o tipo Pessoa e declara as funções de manipulação do tipo:

```c
#ifndef _PESSOA_H_
#define _PESSOA_H_

typedef struct pessoa Pessoa;

Pessoa *pessoa_construtor(const char *nome, int idade, float altura);

char *pessoa_get_nome(Pessoa *pessoa);
int pessoa_get_idade(Pessoa *pessoa);
float pessoa_get_altura(Pessoa *pessoa);

// Note que as funções abaixo podem verificar a validade dos valores passados como argumento
void pessoa_set_nome(Pessoa *pessoa, const char *nome);
void pessoa_set_idade(Pessoa *pessoa, int idade);
void pessoa_set_altura(Pessoa *pessoa, float altura);

void pessoa_destrutor(Pessoa *pessoa);

#endif
```

**Observação 1**: As funções get e set são necessárias dado que não podemos acessar diretamente os atributos da estrutura Pessoa. As operações que o tipo disponibiliza são chamadas de **interface** do tipo.

**Observação 2**: Note que nem sempre é necessário fornecer funções para alterar os atributos de um tipo opaco. Por exemplo, se a altura de uma pessoa não puder ser alterada, não é necessário fornecer uma função set_altura. Em outro exemplo, considere uma conta de banco. Ao invés de prover funções para atualizar o saldo, faria mais sentido prover funções de depósito e saque, por exemplo, que estão alinhadas com o domínio do problema.

Voltando ao exemplo, o arquivo pessoa.c define a estrutura e implementa as funções.

```c

#include "pessoa.h"


struct pessoa
{
    char nome[64];
    int idade;
    float altura;
};

Pessoa *pessoa_criar(const char *nome, int idade, float altura)
{
    Pessoa *pessoa = malloc(sizeof(Pessoa));

    strcpy(pessoa->nome, nome);
    pessoa->idade = idade;
    pessoa->altura = altura;

    return pessoa;
}

// Implementação das demais funções...

void pessoa_destruir(Pessoa *pessoa)
{
    free(pessoa);
}
```

## Exercício 1: Criando o tipo opaco produto

Nesta atividade, você deve criar o tipo Product com os seguintes atributos:
* name: nome (string com no máximo 64 caracteres),
* price: preco (float positivo),
* discount: desconto (float entre 0 e 1),
* qtd: quantidade em estoque (inteiro não negativo) e
* sales: numero de vendas (inteiro não negativo).


```
Obs: O termo "positivo" indica valores maiores que zero. O termo "não negativo" indica valores maiores ou iguais a zero.
```

O tipo Product deve ser **opaco**, isto é, sua definição deve estar no arquivo product.c. O arquivo product.h deve conter apenas a declaração do tipo Product e da interface operação com o tipo:

```c
#ifndef _PRODUCT_H_
#define _PRODUCT_H_

typedef struct product Product;

// funcoes

#endif
```

Crie as seguintes funções (declarações em product.h e implementações em product.c) para manipular os produtos:
* product_constructor: recebe como entrada o nome do produto, o preço e a quantidade inicial em estoque. Em seguida aloca espaço para um novo produto, inicializa os atributos e retorna o ponteiro para o novo produto. Os atributos desconto e número de vendas devem ser inicializados com 0. Use a função strncpy para garantir que o nome do produto tenha no máximo 64 caracteres. Se o preço ou a quantidade inicial em estoque forem negativos, exiba na tela a mensagem "VALOR INVALIDO" e retorne NULL.
* product_destructor: recebe como entrada um ponteiro para um produto e libera a memória alocada para ele.
* product_get_<nome do atributo> (e.g., product_get_nome, product_get_preco, etc.).
* product_set_name, product_set_price, product_set_discount: Estas funções devem atualizar os valores dos atributos se os os valores forem válidos (por exemplo, o preço ser positivo). Se não forem, deve ser exibida na tela a mensagem "VALOR INVALIDO" e o valor não deve ser alterado. Use a função strncpy para garantir que o nome do produto tenha no máximo 64 caracteres.
* product_sell: recebe como entrada um ponteiro para um produto e a quantidade vendida. Se a quantidade vendida for maior que a quantidade em estoque, deve ser exibida na tela a mensagem "ESTOQUE INSUFICIENTE" e a quantidade vendida não deve ser atualizada. Caso contrário, a quantidade vendida deve ser adicionada ao número de vendas e subtraída da quantidade em estoque. Verifique se a quantidade vendida é maior que zero. Se não for, exiba a mensagem "QUANTIDADE INVALIDA".
* product_buy: recebe como entrada um ponteiro para um produto e a quantidade comprada. A quantidade comprada deve ser adicionada à quantidade em estoque. Verifique se a quantidade comprada é maior que zero. Se não for, exiba a mensagem "QUANTIDADE INVALIDA".
* product_get_price_with_discount: retorna o preço com desconto aplicado.
* product_print: exibe na tela o nome, preço, desconto, preço com desconto, quantidade em estoque e número de vendas do produto usando o formato "Product(nome, price, discount, price_with_discount, qtd, sales)". Os campos float devem ser exibidos com 2 casas depois da vírgula.

Implemente um programa de teste main.c que inicialmente leia o nome, o preço e a quantidade em estoque de um produto e crie uma variável deste tipo usando product_constructor. Em seguida, leia um número inteiro n de operações e realize n operações. Cada operação é composta por um caractere c e, dependendo do valor de c, pode ser seguido por um ou dois valores inteiros. As operações possíveis são:
* 'P': imprimir os dados do produto.
* 'S': vender uma quantidade de produtos. Leia a quantidade vendida e use-a como entrada para a função product_sell.
* 'B': comprar uma quantidade de produtos. Leia a quantidade comprada e use-a como entrada para a função product_buy.
* 'D': alterar o desconto do produto. Leia o novo valor do desconto e use-o como entrada para a função product_set_discount.
* 'Q': alterar o preço do produto. Leia o novo valor do preço e use-o como entrada para a função product_set_price.
* 'N': alterar o nome do produto. Leia o novo valor do nome e use-o como entrada para a função product_set_name.

## Exercício 2: Adicionando funções de comparação aos produtos

Adicione as seguintes funções ao arquivo product.h e implemente-as no arquivo product.c:
* product_compare_name: recebe duas variáveis do tipo ```const void*``` representando dois produtos, converte os ponteiros para ```Product*``` e retorna o resultado da comparação dos nomes. Deve ser retornado um valor negativo se o nome do primeiro produto for lexicograficamente menor que o nome do segundo produto, um valor positivo se o nome do primeiro produto for lexicograficamente maior que o nome do segundo produto e zero se os nomes forem iguais. Use a função strcmp para implementar a função.
* product_compare_price: recebe dois ```const void*``` representando dois produtos, converte os ponteiros para ```Product*``` e retorna a diferença entre os preços dos produtos. Deve ser retornado um valor negativo se o preço do primeiro produto for menor que o preço do segundo produto, um valor positivo se o preço do primeiro produto for maior que o preço do segundo produto e zero se os preços forem iguais.
* product_compare_sales: recebe dois ```const void*``` representando dois produtos, converte os ponteiros para ```Product*``` e retorna a diferença entre o número de vendas dos produtos. Deve ser retornado um valor negativo se o número de vendas do primeiro produto for menor que o número de vendas do segundo produto, um valor positivo se o número de vendas do primeiro produto for maior que o número de vendas do segundo produto e zero se os números de vendas forem iguais.

Usaremos ```const void*``` como entrada para as funções para que elas possam ser usadas pela função qsort e junto ao tipo Vector depois.

Mas inicialmente, para testar as funções, vamos fazer um programa de teste main.c que leia um número inteiro n e, em seguida, leia o nome, o preço, a quantidade em estoque e o número de vendas de n produtos. Use estes dados para preencher um array de produtos (sem usar o tipo Vector por enquanto). Por fim, leia um caractere c e, dependendo do valor de c, ordene o array de produtos pelo nome, pelo preço ou pelo número de vendas. Se for digitado 'N', ordene o array pelo nome, se for digitado 'P', ordene o array pelo preço e se for digitado 'S', ordene o array pelo número de vendas.

Utilize a função [qsort](https://www.galirows.com.br/meublog/programacao/utilizacao-funcao-qsort/) e as funções de comparação para ordenar o array. A função qsort recebe como entrada o vetor, o número de elementos, o tamanho de cada elemento (em bytes; use a função sizeof), uma função de comparação e retorna o vetor ordenado.

**IMPORTANTE: A FUNÇÃO QSORT PARA PARA AS FUNÇÕES DE COMPARAÇÃO O ENDEREÇO DO I-ÉSIMO ELEMENTO DO ARRAY E NÃO O VALOR!! PORTANTO, PARA CONVERTER DE PONTEIRO GENÉRICO PARA PONTEIRO DE PRODUTO, TEREMOS QUE FAZER:**

```c

int product_compare_name(const void *prod1, const void *prod2)
{
    // se a função qsort passa o endereço do i-ésimo elemento e o array é de ponteiros, então
    // o endereço do i-ésimo elemento é um ponteiro para um ponteiro de produto.
    Product *product1 = *((Product **)prod1);
    Product *product2 = *((Product **)prod2);
    return strcmp(product1->name, product2->name);
}

```


## Exercício 3: Vector Genérico

Conforme instruções dadas em aula, adapte o tipo Vector para que ele seja genérico. Substitua o data_type de int para void* e adapte as funções para que elas possam ser usadas com qualquer tipo de dado. Para isso, você deve, por exemplo, passar uma função de comparação como argumento para as funções de ordenação e busca.

**IMPORTANTE: DIFERENTE DA QSORT, EM NOSSA FUNÇÃO DE ORDENAÇÃO PODEMOS PASSAR O PONTEIRO DIRETAMENTE PARA A FUNÇÃO DE COMPARAÇÃO AO INVÉS DE PASSAR O ENDEREÇO. NESTE CASO, PARA CONVERTER DE PONTEIRO GENÉRICO PARA PONTEIRO DE PRODUTO, TEREMOS QUE FAZER:**

```c

int product_compare_name(const void *prod1, const void *prod2)
{
    // assumindo que a funcao de comparacao recebera' os valores dos i-esimos elementos,
    // entao nao precisamos de um ponteiro para ponteiro de produto.
    Product *product1 = (Product *)prod1;
    Product *product2 = (Product *)prod2;
    return strcmp(product1->name, product2->name);
}

```

A implementação acima assume que a função de comparação será chamada da seguinte forma:

```c
    if (cmp_fn(v->data[j], v->data[j + 1]) > 0)  // cmp_fn é a função de comparação
```


Utilizaremos um programa similar ao anterior para testar o tipo Vector genérico. O programa deve ler um número inteiro n e, em seguida, ler n produtos. Use os produtos lidos para preencher um Vector de produtos. Use a função vector_push_back para adicionar os itens no vector. Por fim, leia um caractere c e, dependendo do valor de c, ordene o vetor de produtos pelo nome, pelo preço ou pelo número de vendas. Se for digitado 'N', ordene o vetor pelo nome, se for digitado 'P', ordene o vetor pelo preço e se for digitado 'S', ordene o vetor pelo número de vendas.

Use o valgrind para verificar vazamentos de memória.

