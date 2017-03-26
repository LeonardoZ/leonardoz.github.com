---
layout: post
title: Propriedades de Variância em Java
date: 27/03/2017 09:00:00
disqus: y
share: y
---

## Propriedades de Variância em Java

No contexto da programação, **variância** descreve as propriedades de variar na hierarquia de tipos que uma determinada escrutura possui. Em Java, temos exemplos de aplicação das regras de variância na sobescrita de métodos, na declaração de tipos que requerem Generic Type Parameters (List<E>, por exemplo), em Arrays e outras estruturas semânticas que a linguagem oferece.

**Subtipagem** é um termo que indica a existência da capacidade de se criar subtipos na linguagem. Em linguagens Orientadas a Objetos, isso se dá pela Herança ou mecanismos correlatos, como interfaces. Uma das melhores características da Herança não é o reuso de código em sim, mas sim a capacidade de se estabelecer **hierarquias de tipos**. Na maioria das vezes, reuso de código é melhor executado com o uso de composição do que herança. 

Primeiro vamos entender os tipos de variância e depois veremos suas aplicações nas estruturas sintático-semânticas do Java. Existem três tipos de variância: **Covariância**, **Contravariância** e **Invariância**, e sendo assim, uma estrutura pode ser: **Covariante**, **Contravariante** e **Invariante**.

### Covariância

Dado uma relação em que **um tipo A é pai de um tipo B ou A > B**, o tipo mais específico (B) pode substituir o tipo menos específico (A). 
"Mais ou menos especifico" se refere a especialização de um tipo. No Princípio de Substituição de Liskov, de forma simples temos que, dada a mesma relação acima, A > B, o tipo filho B deve fazer no mínimo tudo o que o tipo A fizer para haver a substituição sem perdas. Se B faz tudo que A faz, então ele é A. E sendo B, pode ir além e fazer outras coisas. Esse princípio se aplica na Covariância.

### Contravariância

Dado uma relação em que **um tipo A é pai de um tipo B ou A > B**, o tipo menos específico (B) pode ser substituído pelo tipo (A), considerando apenas o que ambos tem em comum, que é o comportamento de A. É um princípio pouco utilizado, sendo a covariância muito mais comum e até mesmo mais intuitiva.

### Invariância

Dado uma relação em que **um tipo A é pai de um tipo B ou A > B**, não há flexão de tipos, sendo assim, se A for requisitado, apenas A poderá ser entregue, e se B for requisitado, apenas B poderá ser entregue. 

### Sobre aplicações dos conceitos

É importante lembrar que os conceitos vistos em Java não são equivalentes aos de outras linguagens. Cada linguagem tem suas próprias regras em relação a variâncias de suas estruturas. Consulte sempre alguma fonte de infomação boa de sua respectiva linguagem de interesse.

### Propriedades de variância na sobrescrita de métodos

Na sobrescrita de métodos em Java, temos duas propriedades diferentes para dois aspectos diferentes do método: **Covariância para o tipo de retorno** e **invariância para os argumentos do método**.

{% highlight java %}
public class A {
	Number addTo(Integer num) {
		return num + 1;
	}
}
 
public class B extends A {
    // Integer é subtipo de Number
	// O argumento do tipo Integer deve permanecer o mesmo
	@Override
	Integer addTo(Integer num){
		return num + 3;
	}
} 
{% endhighlight %}

### Propriedades de variância em Arrays

O Arrays em Java, além de serem estáticos, são covariantes por natureza. Em suma, dado um Array A[], é possível substituí-lo por um Array de um de seus tipos mais específicos. No entanto, um array de um tipo mais específico, um subtipo, não aceita a inserção de um tipo pai menos específico.

{ % highlight java %}
public class ArrayOperations {

	public static void main(String[] args) {
		A a1 = new A();
		B b1 = new B();
		
		A[] arrayA = new A[3];
		B[] arrayB = new B[3];
		
		// Válidos
		insertInto(arrayA, a1);
		insertInto(arrayA, b1);
		
		/*
		 * Lança java.lang.ArrayStoreException em Runtime
		 * Um B[] não armazena um valor A
		 */
		insertInto(arrayB, a1);
	}

