---
layout:     post
title:      Declarando variáveis no Scala
date:       2014-04-09 12:00:00
categories: Scala
---

Vamos colocar a mão na massa agora! Como primeiro assunto, iremos aprender como funciona a declaração de variáveis no Scala. Entre no console do Scala e vamos estudar!

## Declaração
[__escopo__] [__tipo da acesso__] [__nome da variável__]__:__ [__tipo da variável__] = [__Valor padrão__]

Exemplo:

{% highlight scala %}
private val name: String = "Diego"
{% endhighlight %}

Essa é a forma completa de declaração, porém não precisamos declarar sempre a forma completa, podemos omitir alguns parâmetros. Mas antes disso iremos entender cada parte da declaração completa.
### __1) Escopo__
Escopo de acesso da sua variável, podendo ser:
####__a) public__
Será uma variável pública, ou seja, será acessível dentro de sua própria classe, instancias dessa classe e subclasses. Esse é o escopo default na declaração de variável no Scala, então podemos omiti-lo.
<!--more-->
{% highlight scala %}
class User {
  var name: String = ""
  def getName = name
}

// Aqui criamos uma classe Employee que herda de User
class Employee extends User {
  def getFirstLetter = name(0)
}

val user = new User

user.name = "Diego"

println(user.name)
println(user.getName)

val employee = new Employee

employee.name = "Diego"

println(employee.name)
println(employee.getName)
println(employee.getFirstLetter)
{% endhighlight %}

Pontos a observar:

* Utilizamos __extends__ para herdar uma classe
* O Scala criou automaticamente os métodos __get/set__ para a variável __name__
* Uma instância de __User__ tem acesso ao __get/set__ gerado pelo Scala
* Uma instância de __Employee__, que herda de __User__, também tem acesso ao __get/set__ gerado pelo Scala

####__b) private__

Será uma variável privada, ou seja, somente os métodos da classe em questão terão acesso a ela, nem uma subclasse terá acesso.

{% highlight scala %}
class User {
  private var name: String = ""

  def getName = name
  def setName(s: String) = name = s
}

class Employee extends User {
  def getFirstLetter = name(0) // Erro
}

val user = new User

user.name = "Diego" // Erro
user.setName("Diego")

println(user.name) // Erro
println(user.getName)
{% endhighlight %}

Pontos a observar:

* O Scala criou automaticamente os métodos __get/set__ para a variável __name__ somente dentro da classe User
* Uma instância de __User__ NÃO tem acesso ao __get/set__ gerado pelo Scala
* A classe Employee, que herda de User, NÃO tem acesso ao __get/set__ gerado pelo Scala
* Uma instância de __Employee__, que herda de __User__, também NÃO tem acesso ao __get/set__ gerado pelo Scala

####__c) protected__

Será uma variável protegida, ou seja, somente poderá ser acessada em sua própria classe ou subclasse. Suas instancias assim como o private não terão acesso a mesma.

{% highlight scala %}
class User {
  protected var name: String = ""

  def getName = name
  def setName(s: String) = name = s
}

class Employee extends User {
  def getFirstLetter = name(0)
}

val user = new User

user.name = "Diego" // Erro
user.setName("Diego")

println(user.name) // Erro
println(user.getName)

val employee = new Employee

employee.name = "Diego" // Erro
employee.setName("Diego")

println(employee.name) // Erro
println(employee.getName)
println(employee.getFirstLetter)
{% endhighlight %}

Pontos a observar:

* O Scala criou automaticamente os métodos __get/set__ para a variável __name__ dentro da classe User
* Uma instância de __User__ NÃO tem acesso ao __get/set__ gerado pelo Scala
* A classe Employee, que herda de User, tem acesso ao __get/set__ gerado pelo Scala
* Uma instância de __Employee__, que herda de __User__, NÃO tem acesso ao __get/set__ gerado pelo Scala

### __2) Tipo de acesso__

O tipo de acesso definimos se nossa variável é mutável ou imutável, ou seja, quando imutável, temos seu comportamento como uma constante.

####__a) val__
Utilizado para deixar a variável imutável. Uma vez o valor definido, não poderá ser mais alterado.

{% highlight scala %}
val name = "Diego"
name = "Nogueira" // Erro
println(name)
{% endhighlight %}

####__b) var__

Utilizado para deixar a variável mutável. Podemos definir e re-definir a qualquer momento o valor da variável.

{% highlight scala %}
var name = "Diego"
name = "Nogueira"
println(name)
{% endhighlight %}

####__Qual devemos utilizar?__
Devemos tentar utilizar ao máximo o __val__, assim nos beneficiaremos da programação funcional, que é um dos pontos fortes da linguagem, além de ser extremamente recomendado pela comunidade do Scala.

### __3) Nome da variável__

Nome da variável em questão. Devemos ficar atentos aos nomes das variáveis em nossa aplicação, utilize sempre nomes façam sentido para as quais as variáveis foram criadas. Caso o nome de uma variável seja muito grande, você pode abreviar, mas sempre mantendo a coesão.

### __4) Tipo da variável__

Assim como em outras linguagens, o Scala possui todos os tipos padrões: String, Int, Double, Boolean entre muitos outros. Além do benefício de podermos utilizar variáveis do Java:

{% highlight scala %}
val name: String = "Diego"
name.getClass

val total: Int = 100
total.getClass

val closed: Boolean = true
closed.getClass

val name: java.lang.String = "Diego"
name.getClass
{% endhighlight %}

Quando o tipo da variável for omitido, o Scala irá atribuir um tipo baseando-se no valor que foi passado.

{% highlight scala %}
val name = "Diego"
name.getClass

val total = 100
total.getClass

val price = 1.0
price.getClass

val closed = true
closed.getClass
{% endhighlight %}

### __4) Valor padrão__

Por último, podemos informar algum valor padrão para nossa variável. Note que caso a variável seja declarada utilizando o __val__, esse será seu valor único, pois variáveis declaradas com val não podem ser alteradas.

####__Declaração múltipla__

Podemos definir várias variáveis ao mesmo tempo e informando um valor padrão pra elas também.

{% highlight scala %}
val variable1, variable2 = 100
{% endhighlight %}

####__Lazy (variável preguiçosa)__

Um recurso bastante interessante é o lazy, quando informado na declaração de uma variável, faz com que o valor padrão somente seja executado no momento da primeira chamada a variável. Vamos ver como funciona:

Sem lazy
{% highlight scala %}
val numbers = for(i <- 1 to 10) yield i
for(e <- numbers) println(e)
{% endhighlight %}

Repare que no momento que declaramos a variável __numbers__, automaticamente foi criado um vetor com o retorno do nosso loop __for__. Agora iremos criar o mesmo exemplo utilizando o __lazy__.

Com lazy
{% highlight scala %}
lazy val numbers = for(i <- 1 to 10) yield i
for(e <- numbers) println(e)
{% endhighlight %}

Agora utilizando __lazy__, somente foi processado nosso loop __for__ no momento em que chamamos a variável. Isso é bastante útil, pois pode evitar um processamento desnecessário.

Bem, acho que é isso aí!
Até o próximo post!
