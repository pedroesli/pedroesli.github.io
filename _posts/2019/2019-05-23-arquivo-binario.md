---
layout: post
title: Arquivo Binário
subtitle: O básico de arquivos em c
date: 2019-05-23 21:42
tags: [arquivo binário]
---
Quando estamos trabalhando com arquivos, existe dois tipos de arquivos que devemos saber:
1. Arquivo de texto
2. Arquivo binário

### 1. Arquivo de texto
Um arquivo de texto é usado para armazenar dados textuais padrão e estruturados ou informações legíveis para humanos.

Eles requer um esforço mínimo pra manter e são facil de ser lidos, mais ocupam um tamanho maior de armazenamento.

O arquivo de texto mais reconhecido é o padrão do Windows ".txt". Outros exemplos são:
- (Linguagem) .c
- .Html
- .css
- .xml

### 2. Arquivo binário
Em vez de guardar os dados em texto simples, eles são guardados em formato de bits. São bastante similares a estruturas de matrizes, exceto as estruturas estão em um arquivo de disco em vez de em uma matriz na memória. Eles consegue manter uma quantidade maior de data e tem uma seguridade melhor do que arquivo de texto.



### Operações de arquivos
Em c, voce consegue realizar quatro operações no arquivo, seja texto ou binário:

1. Criar um novo arquivo
2. Abrir um arquivo existente
3. Fechar o arquivo
4. Ler/Escrever

### Trabalhando com arquivos
Para trabalhar com arquivos em c, voce precisa declarar um ponteiro do tipo FILE.
```
FILE *fptr;
```
### Abrindo o arquivo - para criar e editar

A operação de abrir o arquivo é feito pela biblioteca "stdio.h" utilizado a função fopen(). A sintaxe é:
```
arq = fopen("nomearquivo","modo")
```
Por exemplo:
```
arq = fopen("E:\\cprogram\\newprogram.txt","w");

arq = fopen("E:\\cprogram\\oldprogram.bin","rb");
```
_Modos de abrir o arquivo_

Modo arquivo | Significado  | existência do arquivo |inexistência do arquivo
--|---|--
r  | Abrir para ler | ✓ | fopen() retorna NULL
w  | Abrir  para escrever | Os conteúdos são sobrescritos | Vai criar um novo arquivo
a  | Abrir para acrescentar no fim do arquivo| ✓ | Vai criar um novo arquivo
r+ | Abrir para ler e escrever | ✓ | Retorna NULL
w+ | Abrir para escrever e ler | Os conteúdos são sobrescritos | Vai criar um novo arquivo
a+ | Abrir para acrescentar e ler | ✓ |  Vai criar um novo arquivo
rb | Abrir para ler em binário | ✓ | Retorna NULL
wb | Abrir para escrever em binário | Os conteúdos são sobrescritos | Vai criar um novo arquivo
ab | Abrir para acrescentar no fim do arquivo em binário | ✓ | Vai criar um novo arquivo
rb+| Abrir para ler e escrever em binário | ✓ | Retorna NULL
wb+| Abrir para escrever e ler em binário | Os conteúdos são sobrescritos | Vai criar um novo arquivo
ab+| Abrir para acrescentar e ler em binário | ✓ |  Vai criar um novo arquivo

### Fechando o arquivo
O arquivo (Tanto do tipo texto como binário) deve ser fechado depois de ler/escrever.

A operação deve ser feita utilizado a função fclose(), da seguinte maneira:

```
fclose(arq);// arq é o que aponta para o arquivo.
```

### Ler/Escrever em um arquivo binário
É utilizado as funções fread() e fwrite() para a leitura e escritura em um arquivo binário.

**fread()**

A função recebe 4 argumentos:
```
size_t fread(void *ptr, size_t size, size_t nmemb, FILE *stream)
```
- **ptr** - Ponteiro para o registro a receber o dado lido.
- **size** - Tamanho em bytes do registro.
- **nmemb** - Numero de elementos a ser lidos.
- **stream** - Ponteiro para um objeto FILE que especifica um fluxo de saída.

**fwrite()**

A função recebe 4 argumentos e bem parecida com a anterior:
```
size_t fwrite(const void *ptr, size_t size, size_t nmemb, FILE *stream)
```
- **ptr** - Ponteiro para o registro a ser escrito.
- **size** - Tamanho em bytes do registro.
- **nmemb** - Numero de elementos a ser lidos.
- **stream** - Ponteiro para um objeto FILE que especifica um fluxo de saída.

### Exemplo utilizando as duas funções
{% highlight c linenos %}
#include <stdio.h>

//Crud
//Autor: Pedro Ésli
#define NOM_ARQUIVO "produtos.dat"
struct tProduto{
	int codigo;
	char descricao[100];
	float valor;
};

void incluir(struct tProduto *, char []);
void listar(char []);
int menu();
int main(){
	int opcao;
	struct tProduto produto;
	do{
		opcao = menu();
		switch(opcao){
			case 1:
				printf("--Incluir--\n\n");
				printf("Digite o codigo: ");
				scanf("%d",&(produto.codigo));
				printf("Digite a descricao: ");
				fflush(stdin);
				gets(produto.descricao);
				printf("Digite o valor: ");
				scanf("%f",&(produto.valor));

				incluir(&produto,NOM_ARQUIVO);
				break;
			case 2:
				printf("--Listar--\n\n");
				listar(NOM_ARQUIVO);
				break;
		}
	}while(opcao!=3);
}

void incluir(struct tProduto *produto, char nomeArquivo[]){
	FILE *arq = fopen(nomeArquivo,"ab");
	fwrite(produto,sizeof(struct tProduto),1,arq);
	fclose(arq);
}
void listar(char nomeArquivo[]){
	FILE *arq = fopen(nomeArquivo,"rb");
	struct tProduto produto;
	while(fread(&produto,sizeof(struct tProduto),1,arq))
		printf("%d - %s - %.2f\n",produto.codigo,produto.descricao,produto.valor);
	fclose(arq);
}
int menu(){
	int opcao;
	printf("\n(1)...Incluir\n");
	printf("(2)...Listar\n");
	printf("(3)...Sair\n");
	scanf("%d",&opcao);
	return opcao;
}
{% endhighlight %}
___
_Bibliografia_

[The Basics of C Programming](https://computer.howstuffworks.com/c39.htm)

[C Programming Files I/O](https://www.programiz.com/c-programming/c-file-input-output)
