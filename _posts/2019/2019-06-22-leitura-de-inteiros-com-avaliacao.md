---
layout: post
title: Leitura de inteiros com avaliação
subtitle: avaliação utilizado cadeias de caracteres
tags: [linguagem c]
---
Digamos que em um programa o usuário digitasse um valor não desejado e isso causasse algum erro no nosso programa. Exemplo de entradas não desejado:  

- 221ca
- abcabc

Uma simples solução seria simplesmente não aceitar valores diferentes de números. Fiz uma função que realiza isso.

{% highlight c linenos %}
#include <stdlib.h>
#include <conio.h>
int lerInt();
int main(){
	int a;
	a = lerInt();
	printf("\n%i\n",a);
	return 0;
}
int lerInt(){
	char n,*num,i=0;
	do{
		n = getch();
		if((n>=48 && n<=57) || n == 8){//Aceitar somente dijitos que são numeros e backspace
			if(n == 8){// Remover ultimo digito
				printf("\b \b");
				i--;
			}
			else{
				putchar(n);
				num[i++] = n;
			}
		}
	}while(n!=13);
	num[i] = '\0';// Colocar o terminador de string
	return atoi(num);//Converte um string para inteiro
}
{% endhighlight %}
