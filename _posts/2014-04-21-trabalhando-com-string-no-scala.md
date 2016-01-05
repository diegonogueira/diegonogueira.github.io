---
layout:     post
title:      Trabalhando com String no Scala
date:       2014-04-21 12:00:00
categories: Scala
---

Neste post iremos aprender a trabalhar um pouco com String no Scala.
## Declaração
Já vimos nos posts anteriores como declaramos uma variável do tipo String. Podemos fazer de duas formas basicamente:

1) Especificando o Tipo

{% highlight scala %}
val name: String = "Diego"
{% endhighlight %}

2) Omitindo o tipo, deixando que o Scala determine automaticamente o tipo.

{% highlight scala %}
val name = "Diego"
{% endhighlight %}

## Funções úteis
#### Capturando um caracter dentro de uma StringPara capturar um caracter, basta informar o indice que o caracter está posicionado. Podemos fazer isso de 3 formas:

{% highlight scala %}
val name = "Diego"
println(name(0))
println(name(1))
println(name(2))
{% endhighlight %}

<!--more-->
Utilizando o método __apply__:

{% highlight scala %}
val name = "Diego"
println(name.apply(0))
println(name.apply(1))
println(name.apply(2))
{% endhighlight %}

Ou utilizando o método __charAt__:

{% highlight scala %}
val name = "Diego"
println(name.charAt(0))
println(name.charAt(1))
println(name.charAt(2))
{% endhighlight %}

#### Transformando o primeiro caracter em maiúsculo
Para isso iremos utilizar o método __capitalize__:

{% highlight scala %}
val name = "diego"
println(name.capitalize)
{% endhighlight %}

#### Comparando Strings
Para compararmos exatamente duas Strings, com distinção de letras maiúsculas e minúsculas, utilizamos o método __==__ ou __compareTo__:

{% highlight scala %}
"Diego" == "diego" // false
"Diego".compareTo("diego") // -32

"Diego" == "Diego" // true
"Diego".compareTo("Diego") // 0
{% endhighlight %}

Reparem que o método __compareTo__ retorna um __Int__, onde se for igual a 0 quer dizer que as Strings são iguais.

Podemos também comparar duas Strings, __ignorando__ letras maiúsculas e minúsculas:

{% highlight scala %}
"Diego".compareToIgnoreCase("diego") // 0

"Diego".compareToIgnoreCase("Nogueira") // -10
{% endhighlight %}

Seguindo a mesma idéia do método __compareTo__, onde 0, significa que as Strings são iguais.
#### Concatenando Strings
Podemos fazer de algumas formas:

Utilizando o método __+__:

{% highlight scala %}
"Diego" + " " + "Nogueira"
{% endhighlight %}

Utilizando o método __concat__:

{% highlight scala %}
"Diego".concat(" ").concat("Nogueira")
{% endhighlight %}

Ou utilizando interpolação:

{% highlight scala %}
val first_name = "Diego"
val last_name = "Nogueira"

val full_name = s"$first_name $last_name"
{% endhighlight %}

Para trabalharmos com interpolação, basta adicionar __s__ antes da string e concatenar utilizando o caracter __$__. Podemos também utilizar blocos de código __${ }__ para fazer a interpolação:

{% highlight scala %}
val first_name = "Diego"
val last_name = "Nogueira"

val full_name = s"${ first_name + " " + last_name } - ${ 1 + 1 }"
{% endhighlight %}

#### Bloco de String
Muitas vezes precisamos definir uma String um tanto grande. Colocar tudo na mesma linha, sem nenhuma indentação, pode ser uma tarefa meio complicada de se escrever e de dar manutenção futuramente. O Scala assim como outras linguagens, permitem definirmos um bloco de String, vejamos:

{% highlight scala %}
val description = """
 Essa
   é
     uma
       demostração
         de String
"""
{% endhighlight %}

Delimitamos nosso bloco de String, utilizando três aspas __"""__. Note que ele irá respeitar a marge que definimos.

Podemos também, informar qual é o nosso delimitador de margem, com o método __stripMargin__:

{% highlight scala %}
val description = """
 |Essa
  |é
    |uma
      |demostração
        |de String
""".stripMargin
{% endhighlight %}

O método stripMargin é responsável por contar as margens da nossa String. Vejam que nossa String possui vários caracteres "|". São eles que indicam até onde será cortado cada linha de nossa String.
#### Checando a existência de caracteres numa String
Para verificarmos se existem um ou mais caracteres dentro de uma String, utilizamos o método __contains__:

{% highlight scala %}
val name = "Diego Nogueira"
name.contains("go") // true

name.contains("Go") // false
{% endhighlight %}

Note que o método contains faz distinção de letras maiúsculas e minúsculas.
#### Contando quantos caracteres existem em uma String
Utilizamos o método __length__:

{% highlight scala %}
"Diego".length
{% endhighlight %}

#### Removendo caracteres duplicados em uma String
Podemos utilizar o método __distinct__ para remover os caracteres duplicados:

{% highlight scala %}
"abbca".distinct
{% endhighlight %}

#### Removendo caracteres de uma String
O método drop, permite que removamos da esquerda pra direita, uma certa quantidades de caracteres informada via parâmetro, vejamos:

{% highlight scala %}
"Diego Nogueira".drop(1)
"Diego Nogueira".drop(2)
"Diego Nogueira".drop(5)
{% endhighlight %}

Já o método dropRight, remove caracteres da direita pra esquerda:

{% highlight scala %}
"Diego Nogueira".dropRight(1)
"Diego Nogueira".dropRight(2)
"Diego Nogueira".dropRight(5)
{% endhighlight %}

#### Varrendo caracter a caracter em uma String
Podemos utilizar a estrutura __foreach__ para varrermos caracter a caracter dentro de uma String:

{% highlight scala %}
"Diego Nogueira".foreach(println(_))
{% endhighlight %}

Veja que o símbolo _, representa o caracter em questão. Podemos omití-lo também, pois o foreach irá entender da mesma forma:

{% highlight scala %}
"Diego Nogueira".foreach(println)
{% endhighlight %}

#### Capturando o primeiro e último caracter de uma String
Utilizamos o método __head__ para capturarmos o primeiro caracter. E o método __last__ para o último:

{% highlight scala %}
"Diego Nogueira".head
"Diego Nogueira".last
{% endhighlight %}

#### Outros métodos
É isso ai galera! Esse post foi só uma introdução de como trabalhar com Strings, não irei abordar todos os métodos, até porque são vários... Para conhecer os métodos, bastar conferir a documentação oficial do Scala:

http://www.scala-lang.org/api/current/#scala.collection.mutable.StringBuilder

Até a próxima!
