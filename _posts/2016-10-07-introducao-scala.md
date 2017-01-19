---
layout: post
title: Uma introdução à programação em Scala
date: 07/10/2016 18:00:00
disqus: y
share: y
---

###  O que é

Scala é uma linguagem de programação multi paradigma que oferece suporte à Programação Orientada a Objetos e Programação Funcional de forma conjunta. A linguagem é compilada e seu sistema de tipos é forte e estático, além de ser unificado (todos os valores, mesmo os numéricos como Integer e Double, ou booleanos) são todos instâncias de uma classe. Você pode conferir tudo em detalhes, fazer o download, instalar e configurar [nesse site](http://scala-lang.org).

A linguagem foi criada pelo cientista da computação alemão Martin Odersky, que é professor na Escola Politécnica Federal de Lausana (École Polytechnique Fédérale de Lausanne, EPFL), na Suíça. Odersky participou do desenvolvimento do próprio Java, trabalhando com toda parte de Generics e com o desenvolvimento da versão atual do javac.

### Público alvo

A linguagem possuí muitos recursos, e o paradigma de programação funcional exige um grande esforço para ser aprendido, principalmente para os programadores que não estão habituados com a forma funcional de se programar. Por conta desses fatores, a quantidade de recursos e um paradigma não usual, a curva de aprendizado de Scala é enorme, o que cria barreiras em determinadas equipes de desenvolvimento.

Por conta das dificuldades, uma métrica de aprendizado da linguagem foi criada por Odersky, e apesar de sua controvérsia, é uma boa fonte para se avaliar o seu progresso e pautar estudos. Uma descrição completa dos níveis pode ser lida aqui [Link](http://www.scala-lang.org/old/node/8610)


### Ambiente de execução
Scala é uma das linguagens que tem como ambiente de execução a JVM, assim como Java, Groovy e Kotlin. Isso significa que ao ser compilada, um arquivo ".scala" (arquivo de código fonte) se torna um arquivo ".class" (bytecode lido pela JVM), e quando empacotada ela se torna um JAR. Resumindo, é possível usar bibliotecas Java em projetos Scala, e apesar da haverem limitações, usar Scala em Java também.

### Um pouco de Programação Orientada a Objetos

Orientação a objetos é um tópico amplamente discutido nos âmbitos acadêmicos, mercadológicos e em basicamente qualquer lugar, portanto serei breve na introdução ao tema.

Orientação a objetos é sobre a **troca de mensagens entre objetos** a fim de controlar o fluxo de execução do programa. 

**Objetos** são estruturas alocadas em memória, complexas, que possuem **métodos** (funções dentro do escopo do objeto) e **atributos** (que representam o estado interno do objeto). Essa estrutura é declarada em uma **Classe**, que descreve todos esses detalhes e que é utilizada para criação do objeto. Dá-se assim então uma forma de se criarem tipos mais complexos para a execução do programa, que podem ser pedidos como parâmetros, declarados como atributos, ou servirem de retorno para algum procedimento e/ou função, concedendo assim as características de "first-class citizen" aos objetos.

**Métodos e atributos** são a interface declarada de um objeto e ditam as regras pelas quais como a interação com um objeto pode ser feita. É possível controlar a visibilidade de atributos e métodos também.

É possível estabelecer relações de hierarquia entre os objetos, fazendo com que uma classe estenda o comportamento de outra, herdando assim características da mesma. Esse mecanismo é chamado de **herança**.

**Polimorfismo**, e suas variações, permitem alterações em tempo de execução no comportamento do programa, devido a possibilidade de se alterar o tipo do objeto requisitado (normalmente por um método) dentro de sua **hierarquia estabelecida**. As propriedades de variância (covariância, invariância e contravariância), que regem as regras de substituição dentro da hierarquia de tipos estabelecida, variam de linguagem para linguagem.

A orientação a objetos é uma forma de se estabelecer tipos de dados mais específicos ao domínio de aplicação e uma excelente forma de se organizar o código da aplicação.


### Programação Funcional

Programação Funcional é sobre o uso de **funções para a composição de programas maiores**. O conceito de função é baseado no conceito matemático homônimo. Além da matemática tradicional e amplamente conhecida, muitos matemáticos importantes, como Alonzo Church e Haskell Curry influenciaram vários conceitos de programação funcional. 

Na matemática, uma função realiza o mapeamento dos membros de um conjunto, denominado conjunto domínio, para um conjunto chamado de conjunto imagem. Uma função é normalmente aplicada em um ou vários elementos do conjunto domínio, dos quais são fornecidos através de parâmetros para a função, que define como resultado um elemento do conjunto imagem. 

Um ponto importante sobre funções matemáticas é que estas **não possuem efeitos colaterais**, pois sempre definem o mesmo valor para o mesmo conjunto de parâmetros fornecidos. Outro conceito importante é que a forma com que suas expressões de mapeamento são controladas: **por recursão e expressões condicionais**.

**Fora do domínio matemático** e no domínio da ciência da computação, podemos dizer que uma função declara ou não parâmetro (s) de um tipo específico e obrigatoriamente retorna algo de outro tipo específico, podendo ser ou não o mesmo tipo do parâmetro.

Linguagens de programação funcional tendem a enfatizar o uso de funções puras, que não possuem efeitos colaterais, ou seja, não realizam algo que não está explicito em sua declaração, como:

+ Realocação de variáveis;
+ Lançamento de exceções;
+ Alteração do estado interno de um objeto;
+ Impressão em console de variáveis de entrada/resultados;
+ Leitura e/ou escrita de arquivos;
+ Entre outros.

Apesar de funções puras permitirem diversas vantagens, sua execução na prática é difícil e muitas vezes abrimos exceções. Poucas linguagens de programação hoje permitem programação funcional de forma pura. E Scala não é uma delas.

Funções são "cidadãs de primeira-classe" em linguagens de programação funcional, ou seja, podem ser utilizadas como parâmetros para outras funções, como retorno, ou até mesmo serem definidas em valores. 

### Imperativo x Funcional

Podemos dizer que **Programação Imperativa**, que se ramifica entre várias formas, utilizada e maior e menor grau por linguagens como Pascal, C, Java, C++, C#, dentre outras, é sobre o uso de:

+ Variáveis mutáveis;
+ Estruturas de repetição;
+ Estruturas condicionais.

Toda lógica de programação aprendida com essas ferramentas tende a seguir o pensamento imperativo, e com a adesão da orientação a objetos por parte de algumas linguagens, muitos desses conceitos são potencializados com o uso de objetos mutáveis (que permitem a alteração de estado interno de seus atributos/variáveis).

**Programação Funcional** é diferente disso. Primeiro que todos as instruções são expressões que retornam obrigatoriamente em algum valor, inclusive elementos como um if, por exemplo. Em oposição à listagem acima, temos:

+ Valores imutáveis, constantes;
+ Recursão;
+ Expressões condicionais.

Não se utiliza variáveis, não ao menos em forma livre, mas sim valores constantes; estruturas de repetição são substituídas por recursão e formas otimizadas como tail-recursion (recursão em cauda). O conceito de imutabilidade, inclusive aplicada aos objetos, favorece a construção de programas que utilizam concorrência / programação paralela, já que o uso de estado mutável compartilhado (Imagine compartilhar o objeto com getters/setters entre várias threads que vão modificá-lo seguindo alguma lógica) ocasiona sérios problemas nesse modelo de programação.

Além disso para esse caso, o uso de recursão, em detrimento das estruturas de repetição, favorece o modelo de programação concorrente/paralelizado, apesar de ter desempenho inferior em relação às estruturas de repetição, quando executado normalmente.

Substituir os elementos tradicionais pode ser doloroso, os algorítimos se modificam totalmente, e por isso a mudança é custosa. Estamos habituados com um conceito de programação e o estabelecemos como verdade. Mas verdade é um conceito que muda dependendo do conhecimento que se adquire.

### Anatomia de um Programa Scala

Vamos ver como tudo isso é utilizado pelo Scala:

### REPL

Scala possuí um interpretador, o Read-Evaluate-Print Loop é um shell que permite a execução de código Scala, de forma rápida e simples, perfeita para explorar a linguagem. Basta instalar e configurar seu ambiente conforme descrito no próprio site da linguagem para executar os exemplos abaixo:

### Function

Como já descrevemos a importância das Funções (Functions) em programação funcional (Functional Programming, FP), vamos entender como escrevê-las em Scala.

Funções da forma mais tradicional são definidas com a palavra-chave "**def**", seguidas por um nome e uma lista de parâmetros entre parênteses. Os parâmetros de uma função são declarados utilizando a sintaxe **nome: Tipo**, sendo separados por vírgula, ficando entre parênteses.

Funções podem ter uma declaração do tipo de retorno após os parênteses. Seu corpo definido após um sinal "**=**". 

O corpo pode estar entre chaves, caso houver necessidade de se terem várias expressões, ou apenas por questão de preferência. A última expressão é a que será retornada, não sendo necessário o uso da palavra-chave "**return**", como em JavaScript e Java. (Ver o exemplo é mais fácil que ler esse parágrafo).

Uma forma informal de descrever uma função é demonstrada abaixo:

> def nome(parâmetros):TipoRetorno = CorpoDoMétodo

O código abaixo contém exemplos das variações descritas:

{% highlight scala %}
// Square: calcula o quadrado de um número**

/**
* Exemplo A1 - Definição de corpo com expressão única.
*/
def squareA(x: Int): Int = x * 2

/**
* Exemplo B1 - Definição de corpo com expressão única 
* e chaves não-obrigatórias.
*/
def squareB(x: Int): Int = { 
    x * 2
}

/**
* Exemplo C1 - Corpo com chaves obrigatórias por haver 
* mais de uma expressão.
*/
def squareC(x: Int): Int = {
    val multiplyBy = 2
    x * multiplyBy
}

/**
* Exemplo D1 - Tipo de retorno da função omitido; 
* Scala descobre em tempo de compilação que
* o tipo da última expressão do corpo é o tipo de retorno.
*/
def squareD(x: Int) = { 
    x * 2
}

/**
* Exemplo E1 - É possível usar a palavra-chave return; 
* Nesse caso declarar o tipo de retorno da função é obrigatório.
*/
def squareE(x: Int): Int = { 
    return x * 2
}

/**
* Exemplo F1 - Declaração de vários parâmetros.
*/
def sum(x: Int, y: Int): Int = x + y

/**
* Exemplo G1 - Atribuição de resultado da chamada de função.
*/
val result = square(2)

/**
* Exemplo H1 - Atribuição de resultado da chamada de função,
* com declaração de tipo.
*/
val result: Int = square(2)


{% endhighlight %}


Para invocar a função, utilize a sintaxe descrita no exemplo **G1**, e o resultado da expressão poderá será calculado.

### Atribuição de valores e expressão condicional
Como visto no exemplo **G1**, valores podem ser  declarados utilizando a palavra chave **val** seguida de seu **nome**. É impossível a partir desse ponto reassinar algum outro valor, pois **val** indica que não poderá ser alterado. Outro ponto interessante é que não é necessário declarar o tipo de **result** (do exemplo **G1**; Int, no caso), pois Scala possuí um sistema de **inferência de tipos (Type Inference)**, que em tempo de compilação consegue verificar o tipo. Isso faz com que se ganhe legibilidade em casos em que o tipo é obvio, reduzindo a verbosidade da linguagem, mas mantendo a checagem de tipos (e sua segurança) em tempo compilação. O exemplo **H1** mostra como seria a declaração explicita do tipo do valor.

Se necessário, Scala suporta o operador **var**, que funciona como uma variável tradicional, assim como outras linguagens. Seu uso no entanto deve ser para casos muito restritos, e deve ser evitado o máximo possível.

Em Scala, **if** é uma expressão que retorna um valor.


{% highlight scala %}
def name(x: String) = if (x.isEmpty) "Anonymous" else x
{% endhighlight %}

É possível utilizar a mesma regra dos blocos de corpo das funções, se  caso houver mais de uma expressão.

###  High-Order Functions e Anonymous Functions

Não existe uma tradução exatamente boa para **High-Order Functions**, mas poderíamos dizer que é algo aproximado à "Funções de Alta-Ordem". High-Order Functions são funções que recebem como parâmetros e/ou retornam outra função.

Além de compor uma função da forma como fizemos acima, é possível utilizar uma **Anonymous Function**, que é uma forma reduzida e localizada de se escrever uma função.

O tipo de função, ou **Function Type** em Scala, é pôr de baixo dos panos um objeto fruto de uma classe, e por isso pode ser utilizado como um tipo qualquer pela linguagem. Sua sintaxe é:

> TipoParâmetro => TipoParâmetroRetorno

ou

> (Param1, Param2) => TipoParâmetroRetorno

O número de parâmetros pode variar. Havia uma limitação em versões antigas do Scala (antes da versão 2.11.x) que limitava a 22 o número de parâmetros. Essa limitação não existe mais nas novas versões.

**Anonymous Functions** são semelhantes às funções normais, podendo serem atribuídas diretamente como um parâmetro para outra função ou armazenadas utilizando **val** ou **def**. Observe a sintaxe abaixo:

> (paramA: String, paramB: String) => paramA == paramB

Segue abaixo descrição e exemplos de como utilizar High-Order Functions e Anonymous Functions.


{% highlight scala %}
/**
 * Exemplo A2
 * High-Order Function
 * Função que recebe dois parâmetros Int
 * e outro do tipo "Type Function".
 * A função aplica a função recebida nos 
 * outros dois parâmetros de entrada.
 */
def apply(x: Int, y: Int, f: (Int, Int) => Int) = f(x, y)

/**
 * Exemplo B2
 * Função anônima (sem nome)
 * Anonymous Function
 * Recebe dois parâmetros do tipo Int
 * Retorna a soma de ambos.
 */
val sum = (x: Int, y: Int) => x + y

/**
 * Exemplo C2
 * É possível utilizar a declaração de função (def)
 * Possibilita passagem de novos parâmetros.
 * Nesse caso uma função retorna outra função.
 * Curry!
 */
def subAndPlus(z: Int) = (x: Int, y: Int) => (x - y) + z

/**
 * Exemplo D2
 * Apesar da Type Inference, é possível
 * declarar o tipo de de mult explicitamente.
 */
val mult: (Int, Int) => Int = (x: Int, y: Int) => x * y


/**
 * Exemplo E2
 * Função anônima (anonymous function) passada
 * diretamente como parâmetro para apply.
 */
val result1 = apply(2, 3, (x, y) => x + y)

/**
 * Exemplo F2
 * Função nomeada passada
 * como parâmetro para apply.
 */
def sum2(x: Int, y: Int) = x + y
val result2 = apply(2, 4, sum2)

{% endhighlight %}

Com várias formas de se utilizar uma função, é possível estabelecer diversas maneiras de como modelar o seu domínio, resolvendo os problemas de forma mais objetiva.

O artigo está longo, pretendo continuar em outra parte falando mais das funcionalidades. Qualquer correção, sugestão ou crítica deixe nos comentários abaixo.
