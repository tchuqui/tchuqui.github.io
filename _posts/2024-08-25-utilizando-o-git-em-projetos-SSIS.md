---
title: "Utilizando o Git em projetos SSIS"
description: "Já precisou versionar seus projetos de SSIS? Você pode utilziar o GIT para esta função e trabalhar em equipe nas customizações de projetos de fluxos de cargas utilizando o SSIS. Vou te mostrar como configurar de forma simples neste artigo"
date: 2024-08-25 07:00:00 -0400
image:
  path: "/assets/img/2024-08-25-utilizando-o-git-em-projetos-SSIS/ssis_git.png"
  alt: "Utilizando o Git em projetos SSIS"
  hide: false
categories: SSIS
tags:
  - git
  - ssis
---

Em grandes projetos utilizando o **Visual Studio**, mais de um profissional pode atuar simultâneamente em uma `solution` utilizando este conceito já difundido entre os programadores no [git](https://git-scm.com/), que é o versionamento e exploração do recurso das `branchs`. Neste artigo do blog vou mostrar como é possível integrar o seu projeto de SSIS (Sql Server Integration Service) com a engrenagem `git`, (github, gitlab, bitbucket e outros).

Vou considerar o Visual Studio já instalado e configurado com um projeto de SSIS pré configurado
{: .bubble-note}


## Teste
