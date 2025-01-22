---
title: "Como recriar 'Database Link' Oracle por aplicações customizadas .exe - Módulo de Pesagem - INFLOR"
description: "Todo DBA uma hora vai precisar ter contato com Database Link no Oracle ou Linked Server no SQL Server. Vou te mostrar neste artigo uma solução que desenvolvi para tirar esta função do DBA e passar para um usuário nomeado conseguir editar este tipo de objeto no banco de dados por meio de uma aplicação .exe"
date: 2025-01-19 17:00:00 -0400
image:
  path: "/assets/img/2025-01-18-desabilitando-database-link-oracle-por-aplicacao-exe/capa_modulo_pesagem.png"
  alt: "Editar Database Link no Oracle por aplicações Windows .exe"
  hide: true
categories: Oracle
tags:
  - oracle
  - inflor
  - dba
  - database link
  - módulo de pesagem
  - sgf
--- 

A aplicação envolvida é o **Módulo de Pesagem** da [INFLOR](https://inflor.com/), que foi planejada com a intenção de manter um banco intermediário Oracle que poderia trabalhar `offline` para não parar o fluxo dos clientes em um sistema apartado quando o MLPS oscilasse ou ficasse indisponível devido à perda do link de internet.

## Arquitetura

<p style="text-align:center"><img src="/assets/img/2025-01-18-desabilitando-database-link-oracle-por-aplicacao-exe/modulo_pesagem.png" align="center" alt="Modulo de Pesagem" style="max-width: 656px"></p>

A arquitetura consiste em dois bancos Oracle: Um **Oracle Principal** com a `aplicação SGF` e outro **Oracle Intermediário** com o `Módulo de Pesagem`. A sincronização entre os bancos acontecia com o apoio de um "Database Link" e um link MPLS, possibilitando a comunicação rápida entre as diferentes regiões.

> O Módulo de Pesagem ficava OFFLINE automaticamente com a perda de comunicação entre os bancos envolvidos.

## Tabela Verdade

{: .caption-table}

| Módulo ONLINE | Módulo OFFLINE |
| :------: | :------:|
| Database Link ON | Database Link OFF |
| aplicação ON_XE1.exe | aplicação OFF_XE1.exe |

Gatilhos
{: .caption-table}

## Qual o Problema e Oportunidade?

O "problema" era que ao oscilar o link de internet, o Módulo de Pesagem ficava OFFLINE e muitas vezes se perdia por não conseguir retomar a comunicação com o SGF, gerando muitos chamados para TI e atuações 24/7 do time. O sistema podia até funcionar por um tempo OFFLINE, mas depois era preciso sincronizá-lo com o SGF para manter as informações consilidados no sistema principal.

<!-- quotes -->

A solução foi passar para stakeholders[^1] a função de controlar se o Módulo de Pesagem funcionaria OFFLINE ou ONLINE, por uma aplicação .exe que poderia ser executada de mais de um módulo, ou seja, de mais de um servidor Windows. Neste artigo do [blog](https://tchuqui.github.io/) vou te mostrar uma solução que desenvolvi para tirar a função do DBA de editar um **database link** no Oracle.

<!-- references -->

[^1]: Conceito criado na década de 1980, pelo filósofo norte-americano Robert Edward Freeman, o stakeholder é qualquer indivíduo ou organização que, de alguma forma, é impactado pelas ações de uma determinada empresa. Em uma tradução livre para o português, o termo significa parte interessada.

Vamos lá?

## Tutorial

Primeiramente para quem não conhece o que é um Database Link ou Linked Server, a descrição vem logo a seguir em `NOTA`

Database Link no Oracle é um objeto criado em um esquema de um banco de dados que possibilita o acesso a objetos de outro banco de dados.
{: .bubble-note}

O executável foi gerado a partir de dois arquivos ".bat", convertidos em ".exe" e disponibilizados na parte superior direita do servidor Windows conforme a imagem abaixo:

![Tela Stakeholders](/assets/img/2025-01-18-desabilitando-database-link-oracle-por-aplicacao-exe/2025-01-18_16-43.png)

Somente usuários com permissões liberadas pela TI podiam executar as aplicações ".exe"
{: .bubble-warning}

Veja o código do arquivo .bat que originou os executáveis

- Scripts
   - off.bat
   - on.bat

### off.bat

```bat
echo connect [usuario_oracle]/[senha_usuario]@[SID] > doit.sql
echo drop database link [DATABASE_LINK]; >> doit.sql
echo create database link "[DATABASE_LINK]" connect to [BANCO_SGF] identified by "[senha_usuario]" using '(DESCRIPTION =(ADDRESS_LIST =(ADDRESS = (PROTOCOL = TCP)(HOST = [IP_BANCO])(PORT = 1521)))(CONNECT_DATA =(SERVER = DEDICATED)(SERVICE_NAME = [NOME_SERVICO2])))'; >> doit.sql
echo exit >> doit.sql
C:\$ORACLE_HOME\server\bin\sqlplus.exe /nolog @doit.sql
del doit.sql
msg * "MODULO OFFLINE"
exit
```
Veja abaixo o que cada linha representa detalhadamente:

1. conecta no banco com o usuário [usuario_oracle] no SID @[SID]
2. apaga o database link [DATABASE_LINK]
3. cria o database link com o nome do serviço errado propositalmente [NOME_SERVICO2]
4. toda a ação é um texto que vai para o arquivo `doit.sql`
5. o script doit.sql é executado no banco com o parâmetro /nolog para não gerar logs
6. o script doit.sql é apagado
7. a mensagem "MODULO OFFLINE" aparece na tela
8. fecha a conexão com o banco

Com a mesma logica foi possível criar outro script para ativar o Database Link.

### on.bat

```bat
echo connect [usuario_oracle]/[senha_usuario]@[SID] > doit.sql
echo drop database link [DATABASE_LINK]; >> doit.sql
echo create database link "[DATABASE_LINK]" connect to [BANCO_SGF] identified by "[senha_usuario]" using '(DESCRIPTION =(ADDRESS_LIST =(ADDRESS = (PROTOCOL = TCP)(HOST = [IP_BANCO])(PORT = 1521)))(CONNECT_DATA =(SERVER = DEDICATED)(SERVICE_NAME = [NOME_SERVICO])))'; >> doit.sql
echo exit >> doit.sql
C:\$ORACLE_HOME\server\bin\sqlplus.exe /nolog @doit.sql
del doit.sql
msg * "MODULO ONLINE"
exit
```
A diferença é que o serviço foi criado corretamente com o mesmo [NOME_SERVICO] disponível no banco do SGF, possibilitando com isso o sincronismo das informações.
{: .bubble-tip}

### Executáveis

com os arquivos .bat prontos, podemos gerar os .exe utilizando um software chamado `Bat to Exe Converter`

<p style="text-align:center"><img src="/assets/img/2025-01-18-desabilitando-database-link-oracle-por-aplicacao-exe/2025-01-18_17-35.png" align="center" alt="conversao .exe" style="max-width: 756px"></p>

Somente cole o conteúdo do script .bat neste campo marcado e clique para Converter em .exe

Em Exe-Format selecione 32 Bit.
{: .bubble-tip}

<p style="text-align:center"><img src="/assets/img/2025-01-18-desabilitando-database-link-oracle-por-aplicacao-exe/2025-01-18_17-39.png" align="center" alt="conversao .exe" style="max-width: 756px"></p>

Faça o mesmo com o outro script "on.bat" para gerar o segundo aplicativo.

Pronto!!

<p style="text-align:center"><img src="/assets/img/2025-01-18-desabilitando-database-link-oracle-por-aplicacao-exe/2025-01-18_17-41.png" align="center" alt="Mensagem OFFLINE" style="max-width: 756px"></p>

No **marcador 1** está o Executável `OFF_XE1.exe`, que foi acionado pelo stakeholder com permissão de execução. No **marcado 2** é a mensagem que aparece quando o database link foi modificado para um serviço que não encontrará o banco Primário.

<p style="text-align:center"><img src="/assets/img/2025-01-18-desabilitando-database-link-oracle-por-aplicacao-exe/2025-01-18_17-44.png" align="center" alt="Modulo de Pesagem offline" style="max-width: 756px"></p>

ao executar o aplicativo `ON_XE1.exe` a mensagem abaixo indica que o Modulo de Pesagem está ativo.


<p style="text-align:center"><img src="/assets/img/2025-01-18-desabilitando-database-link-oracle-por-aplicacao-exe/2025-01-18_17-47.png" align="center" alt="Modulo de Pesagem online" style="max-width: 256px"></p>

<p style="text-align:center"><img src="/assets/img/2025-01-18-desabilitando-database-link-oracle-por-aplicacao-exe/2025-01-18_17-52.png" align="center" alt="Modulo de Pesagem online" style="max-width: 756px"></p>

Gostou da dica?

Então é possível automatizar ações de DBA através de uma aplicação .exe via Windows. Você só precisa se certificador da sintaxe correta do script e a permissão correta para o usuário do banco conseguir realizar a operação desejada!! 

Legal, né?

O céu é o limite para a sua criatividade.

## Referência