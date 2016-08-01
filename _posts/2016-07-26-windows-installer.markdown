---
layout: post
title:  "Estudos sobre Windows Installer e Wix"
date:   2016-07-26 22:59:07
categories: windows
tags: windows wix msi
image: /images/pic02.jpg
---
## Open Source

Quem não sabe, a Microsoft anda nessa onda de código aberto com alguns projetos como o Wix (gerador de instaladores com formato Windows Installer). 

## Wix

O projeto foi inicialmente feito pensando no Office. Hoje suporta qualquer customização.

O Windows Installer contém uma biblioteca de instalação e gerenciamento de componentes para controlar os arquivos instalados. O Wix entra na jogada para fazer a interface com esta biblioteca através de declarações XML.

## Requerimento

Ter um Windows, já que da forma apresentada ainda não é possível gerar o MSI pelo Linux. Quem sabe no futuro.

Antes de começar, para conseguir trabalhar de forma razovel com os pacotes do Windows Installer, o mínimo requerido em sistemas operacionais é o Windows XP (com Service Pack 3), em termos de quantidade de usuários ativos com este sistema é misterioso (um exemplo interessante de requerimento mínimo em sistema operacional é o Android, eles tem um gráfico representando usuários com o sistema deles). Voltando ao Windows, um dia foi o NT, outra o 2000 e agora é a vez do XP.

Não quer dizer que um instalador simples não irá funcionar no Windows 2000, o requerimento serve para delimitar o que será usado de funcionalidade no MSI. Este limite do Windows XP permite trabalhar com o sistema de patches do Windows Installer, chamados MSP. Consiste basicamente em um delta binário da versão antiga para a nova, economizando uns bons megas para serem baixados. Ainda existe conexão ruim neste mundo.

## Como fazer?

O tutorial do [site](https://www.firegiant.com/wix/tutorial/) da Firegiant é detalhada, para conseguir gerar um pequeno instalador e fazer algumas brincadeiras (instalar serviços e criar pastas no Arquivos de Programas)  bem explicativo seguindo o passo a passo, ensina inclusive a geração de patches.

## Estruturas e nomes estranhos

Com o XML, além das declarações e atributos, também é usado _GUID_ para cada componente (o componente é o arquivo que ser instalado). Também é usado _GUID_ no identificador do produto e no identificador de atualização.

Os componentes podem ser agrupados e também organizados por funcionalidade (_feature_).

Podemos ter vários arquivos wxs, wxi, wxl para gerar um instalador.

O TARGETDIR é o local onde estão os arquivos para montar o instalador, toma como ponto de partida o local do projeto que seria o SourceDir (isto é fixo).

Caso queira criar uma pasta vazia, crie um componente vazio com a declaração de _CreateFolder_.

Se for instalar os arquivos em uma estrutura fora do padrão dos programas do Windows, ser necessário o uso do GUID para cada componente de forma estática. Caso use as estruturas padres (Arquivos de Programas, AppData, etc.) pode se usar o gerador automático. O Wix tem o Heat.exe que auxilia na coleta de componentes e gera os arquivos wxs automaticamente.

## Versionamento

Além dos patches, apesar de existir a forma tradicional de atualizar os arquivos pelo _minor_, os próprios desenvolvedores sugerem o uso do _MajorVersion_, não que seja obrigatório incrementar os projetos pelo Major (1.0.0.0 para 2.0.0.0), mas sim atualizar o sistema como se o pacote antigo fosse um instalador totalmente diferente, assim o sistema detecta a versão nova, desinstala a antiga e instala a nova.

Isto implica em ter um identificador de produto diferente para cada instalador gerado, porém o identificador de atualização sempre manter o mesmo. Para gerar automaticamente o GUID do identificador do produto somente insira um asterisco no campo _Id_.

## Sem telas?

Uma vez com estas partes definidas é possível tambm utilizar um modelo para as telas de instalação, simples de serem chamadas (caso não tenha customizações em mente).

A ideia é que se o instalador funciona silenciosamente bem, com a interface gráfica funcionar da mesma forma, afinal de contas deve ser somente uma interface.

## Propriedades e peculiaridades

Se o instalador necessita de alguma informação na forma silenciosa, ela pode ser passada via linha de comando pela _Property_.

Inserir muitas _CustomAction_ (ações customizadas por DLL ou chamada de executável / script) que modifiquem muito o sistema não é bom, pelo fato do sistema perder a capacidade de reverter estas ações automaticamente, conhecido como _rollback_.

[Documentação da versão 3.x](http://wixtoolset.org/documentation/)
