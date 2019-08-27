---
layout: post
title: Lista Duplamente Encadeada
subtitle: Implementando funções básicas
tags: [linguagem c]
---

Com uma lista simplesmente encadeada podemos formar uma encadeamento simples entre elementos, onde cada elemento armazena um ponteiro que aponta para o próximo elemento. Ela só nos permite o acesso dos elementos apenas em uma direção, sem poder percorrê-los no sentido inverso  

O encadeamento simples também dificulta a retirada de um elemento da lista. Mesmo se tivermos o ponteiro do elemento que desejamos retirar, temos que percorrer a lista, elemento por  elemento, para encontrarmos o elemento anterior, pois, dado um determinado elemento, não temos como acessar diretamente seu elemento anterior.

Porem uma solução seria utilizar uma lista duplamente encadeada no qual cada elemento tem um ponteiro para o próximo elemento e um ponteiro para o elemento anterior.

### Estrutura da Lista Duplamente encadeada
![estrutura-lista-duplamente-encadeada](/img/estrutura-lde.png)

### Funções a ser implementado

{% highlight c linenos %}
Descritor criar_lista( void );
int inserir_cidade( Descritor *, Cidade );
int remover_cidade( Descritor *, int );
int lista_vazia( Descritor * );
int tamanho_lista( Descritor * );
int imprimir_lista( Descritor * );
void destruir_lista( Descritor * );
{% endhighlight %}

### Codigo fonte:
[Lista Duplamente Encadeada em c](/downloadables/lista_duplamente_encadeada.zip)
