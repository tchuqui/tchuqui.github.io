---
title: "Sincronizando o cofre do Obsidian com Iphone utilizando o Github"
description: "Já imaginou sincronizar o repositório do Obsidian de sua máquina com o Iphone sem pagar nada? Neste post vou mostrar como é possível fazer isso utilizando apenas 3 aplicativos em seu Iphone: ISH (terminal), Icloud e Obsidian"
date: 2025-04-19 21:30:00 -0400
image:
  path: "/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/obsidian_git.png"
  alt: "Sincronizar cofre do Obsidian com o Github no Iphone de graça"
  hide: false
categories: Obsidian
tags:
  - obsidian
  - github
  - git
  - iphone
  - linux
  - ish
--- 

O [Obsidian](https://obsidian.md/) é um software de `base de conhecimento` pessoal que funciona como um segundo cérebro. Sua função é registrar notas que podem ter um relacionamentos para facilitar a organização de idéias. 

<!-- quotes -->

> Além de notas em markdown[^1], existem plugins com funcionalidades como Canvas, [Excalidraw](https://excalidraw.com/), Kanban e outros recursos alimentados pela comunidade.

<!-- references -->

[^1]: Markdown é um formato de simples de markup, isso é, de marcação de texto. A ideia é marcar um texto informando o que é importante, o que é um tópico, o que são links e imagens, sem a necessidade de utilizar marcações mais complexas, como o HTML.

O software opera a partir de uma pasta de documentos textos no formato `markdown`, a qual, dentro do **Obsidian** se chama `cofre` e pode ser totalmente customizado pelo usuário (acesso total ao código).

Cada nova anotação criada no Obsidian gera um documento ".md" e o conteúdo pode ser relacionado com notas existentes, o que auxilia o motor de busca interno da ferramenta a encontrar as notas de forma mais rápida. Também existe a possibilidade de criar `tags`.

Depois de começar a alimentar o cofre, uma visão gráfica igual aos exemplos abaixo tornam-se disponíveis para consultar o relacionamentos:

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/grafo2.png" align="center" alt="Grafo 1" style="max-width: 856px"></p>

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/grafo.png" align="center" alt="Grafo 2" style="max-width: 856px"></p>

É possível utilizar ferramentas como Dropbox, Onedrive e Icloud para salvar os arquivos, mas com isso você perde o principal recurso do Git, que é o versionamento.

É possível sincronizar estes arquivos com o Iphone sem nenhum custo utilizando o Github.
{: .bubble-tip}

Vamos lá?

# Tutorial

## Na estação de trabalho

Vamos considerar que o Obsidian já está instalado e com o cofre criado em sua estação de trabalho
{: .bubble-note}

Existe a opção de sincronismo utilizando o próprio Obsidian, mas para isso é preciso ter uma conta cadastrada e arcar com um custo para utilizar o recurso de sincronismo entre dispositivos. Vou mostrar como podemos utilizar um repositório privado no [Github](https://github.com/) para fazer esta função de graça no seu Iphone ou Android.

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/2025-04-19_23-18.png
" align="center" alt="obsidian cloud" style="max-width: 956px"></p>

> Mostro neste post somente como fazer no Iphone, mas o conceito é o mesmo para Android.

### Instalar plugin do Git

Primeiro precisamos instalar o plugin não nativo do git no Obsidian, Clique em `Plugins não oficiais` e depois em Procurar

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/2025-04-19_23-26.png
" align="center" alt="git plugin" style="max-width: 956px"></p>

Digite Git no ponto 1 e depois no ponto 2 clique para Instalar o plugin do Git

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/2025-04-19_23-29.png
" align="center" alt="git plugin" style="max-width: 956px"></p>

Após instalar o aplicativo será preciso criar um repositório privado no **Github** e fazer um clone em sua estação de trabalho utilizando os seguintes comandos via terminal:

```shell
git clone https://[github]/projeto
```

Vamos considerar daqui, o diretório do cofre do Obsidian já versionado com um repositório privado em uma conta do Github.
{: .bubble-note}

Após versionado, qualquer mudança no Obsidian é indicado no canto inferior direto, conforme a imagem abaixo:

![git painel](/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/2025-04-19_23-50.png)

Tente não esquecer de subir todas as mudanças importantes para o repositório utilizando os conceitos de versionamento;
{: .bubble-warning}

### Fluxo de versionamento Git

O fluxo correto para não esquecer de subir as versões corretamente está logo abaixo:

```mermaid
flowchart LR
  A[Abrir Obsidian] --> B[Git: Pull]
  B --> C[mudanças]
  C --> D[Git: Commit All changes]
  D --> E[Git: Push]
  E --> F[Fechar Solution]
```
Até este momento, o Obsidian deve estar vinculado ao projeto privado da sua conta no Github

Agora seguiremos com a parte principal deste post, que é como Acessar este repositório no github utilizando aplicativos sem custo do Iphone.

Vamos lá?

## No Iphone

### Instalação do Obsidian

Primeiro instale o aplicativo **Obsidian**, disponível em sua Apple Store e escolha o Icloud para armazenar o cofre. Ao escolher o Icloud uma pasta do Obsidian será criada no repositório do Icloud conforme a imagem abaixo:

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/whats05.jpeg" align="center" alt="git plugin" style="max-width: 356px"></p>

### Instalação do ISH (terminal)

Depois instale o aplicativo ISH, ele é um terminal linux na distribuição mais leve chamada [Alpine](https://alpinelinux.org/), também disponível na Apple Store.

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/whats02.jpeg" align="center" alt="git plugin" style="max-width: 356px"></p>

Ao abrir o app ISH, execute os comandos abaixo no terminal linux Alpine para atualizar o repositório.

```shell
apk update
```

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/whats03.jpeg" align="center" alt="git plugin" style="max-width: 356px"></p>

### Instalação do git no ISH

Agora instale o git no diretório apartado do ISH

```shell
apk add git
```
<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/whats04.jpeg" align="center" alt="git plugin" style="max-width: 356px"></p>

### Vínculo do repositório obsidian com o Icloud

Vá ao diretório raiz

```shell
cd /
```

Crie uma pasta do obsidian na raíz

```shell
mkdir obsidian
```

Agora vem o pulo do gato que é montar o repositório do Icloud na pasta do obsidian criada anteriormente no ponto de montagem "/obsidian".

```shell
mount -t ios . obsidian
```
Ao executar o último comando, o aplicativo vai abrir o app **Arquivos** em background e vai pedir para você escolher uma pasta diretamente para ser o ponto de montagem da pasta obsidian. É nesta pasta que você precisa escolher o diretório Obsidian no Icloud e clicar em `Abrir`.

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/whats05.jpeg" align="center" alt="git plugin" style="max-width: 356px"></p>

Abra o aplicativo do github e copie o endereço do projeto

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/whats06.jpeg" align="center" alt="git plugin" style="max-width: 356px"></p>

Vá na pasta do obsidian

```shell
cd \obsidian
```

Agora faça o clone do projeto

```shell
git clone https://[github]/projeto
```
Despois de digitar a senha, um clone do projeto será baixado para o diretório `\obsidian` que é praticamente um link até a pasta do Obsidian de dentro do Icloud. Esta é a mágica para conseguir sincronizar o repositório e fazer o Obsidian funcionar no celular.

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/whats07.jpeg" align="center" alt="git plugin" style="max-width: 356px"></p>

Digite o seu usuário do Github e a senha que podem ser feitas de várias formas (chave gpg, chave pública e privada). Escolha a forma de autenticar em seus projetos no Github.

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/whats08.jpeg" align="center" alt="git plugin" style="max-width: 356px"></p>

Ao terminar, fica 100%

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/whats10.jpeg" align="center" alt="git plugin" style="max-width: 356px"></p>

Com isso, o repositório já está em seu celular, especificamente na pasta do Obsidian ocupando o espaço de sua conta no [Icloud](https://www.icloud.com/)]. Abra o aplicativo Obsidian e veja está com o conteúdo do seu cofre.

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/whats12.jpeg" align="center" alt="git plugin" style="max-width: 356px"></p>

Depois, você pode configurar usuário e senha dentro do plugin do Obsidian para não precisar executar os comandos do terminal, podendo utilizar os comandos git de dentro do próprio obsidian, conforme as imagens abaixo:

Clique na Engrenagem principal do Obsidian e vá no plugin `Git`

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/whats15.jpeg" align="center" alt="git plugin" style="max-width: 356px"></p>

Preencha as informações de conta e senha conforme a imagem abaixo:

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/whats16.jpeg" align="center" alt="git plugin" style="max-width: 356px"></p>

Agora você pode utilizar a opção de dentro do Obsidian para disparar os comandos **Git**

<p style="text-align:center"><img src="/assets/img/2025-04-19-sincronizar_obsidian_com_iphone_utilizando_github/whats13.jpeg" align="center" alt="git plugin" style="max-width: 356px"></p>

Gostou da dica?

Lembre-se de respeitar o fluxo correto para um bom funcionamento do versionamento utilizando conceitos `Git`.

```mermaid
flowchart LR
  A[Abrir Obsidian] --> B[Git: Pull]
  B --> C[mudanças]
  C --> D[Git: Commit All changes]
  D --> E[Git: Push]
  E --> F[Fechar Solution]
```
Legal a dica, né? 

Comente abaixo caso tenha alguma dúvida!!

## Referência