	public static void insertInto(A[] arrayA, A a) {
		arrayA[0] = a;
	}
}
{% endhighlight %}

### Propriedades de variância em Generic Types

No caso dos Generic Types, temos uma gama maior de variações. De forma simples, um tipo container List<A> é invariante, não permitindo em Runtime sua substituição por List<B>, por exemplo. No entanto, os métodos que requerem um tipo A ainda aceitam quando um tipo B é informado, devido ao polimorfismo, ou seja, o Generic Type informado ainda obedece ao polimorfismo normal. A invariância ocorre apenas no estabelecimento da relaçao entre List<A> e List<B>, os tipos que "encaixotam" o Generic Type.

{% highlight java %}
public class GenericsVariance {
	public static void main(String[] args) {
		ArrayList<A> listA = new ArrayList<>();
		ArrayList<B> listB = new ArrayList<>();
		
		// válidos
		listA.add(new A());
		listB.add(new B());
		listA.add(new B());
		insertInto(listA, new A());
		insertInto(listA, new B());
		
		 //Não compila
		 listB.add(new A());
         
		 // não compila - aceita apenas ArrayList<A>
		 listA = listB;
		 insertInto(listB, new A());
		 ArrayList<A> invalidList = new ArrayList<B>();		 
	}
	
    /*
    * list é invariante
    */ 
	public static void insertInto(ArrayList<A> list, A...as) {
		list.addAll(Arrays.asList(as));
	}
}
{% endhighlight %}

### Limitando para flexibilizar

Se uma classe que aceita um Generic Type Parameter, em tese ela aceita qualquer tipo imposto a ela. Por ser genérico demais, não temos informação do tipo que for informado nos escopos (corpo de um método, por exemplo) que trabalharem com o tipo. Sem limitações da abrangência de tipos que pode ser informado, restringe-se o que pode ser feito com o tipo informado. Não há como extrair nada disso. 

Com os **Bounded Type Parameters**, podemos limitar uso dos Generic Types informados em uma determinada hierarquia de tipos, e com essa limitação, temos mais informação, pois agora podemos utilizar os métodos não-estáticos declarados pelo Generic Type que define o limite.

Um **Upper Bound Type** define um limite superior (um tipo pai), e para utilizamos fazemos isso no formato <T extends A> (para covariância), sendo T um indicador do tipo genérico e A um tipo qualquer (poderia ser Pessoa, Estoque, ou qualquer outra classe que você tenha no seu projeto). 

No caso abaixo, o método genérico **print** declara um Upper Bound Type que nos permite passar um ArrayList<A> ou qualquer ArrayList<subtipo de A> como parâmetro.

{% highlight java %}
public class GenericsUpperBoundedTypeVariance {
	public static void main(String[] args) {
		ArrayList<A> listA = new ArrayList<>();
		ArrayList<B> listB = new ArrayList<>();
		
		// válidos
		listB.add(new B());
		
		// aceitas listas diferentes de A, como as de tipo B, que é subtipo de A
		insertInto(listB);
		insertInto(listA);
	}
	
    
    /*
    * list é covariante
    */ 
	public static <T extends A> void print(ArrayList<T> list) {
		for (A a : list) {
			// Podemos acessar o método de A que é comum para toda hierarquia
			System.out.println(a.add3(23));
		}
	}
}
{% endhighlight %}

No caso, **Lower Bounded Types** não são suportados pelo Java, que seria algo como <T super A>. Nesse cenário, ter um Lower Bound seria semelhante a não ter nenhum bound, sendo o Generic Type na prática semelhante a Object. 

### Wildcards

Em Java, o Wildcard é representado por ? e é interpretado como um "Tipo desconhecido", que basicamente aceita qualquer tipo informado. O detalhe é que você não precisa declará-lo como se faz com o Generic Type, então usar ele e suas variações (descritas abaixo) pontualmente em parâmetros pode poupar código, evitando a declaração de Generic Types sem razão. Se você não precisar da informação do Generic Type sendo informado, então você pode utilizar o Wildcard sem problemas.

