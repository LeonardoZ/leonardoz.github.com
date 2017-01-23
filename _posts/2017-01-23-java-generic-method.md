---
layout: post
title: Generic Methods em Java
date: 23/01/2017 16:00:00
disqus: y
share: y
---

**Generic Methods, ou Métodos Genéricos**, são métodos que possuem uma declaração própria de um **Generic Type (Tipo Genérico)**, que é apenas visível dentro do escopo do método em que é declarado.


### Resumo
Em um breve resumo, **Generic Types** são muito utilizados para criação de classes com comportamento genérico, que não dependem exclusivamente de um determinado tipo, pois este é definido no seu momento de instanciação.

A conhecidíssima APIs de Collections faz uso dessa funcionalidade; Muitos padrões como o DAO (Data Access Object) também o fazem.

Se quiseseemos utilizar em alguma classe de nosso projetos, poderiamos utilizar algo nos seguintes moldes:

{% highlight java %}
class Box<T> {

	private T content;

	public Box(T contentParam) {
		this.content = contentParam;
	}

	public T getContent() {
	 	return content;
	}

	public T setContent(T contentParam) {
		this.content = contentParam;
	}

}
{% endhighlight %}

+ A declaração da classe utiliza o Diamond Operator (<> - Operador Diamante);
+ O Valor "T" dentro do operador é apenas um símbolo que será substituído em tempo de compilação;
+ Por conta disso, o desempenho em tempo de execução não é afetado;
+ Outras letras poderiam ser utilizadas, e até mesmo palavras;
+ Outros tipos poderiam serem declados após o T, bastaria apenas separá-los com uma vírgula: ex.: T, U.

A utilização dos **Generic Types** dessa maneira é bastante tradicional e costumeira em projetos Java, pois o escopo de declaração do tipo genérico abrange toda a classe, ou seja, seus atributos e métodos de instância (métodos e atributos estáticos não possuem essa informação, pois a mesma é passada durante a instanciação da classe).

### Generic Methods na prática

Os **Generic Methods** suprem dois problemas específicos:

+ Generics de escopo menor, a nível métodos;
+ Generics para métodos estáticos.

O Generic Method abaixo pertence a classe Box declarada acima. **factory** é um método que facilita a instanciação de Box, e também demonstra como declarar um **Generic Type** em um método, que no caso indifere dele ser de instância ou estático:

{% highlight java %}

public static <T> Box<T> factory(T valueToBuild) { 
	return new Box<T>(valueToBuild);
} 

{% endhighlight %}

Temos algumas regras e sintaxes um pouco diferentes do padrão de declaração dos **Generic Types em classes**, mas vamos explicar:

+ Declaração: Deve ser declarado antes da declaração de **retorno do método** (antes do tipo ou antes de **void**, em caso de não haver retorno no método);
+ Ao ser declarado, pode ser utilizado na **assinatura do retorno do método**, em **parâmetros** e no **corpo do método**, inclusive podendo ser referenciado por outras estruturas que utilizem **Generic Types**;

### Consistência de parâmetros em Generic Methods
{% highlight java %}

/**
* 
* Classe que demonstra o uso de Generic Methods
*
**/
public class MyClass {
	
	// Generic Instance Method
	public <T> void myMethod(T param1, T param2) {
		System.out.println(param1);
		System.out.println(param2);
	}

	// Generic Static Method
	public static <T> void myStaticMethod(T param1, T param2) {
		System.out.println(param1);
		System.out.println(param2);
	}
	
	public static void main(String[] args) {
		MyClass myClass = new MyClass();
		
		// Aceita argumentos de tipos diferentes - Type Inference
		myClass.myMethod(100, "I'm not a Integer");
		
		// Mantém consistência entre os tipos declarados
		myClass.<Integer>myMethod(100, 200);
		
		// Mesmas regras para Métodos Estáticos
		MyClass.myStaticMethod(100, "I'm not a Integer");
		MyClass.<Integer>myStaticMethod(100, 200);
	}

}

{% endhighlight %}

Em um Generic Method com **aridade maior que 1** (com dois ou mais parâmetros), em que os parâmetros requerem um tipo genérico T (por exemplo) declarado localmente, durante a sua utilização o sistema de Type Inference (Inferência de Tipos) consegue descobrir por conta própria o tipo dos parâmetros que estão sendo passados, mas não garantindo a consistência entre os dois tipos informados, portanto, apesar de declaração de T, tipos diferentes podem serem informados.

Para garantir a consistência dos tipos durante a invocação do método, após o ponto e antes do nome do método, devemos informar o **Generic Type** a ser utilizado junto do **diamond operator**. 
ex.: MyClass.<Integer>myMethod(100, 200).

Seguindo essas regras, é possível construir APIs internas mais consistentes e expressivas. Todo cuidado é pouco, pois o abuso dessa funcionalidade pode complicar em excesso seu código.

Os **Generic Methods** são importantes para mantermos nossos **Generic Types** em escopos menores, mantendo nossas classes mais limpas e métodos estáticos mais flexíveis.

Existem outros tópicos como a Variância, Bounded Types e WildCards que fazem parte do assunto, mas que requerem um estudo maior ainda. Mas esses assuntos ficam para uma próxima.
