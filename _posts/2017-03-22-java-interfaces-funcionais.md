---
layout: post
title: Java 8 e suas Interfaces Funcionais
date: 22/03/2017 09:30:00
disqus: y
share: y
---

### Atenção!
Esse post requer conhecimento nos seguintes tópicos em Java:
* Classes anônimas;
* Genéricos;
* Expressões lambda.

No Java 8 tivemos adições interessantes na linguagem Java, e dentre elas destaco as Expressões Lambdas e as Interfaces Funcionais (Functional Interfaces, conhecidas também por Single Abstract Method Interfaces).

Uma interface funcional é uma interface como já conhecemos, com a diferença que ela possui apenas um método abstrato, mas podendo ter quantos métodos “default” (métodos com corpo implementado) elas quiserem. Normalmente, para fins de declaração de intenção e design apenas, essa interface possuí a anotação @FunctionalInterface, que não é obrigatória. 
{% highlight java %}
@FunctionalInterface
public interface SouFuncional {

	void tenhoApenasUmMetoadoAbstratoDeclarado();
	
	default String ePossoTerVariosMetodosDefault() {
		return "Sou Funcional";
	}
}
{% endhighlight %}

Interfaces como a conhecida Runnable são consideradas interfaces funcionais também, por terem apenas um método abstrato declarado.

As expressões lambdas em Java permitem implementações mais concísas de uma Interface Funcional, evitando a necessidade de utilizar instâncias das interfaces como classes anônimas. Em suma, expressões lambdas aceitam apenas interfaces funcionais. Existem vários materiais sobre essas novidades, portanto, focarei no assunto principal: As funções preestabelecidas da pacote **java.util.function**.

Considero conhecer as funções desse pacote um fator essêncial para se entender os aspectos funcionais do Java 8. As funções possuem características únicas e são utilizadas em algumas APIs Java, e provavelmente serão utilizadas por frameworks daqui em diante. Portanto, é hora de conhecê-las e ver como elas se parecem.

## Estrutura de descrição das Interfaces Funcionais:

### NomeDaFunção<Tipos Parametrizados Genericos> (breve tradução)
**Parâmetros:** Listagem dos parâmetros (Tipo, Tipo...)

**Retorno:** O retorno da função (Tipo), quando houver

**Funcionalidade:** Breve descrição de como a função funciona e como por ser utilizada.

**Lambda:** 
{% highlight java %}
Trecho de codigo Java demonstrando a estrutura no formato de Lambda da função, usa exemplos da API básica que qualquer novato pode reconhecer. Não usei a inferência de tipos para não complicar a leitura sem contexto.
{% endhighlight %}

## Listagem das principais Interfaces Funcionais:

### Consumer<T> (Consumidor)
**Parâmetro:** T

**Retorno:** Nenhum

**Funcionalidade:** Interface que pede um parâmetro do tipo genérico T e não retorna nenhum resultado. Utilizada quando será executada alguma ação em que o feedback (retorno) não será necessário.

**Lambda:**
{% highlight java %}
(String valor) -> metodoSemRetornoQueProcessaValor(valor);
{% endhighlight %}


### BiConsumer<T, U> (Consumidor de dois parâmetros)
**Parâmetros:** T e U

**Retorno:** Nenhum

**Funcionalidade:** Função que pede dois parâmetros, de tipos genéricos, que podem ser diferentes (T e U), e que não retornam nenhum resultado. Possuí a mesma finalidade de Consumer<T>, com a diferença da aridade (quantia e tipos dos parâmetros).

**Lambda:**
{% highlight java %}
(String aluno, Double nota) -> metodoSemRetornoQueProcessaNota(aluno, nota);
{% endhighlight %}


### Supplier<T> (Fornecedor)
**Parâmetros:** Nenhum.

**Retorno:** T

**Funcionalidade:** Função que fornece um valor qualquer do tipo T, e que não pede nenhum parâmetro. Geralmente utilizada para fornecimento de alguma instância de uma classe qualquer, por exemplo.

