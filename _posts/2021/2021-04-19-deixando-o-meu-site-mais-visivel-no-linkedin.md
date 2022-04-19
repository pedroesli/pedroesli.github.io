---
layout: post
title: Deixando o meu site mais visível no Linkedin
subtitle: Como somar os dígitos do numero de um fatorial?
cover-img: /img/cover-pink.png
black-header: true
comments: true
css: "/css/post.css"
tags: [improvement]
---

# Deixando o meu site mais visível no Linkedin

Então, no outro dia eu tava dando uma olhada melhor no perfil do meu LinkedIn e vi algo que podia ser melhorado. No destaque eu tenho um link redirecionando para o meu site.

![before update](/img/2021/april/before-update.png)

Pois, com meta tags é claro. Mais especificamente usando o tal de **Open Graph Protocol (OGP),** que para quem não sabe, foi criado pelo Facebook em 2010 e agora gerenciado pelo **Open Web Foundation.** E ele permite controlar quais informações são usadas quando um site é compartilhado. Linkedin utiliza exatamente esse protocolo, como mostrado na [ajuda](https://www.linkedin.com/help/linkedin/answer/a521928/making-your-website-shareable-on-linkedin?lang=en) deles. E para utilizar esse protocolo precisamos colocar meta tags do **Open Graph** na parte do `<head>` do nosso site.

Aqui um exemplo tirado do site do protocolo ([https://ogp.me](https://ogp.me/)):

```html
<html prefix="og: https://ogp.me/ns#">
<head>
<title>The Rock (1996)</title>
<meta property="og:title" content="The Rock" />
<meta property="og:type" content="video.movie" />
<meta property="og:url" content="https://www.imdb.com/title/tt0117500/" />
<meta property="og:image" content="https://ia.media-imdb.com/images/rock.jpg" />
...
</head>
...
</html>
```

# Então vou colocar no meu site.

A tag que estou mais interessado é a imagem e editar a descrição. Então primeiramente vou ter que encontrar a pagina do meu site pra fazer essas mudanças.

![find meta tag](/img/2021/april/find-meta-tag.png)

E como eu mudo isso... não parece ser como o é exemplo anterior. Bom, primeiramente no meu caso como eu estou utilizando uma framework chamado de Jekyll. Uma Framework que facilita a criação de site blogs estáticos, e não precisa de um banco de dados como Wordpress para guardar os textos.

Então, devo utilizar a variável `share-img` na configuração da pagina do projeto (`projects.md`).

**OBS:** Se você estiver utilizando uma Framework diferente, você deverá consultar no site dele.

Então eu preparo uma imagem de preview (Só tirei uma screenshot do meu site) e redimensiono ele para o tamanho certo.

### Sem a variável

```html
---
layout: page
title: Projects
subtitle: Projects that where developed by me. Enjoy!
css: "/css/project.css"
---
```

### Colocando a variável

```html
---
layout: page
title: Projects
subtitle: Projects that where developed by me. Enjoy!
share-img: https://pedroesli.com/img/projects-preview.png
css: "/css/project.css"
---
```

# Resultado Final

![final result](/img/2021/april/final-result.png)

Para o título e a descrição acabei mudando no próprio site do Linkedin.

Você pode testar se a tag tá funcionando no Linkedin usando este site oficial deles: [https://www.linkedin.com/post-inspector/inspect/](https://www.linkedin.com/post-inspector/inspect/)

Porque no Linkedin a mudança da imagem não é imediata e pode demorar um pouco, então utilizando esse site é possível verificar se tá funcionando certo.

E seria isso. Para deixar ele um pouquinho mais visível tive que passar por todo esse trabalho. Más eu amo fazer isso. E o meu perfil fica bem mais profissional.

Da uma olhada no meu perfil do Linkedin: [pedroesli](https://www.linkedin.com/in/pedroesli/)
