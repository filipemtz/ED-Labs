
# Vectors (parte 2)

Neste laboratório, vamos continuar implementando a estrutura de dados Vector e seus funções.
Como no reteiro anterior, as intruções de como implementar as funções são dadas a seguir. Parecem muitas, mas veremos que algumas serão utilizadas para implementar as outras.

Vamos começar adicionando as novas funções no arquivo ```vector.h```.

```
// Remove o i-ésimo elemento do vetor.
data_type vector_remove(Vector *v, int i);

// Remove o primeiro elemento
data_type vector_pop_front(Vector *v);

// Remove o ultimo elemento
data_type vector_pop_back(Vector *v);

// Insere o elemento na i-esima posicao
void vector_insert(Vector *v, int i, data_type val);

// Troca os elementos das posições i e j (i vira j e j vira i)
void vector_swap(Vector *v, int i, int j);

// Ordena o vetor in-place (sem criar um novo vetor)
void vector_sort(Vector *v);

// Retorna o indice de val usando busca binaria. Retorna -1 se nao encontrado.
int vector_binary_search(Vector *v, data_type val);

// Inverte o vetor in-place (sem criar um novo vetor)
void vector_reverse(Vector *v);

// Cria uma cópia do vector e dos valores de seus atributos.
Vector *vector_copy(Vector *v);

// Remove todos os elementos de v
void vector_clear(Vector *v);
```

## 11. Ordenação In-Place (sem criar um novo Vector)

A função ```vector_sort``` deve ordenar o vetor em ordem crescente usando o algoritmo de ordenação *Bubble Sort*. Para implementar o *Bubble Sort*, devemos percorrer o vetor comparando cada elemento com o próximo e trocando-os de posição quando estiverem com a ordem errada (o primeiro maior que o segundo). O processo deve ser repetido até que não ocorram mais trocas durante uma passagem pelo vetor. Isso pode ser implementado usando um loop externo que controla o número de passagens pelo vetor e um loop interno que percorre o vetor comparando elementos adjacentes. Se durante a comparação um elemento estiver na ordem errada, eles devem ser trocados de posição usando a função ```vector_swap```.

Veja a [animação](https://visualgo.net/en/sorting) ([alternativa ](https://www.cs.usfca.edu/~galles/visualization/ComparisonSort.html)) do *Bubble Sort* (aumente a velocidade) e note como a cada passagem pelo vetor, os maiores elementos são levados para as posições finais. Isso significa que, ao final da primeira iteração, o maior elemento estará na posição ```n - 1```, onde ```n``` é o tamanho do vetor. Na segunda iteração, o segundo maior elemento será movido para a posição ```n - 2```, que agora é a última posição restante no vetor que ainda precisa ser ordenada. E assim por diante. Como temos a garantia que os maiores elementos estarão nas posições corretas, na iteração ```i``` não precisamos percorrer o vetor fazendo trocas até o final, mas podemos iterar apenas enquanto ```j < n - i - 1```, onde ```j``` é o contador do loop interno. Essa mudança ajuda a economizar algumas iterações e tornar o algoritmo um pouco mais eficiente.

**Dica para melhoria de performance**: Quando o loop interno terminar de percorrer o vetor, deve-se verificar se alguma troca ocorreu. Se não, isso significa que o vetor está ordenado e o loop externo pode ser interrompido.

Use a função para resolver o quinto exercício da seção no [Testr](http://200.137.66.73:8000/).

## 12. Busca Binária

A função ```vector_binary_search``` deve receber um ponteiro para o vetor e um valor ```val``` como parâmetros e realizar uma busca binária pelo valor no vetor. Se o valor for encontrado, a função deve retornar o seu índice. Caso contrário, deve retornar -1.

A busca binária é um algoritmo eficiente de busca em um vetor ordenado, que divide o vetor em duas metades e verifica se o valor buscado está na metade esquerda ou direita do vetor. O processo é repetido até que o valor seja encontrado ou que não haja mais elementos para serem verificados.

Para implementar a busca binária, definimos primeiro um índice mínimo e um máximo que definem  o intervalo em que a busca será realizada. Inicialmente, o intervalo vai do índice 0 até o último índice do vetor, ```n-1```. Em cada iteração do algoritmo, calculamos o índice do elemento central do intervalo. Em seguida, verificamos se o valor buscado ```val``` é menor ou maior do que o elemento central. Se for menor, a busca deve ser realizada na metade esquerda do vetor, ou seja, do índice mínimo até o índice ```central - 1```. Se for maior, a busca deve ser realizada na metade direita do vetor, ou seja, do índice ```central + 1``` até o índice máximo. O processo é repetido até que o valor seja encontrado ou enquanto o índice mínimo é menor que o índice máximo. A segunda condição irá ocorrer quando o elemento não existe no vetor.

Use a função para resolver o sexto exercício da seção no [Testr](http://200.137.66.73:8000/).

## 13. Inversão do Vetor

A função ```vector_reverse``` deve inverter a ordem dos elementos do vetor *in-place* (sem criar um novo vetor). Use a função ```vector_swap``` na implementação.

## [Extra] 14. Cria uma cópia do vector e dos valores de seus atributos.

A função ```vector_copy``` deve receber um ponteiro para o vetor como parâmetro e criar uma cópia do vetor e seus atributos (data, size e allocated) em um novo vetor alocado dinamicamente. A função deve retornar um ponteiro para o novo vetor. Use a função ```memcpy``` ([documentação](https://cplusplus.com/reference/cstring/memcpy/)) da ```string.h``` para copiar o campo ```data```. Use o valgrind para verificar se a cópia foi feita corretamente.

## [Extra] 15. Remoção de Todos os Elementos

A função ```vector_clear``` deve remover todos os elementos do vetor, definindo o tamanho do vetor como 0. Após a chamada da função, o Vector deve voltar à um estado igual àquele de quando ele é criado. Se o vetor estiver vazio, não devem ocorrer erros ou vazamentos de memória.

## [Extra] 16. Busca por Todas as Ocorrências

Escreva uma função ```vector_find_all``` que dado um Vector e um valor, retorne um Vector com os índices de todas as ocorrências do valor.

