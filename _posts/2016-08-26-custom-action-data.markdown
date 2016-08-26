---
layout: post
title:  "Custom Action Data"
date:   2016-08-26 00:08:52
categories: windows
tags: windows wix msi customaction
image: /images/pic02.jpg
---

## Custom Action

Uma custom action é uma ação feita geralmente em código a parte do XML do WiX, seja ele em C++, C# e o que der para gerar uma DLL. O uso de uma custom action pode ser para realizar verificações customizadas no qual o instalador não teria capacidade de realizar forma nativa.

## Primeira e segunda fase de instalação

O ideal é fazer a custom actions na primeira fase da instalação, este seria entendido desde o começo da instalação até o ponto antes de clicar em instalar. Quando a instalação começa a ser realizada (cópia de arquivos, instalação de serviços e controladores de dispositivos), já estamos na segunda fase da instalação. Neste ponto algumas propriedades já não estão mais disponíveis para uso.

## Custom Action Data

Apesar de não ser recomendado, na segunda fase da instalação é que entra a questão do Custom Action Data. Como as propriedades não estão disponíveis, como ter acesso a elas?

Um exemplo: Rodar um processo e fazer uma verificação durante a segunda fase com base nos dados coletados em uma interface (na primeira fase).

A declaração do código seria algo parecido com o trecho:

~~~
<CustomAction Id="DadosDoProcessoMaroto" Property="CriarProcessoMaroto" Value="[VALORESIMPORTANTES]" Execute="immediate" />
<CustomAction Id="CriarProcessoMaroto" BinaryKey="MINHA_DLL" DllEntry="CriarProcessoMaroto" Execute="deferred" />

<InstallExecuteSequence>
	<Custom Action="DadosDoProcessoMaroto" After="LaunchConditions">1</Custom>
	<Custom Action="CriarProcessoMaroto" After="InstallInitialize">1</Custom>
</InstallExecuteSequence>
~~~

É necessário a declaração de duas custom actions, uma que define a propriedade a ser acessada e a outra faz a chamada do processo e a verificação.

Pode se reparar que apesar delas estarem juntas, uma ação é executada na primeira fase e a outra somente na segunda.

Dentro da custom action CriarProcessoMaroto, como conseguir acessar o dado passado pela DadosDoProcessoMaroto?

## Acessando os valores na DLL

Se estiver tudo bem, a custom action CriarProcessoMaroto terá acesso a uma propriedade chamada CustomActionData, onde estará o conteúdo da propriedade VALORESIMPORTANTES, vale ressaltar que isto é só uma cópia, não uma referência.

Na DLL onde está o CriarProcessoMaroto pode se acessar a CustomActionData com a função MsiGetProperty:

~~~
UINT er = MsiGetProperty(hInstall, "CustomActionData", szValueBuf, NULL);
~~~

Finalmente será possível utilizar o valor para fazer o instalador funcionar.

A verificação através do log do instalador consegue esclarecer se o que foi feito está funcionando de forma adequada. As duas custom actions deverão ser chamadas, cada uma em sua fase, mas quando a custom action CriarProcessoMaroto for chamada, aparecerá a propriedade CustomActionData como parâmetro.

[Exemplo do MsiGetProperty](https://msdn.microsoft.com/en-us/library/windows/desktop/aa370134(v=vs.85).aspx)