{% highlight java %}
public static void accept(Class<?> clazz) {...}

accept(Integer.class)
accept(Double.class)
{% endhighlight %}

Já os Bounded Wildcards são mas interessantes. O **Upper Bound Wildcard** tem efeito semelhante ao Upper Bound Type descrito acima. Seu ganho é o mesmo do Wildcard, já que se não precisar declarar explicitamente o Generic Type. Sua sintaxe:

{% highlight java %}
/*
* list é covariante.
* No entanto não há informação do Generic Type informado
*/
public static void print(ArrayList<? extends A> list) {
		for (A a : list) {
			System.out.println(a.add3(23));
		}
}
{% endhighlight %}

Uma grande diferença é que podemos utilizar os **Lower Bounded Wildcards**. Nesse caso, utilizamos quando queremos que nosso método trabalhe com o tipo declarado e seus tipos menos específicos (tipos pai, avô, bisavo, etc).

{% highlight java %}
/*
* list é contravariante
* aceita Double, Number, Object
*/
public static void adiciona(List<? super Double> list) {
    for (int i = 1; i <= 10; i++) {
        list.add(i);
    }
}
{% endhighlight %}

 
### Variância e PECS

Os motivos dessas limitações e de termos de utilizar os Bounded Types para simularmos variância é que certas circunstâncias podem acarretarem problemas, e por isso são restritas. O princípio **PECS** nos auxilia a entender nossos problemas e a quando utilizarmos um Lower ou Upper Bound de maneira correta.

**PECS significa Producer Extends, Consumer Super.**

#### Producer Extends
Quando temos um parâmetro que fornecerá um valor que será útil para nosso método, sendo lido por ele, utilizamos extends.
Ex.: Se precisarmos Iterar um ArrayList, ele está "produzindo" valores. Nesse caso teremos garantia de lermos os valores corretor limitados pelo Bound definido. No entando, a escrita não é garantida.

{% highlight java %}
public class ProducerExtendsDemo {
	/*
	 *  Garante que a list recebida é de Number ou de um subtipo
	 */
	public static void producerParameter(ArrayList<? extends Number> list) {
		// Parâmetro "produz" tipo de acordo com seu Bound
		for (Number number : list) {}
		
		// Não compila - Escrita não é garantida pelo extends
		list.add(10);
	}
	
	public static void main(String[] args) {
		ArrayList<Integer> listInts = new ArrayList<>();
		ArrayList<Double> listDoubles = new ArrayList<>();
		
		// covariância na prática
		producerParameter(listInts);
		producerParameter(listDoubles);
	}
}	
{% endhighlight %}


#### Consumer Super

Quando temos um parâmetro que será utilizado como saída do nosso método, no qual escreveremos valores nele, utilizamos super.
Ex.: Se precisarmos inserir valores em um ArrayList vindo por parâmetro, ele estará consumindo valores. Nesse caso, temos garantia apenas que podemos escrever qualquer tipo definido pelo Bound. Entretanto, consumir a valor da estrutura não será possível, pois não há garantias do que será informado como valor para o ArrayList.

{% highlight java %}
public class ConsumerSuperDemo {
	/*
	 * Garante que a list recebida é de Integer ou de um Ancestral
	 */
	public static void consumerParameter(ArrayList<? super Integer> list) {
		// aceita apenas Integer
		list.add(10);
		
		// Não compila - Leitura não é garantida pela super
		for (Integer i : list) {}
	}
	
	public static void main(String[] args) {
		ArrayList<Number> listNumbers = new ArrayList<>();
		ArrayList<Object> listObjects = new ArrayList<>();
		
		// contravariância na prática
		consumerParameter(listNumbers);
		consumerParameter(listObjects);
	}
}	
{% endhighlight %}


Concluindo, esses são os principais casos de aplicação das propriedades de Variância em Java. Existem outros, e principalmente com o uso de Generics as coisas podem se complicar um pouco mais. Mas por hora acredito que esses são o bastante. Deixemos os outros para depois.