**Lambda:**
{% highlight java %}
// expressões lambda de uma linha não requerem o uso da keyword return 
() -> new ArrayList<>();
{% endhighlight %}


### Function<T, R> (Função)
**Parâmetro:** T

**Retorno:** R

**Funcionalidade:** Função que aceita um parâmetro do tipo T e retorna um valor do tipo R. Pode ser utilizada em vários cenários diferentes, em qualquer um que obedecer a regra "Recebe x, devolve y".

**Lambda:**
{% highlight java %}
(String informacao) -> metodoQueProduzConexaoComBancoDeDados(informacao);
{% endhighlight %}


### BiFunction<T, U, R> (Função de dois parâmetros)
**Parâmetros:** T, U

**Retorno:** R

**Funcionalidade:** Função que aceita dois parâmetros, um do tipo T e outro do tipo U, e que retorna um valor do tipo R. Segue a mesma regra de Function, com diferenças na aridade da função.

**Lambda:**
{% highlight java %}
(String valor, Double num) -> metodoQueConverteValorESomaComNum(valor, num);
{% endhighlight %}


### UnaryOperator<T> (Operador Unário)
**Parâmetro:** T

**Retorno:** T

**Funcionalidade:** Possui um único parâmetro do tipo T e o retorno é do mesmo tipo T. Forma especializada de Function que força a tipagem, fazendo tanto o parâmetro de entrada quanto o resultado de saída serem do mesmo tipo.

**Lambda:**
{% highlight java %}
(Double numero) -> metodoQueRetornaNumeroAoCubo(numero);
{% endhighlight %}


### BinaryOperator<T>
**Parâmetro:** T

**Retorno:** T

**Funcionalidade:** Forma especializada de UnaryOperator<T>. Possui dois parâmetros do tipo T e o retorno é do mesmo tipo T.
**Lambda:**
{% highlight java %}
(Double num1, Double num2) - > num1 + num2;
{% endhighlight %}


### Predicate<T> (Predicado)
**Parâmetro:** T

**Retorno:** boolean

**Funcionalidade:** Função que recebe um parâmetro do tipo T e que deve retornar um valor verdadeiro ou falso. Utilizada em filtros, ou em qualquer situação em que for necessário classificar um valor (T) como verdadeiro ou falso.

**Lambda:**
{% highlight java %}
(String cpf) -> metodoQueAvaliaValidadeDoCpf(cpf);
{% endhighlight %}


### BiPredicate<T> (Predicado com dois parâmetros)
**Parâmetros:** T, U

**Retorno:** boolean

**Funcionalidade:** Função que recebe dois parâmetros, um do tipo T e outro do tipo U, e que deve retornar um valor verdadeiro ou falso. Forma especializada do Predicate.

**Lambda:**
{% highlight java %}
(String login, byte[] senha) -> metodoQueRetornaBoolean(login, senha);
{% endhighlight %}


Muitas dessas Interfaces Funcionais possuem métodos defualt com outros utilidades.

O principal caso de uso atualmente dessas interfaces está presente nas Collections do Java, que ganharam um novo método chamado **stream()**, que cria um pipeline de processamento dos elementos da coleção, sem alterações na coleção original; contém operações de processamento de elementos (filter, map, flatMap), podendo estas serem encadeadas, e operações de termino (collect, reduce) que finalizam o processamento do stream. Um ponto chave para dominar essa API é compreender fluentemente o uso das Interfaces Funcionais descritas.

As Interfaces Funcionais listadas também podem ser utilizadas em suas próprias APIs ou Bibliotecas, desde que se entenda perfeitamente seu caso de uso.

Existem outras funções nesse pacote e sugiro que você dê uma olhada com atenção. Existem versões otimizadas para determinados tipos numéricos que podem ser de grande valia. As que descrevi acima são generalistas, mas em determinados casos um função específca para Inteiros, por exemplo, pode ser a melhor opção.

Como último conselho, estude bem, compreenda, e só assim passe a utilizá-las em seus projetos.


