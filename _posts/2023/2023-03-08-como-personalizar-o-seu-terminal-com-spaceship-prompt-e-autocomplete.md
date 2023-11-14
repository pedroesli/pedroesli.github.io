---
layout: post
title: Terminal com Spaceship e autocomplete
subtitle: Como personalizar o seu terminal com Spaceship Prompt e autocomplete
tags: [tutorial]
---

# Como personalizar o seu terminal com Spaceship Prompt e autocomplete

Neste post vou mostrar para você como é possível transformar o seu terminal, de um programa chato de usar, para o seu melhor amigo que sempre vai te ajudar. Nesse tutorial vamos utilizar o Spaceship Promp, que é um prompt Zsh minimalista e extremamente personalizável. Prompt é o que você vê quando digita um comando. Ele pode mostrar muitas dicas úteis, economizando seu tempo e tornando a experiência do usuário suave e agradável. 

Realizei este tutorial considerando que você nunca customizou o seu terminal e você não tem muita experiencia usando ele. Então basta seguir passo a passo que você vai ter um terminal top como mostrado em seguida.

### Terminal atual

![terminal preview 1](/assets/img/2023/terminal-preview-1.png)

### Terminal final com Spaceship Prompt

![terminal preview 5](/assets/img/2023/terminal-preview-5.png)

# Passo 1: Instalar Oh My Zsh

[Oh My Zsh](https://ohmyz.sh/#install) é uma framework open source, feita para facilitar a configuração do Zsh.  Para instalar, temos que rodar o seguinte comando no terminal: 

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Dependendo se você não instalou `Command Line Developer Tools` antes, ele vai dar error mas em seguida vai aparecer uma opção para você baixar ele. Aceita a instalação e assim que terminar de instalar, basta rodar o comando novamente no terminal. 

### Oh My Zsh baixado

![terminal preview 2](/assets/img/2023/terminal-preview-2.png)

Oh My Zsh cria um arquivo oculto de configurações onde vamos utilizar em algumas etapas em seguida. para abrir ele podemos rodar o comando:

```bash
open ~/.zshrc
```

Podemos ver que **Oh My Zsh** já vem com um tema padrão (Nome do tema: robbyrussell), você está livre de utilizar apenas ele ou escolher outro tema a partir dessa lista: [Lista de temas](https://github.com/ohmyzsh/ohmyzsh/wiki/Themes). 

Mas nesse tutorial vamos utilizar o **Spaceship Prompt,** já que ele nos possibilita a realizar customizações e deixar do jeito que preferimos. 

# Passo 2: Instalar Homebrew

Para instalar **Spaceship Prompt** vamos utilizar [**Homebrew**](https://brew.sh) que é um sistema de gerenciamento de pacotes de software que simplifica a instalação de software no sistema operacional MacOS. Basta rodar o seguinte comando para instalar: 

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

# Passo 3: Instalar Spaceship Prompt e **a**utocomplete

Para instalar Spaceship Prompt e autocomplete basta rodar os seguintes comandos (um de vez): 

```bash
brew install spaceship
brew install zsh-autocomplete
```

<aside>
💡 Para ver uma lista de pacotes instalados basta rodar `brew list`
</aside>

# Passo 4: Configurar .zshrc

Para ambos pacotes funcionarem no nosso terminal precisamos colocar as seguintes configurações no arquivo .zshrc. Para isso abrimos o arquivo com `open ~/.zshrc` , modificamos o comando ZSH_THEME para `ZSH_THEME="”` , e colamos no final do arquivo: 

```bash
source $(brew --prefix)/share/zsh-autocomplete/zsh-autocomplete.plugin.zsh
source $(brew --prefix)/opt/spaceship/spaceship.zsh
```

# Passo 5: Baixar fonte correta

Você pode ter percebido que alguns ícones não estão aparecendo corretamente. 

![terminal preview 3](/assets/img/2023/terminal-preview-3.png)

Isso é porque não estamos utilizando a fonte correta, para isso basta baixar a fonte [Nerd Font](https://www.nerdfonts.com/font-downloads), ou [FiraCode Nerd Font](https://www.nerdfonts.com/font-downloads) que é o mais popular. Depois de instalar a fonte, volta para o terminal e navega no menu **Terminal>Ajustes…**

Depois em **Perfis>Texto>Fonte>Trocar** e mudar para a fonte instalada.

![terminal preview 4](/assets/img/2023/terminal-preview-4.png)

E agora você tem um terminal mais customizado. Você pode ir mais alem e customizar o seu tema, basta seguir as instruções no site oficial: [Spaceship configuration](https://spaceship-prompt.sh/config/intro/)