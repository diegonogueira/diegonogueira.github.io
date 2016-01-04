---
layout:     post
title:      Entendendo o Implicit
date:       2014-04-29 12:00:00
categories: Scala
---

Ao nos depararmos com __implicit__ a primeira vez, pode ser algo um pouco confuso. Vou tentar explicar e exemplificar o uso de implicits.

Quando queremos definir uma variável ou método como privado/protegido, basta somente adicionarmos __private__/__protected__ na frente, certo? Isso informa ao compilador que teremos aquelas variáveis e métodos privados ou protegidos. Trabalhamos com __implicit__ da mesma forma, sendo que o seu funcionamento é completamente diferente. Iremos ver 3 maneiras de utilizar o __implicit__, mas em todos os casos com o mesmo objetivo, tornar algo implícito!

Antes de entrarmos a fundo no assunto, precisamos entender como funciona o implicit.

O implicit de maneira geral funciona da mesma forma. Quando definimos algo como implícito, o compilador registra tudo que é implícito em uma lista de __"coisas implícitas"__. A partir daí, quando invocamos um método qualquer, por exemplo, o compilador sempre irá verificar em sua lista os objetos implícitos, caso algum atenda a sua condição implícita ele irá invocar o que foi definido como implícito.

Confuso? A baixo iremos ver na prática como o implicit funciona:

# Classes implícitas

O primeiro tipo de implicit que iremos aprender é o implicit de classe. Com ele conseguimos de maneira fácil adicionar ou modificar o comportamento de uma classe, seja em seus atributos ou métodos. 
<!--more-->

### Adicionando um método na classe String

{% highlight scala %}
implicit class MyString(s: String) {
  def my_method = s + " !!!!"
}

val s = "Olá"
s.my_method
{% endhighlight %}

__O que aconteceu aqui?__

Primeiro criamos uma classe(MyString) que tem o mesmo construtor de uma classe String, onde recebe uma String como parâmetro.
Ao criar um objeto String e chamar o método "meu_metodo", o scala irá verificar se o "meu_metodo" pertence a classe nativa String, caso não encontre, o Scala irá pesquisar nos métodos de conversão implícitos(lista de coisas implícitas) e irá ver que o o método recebe uma String e possui um metodo "meu_metodo".

Isso funciona bem, porém pode ser um pouco limitado ao seu escopo, onde esta declarado. Para resolver isso, podemos colocar nossa implicit class dentro de um objeto e importar.

{% highlight scala %}
package com.projeto.utils

object NewString {
  implicit class MinhaString(s: String) {
    def meu_metodo = s + " !!!!"
  }
}
{% endhighlight %}

Agora podemos trabalhar de forma mais completa, apenas importando o objeto.

{% highlight scala %}
import com.projeto.utils.NewString._

val s = "Olá"
s.meu_metodo
{% endhighlight %}

# Métodos implícitos

Antes do Scala 2.10, não existia o implicit class. Porém podemos nos beneficiar do implicit, mas com o implicit def.

O que acontece se tentarmos atribuir um objeto double em uma variavel int?

{% highlight scala %}
val i: Int = 2.30 // Erro
{% endhighlight %}

Da erro né? Porém podemos criar um método implicito que diz que, se for atribuido algum valor double a uma variável int, ele deve automaticamente converter o double para int. vejam?

{% highlight scala %}
implicit def doubleToInt(d: Double) = d.toInt
{% endhighlight %}

Pronto, criamos a conversão, agora experimente rodar o código acima que deu erro:

{% highlight scala %}
val i: Int = 2.30 // Foi!!
{% endhighlight %}

Agora sim! O que aconteceu foi o seguinte: Ao tentar atribuir um valor do tipo Doble em uma variável Int, naturalmente são tipos diferentes, o Scala iria lançar uma exceção. Porém, antes de lançar uma exceção, ele vai buscar primeiro se não existe nenhum método implícito que recebe um double e retorne um Int. 

Entenda da seguinte maneira. Ao tentarmos adicionar uma variavel do tipo double em uma do tipo int, um bub, porem o scala primeiro verifica se há alguma vacina(correção) para caso tenhamos esse bug.

### Adicionando métodos sem implicit class

{% highlight scala %}
class NewString(val s: String) {
  def meu_metodo = s + " !!!!"
}

implicit def meu_metodo_implicit(s: String) = new NewString(s)

val a = "Diego"
a.meu_metodo
{% endhighlight %}

# Parâmetros implícitos

Permite referenciar um valor para um parâmetro do método em questão. Caso seja passado algum valor para o parametro, este sempre terá prioridade sobre o implicit.

{% highlight scala %}
def my_name(n: String) = println("my name is " + n + " !!!")

my_name("diego")
my_name // Erro!
{% endhighlight %}

Definimos um método __my_name__ que recebe uma String como parâmetro e depois exibe uma String como retorno. A primeira chamada do método funciona como esperado, já a segunda da um erro, por conta de não termos passado uma String como esperada no método.
Agora vamos utilizar um parâmetro implícito:

{% highlight scala %}
def my_name(implicit n: String) = println("my name is " + n + " !!!")

implicit val name = "Nome implícito"

my_name("diego")
my_name // Não da mais erro!
{% endhighlight %}

Agora podemos chamar nossa função __my_name__ sem nenhum parâmetro!

### Mas porque funcionou agora?

Ao definir o parâmetro como implícito, ele nos permite definir fora de seu escopo, variáveis implícitas também. Como o parâmetro é do tipo String, qualquer variável do tipo String que definírmos como implicit, irá servir de valor default para o parâmetro, caso não seja passado nenhum valor no momento da chamada do método.

Um exemplo de utilização do implicit, está no [http://www.playframework.com](http://www.playframework.com). Quando criamos uma Action em um controller nosso, é uma boa prática utilizar o __implicit request__, pois em qualquer lugar que precise utilizar o request em questão, irá utilizar o request já alocado.

{% highlight scala %}
  def index = Action { implicit request =>
    Ok(views.html.home.index())
  }
{% endhighlight %}

É isso ai, até a próxima!
