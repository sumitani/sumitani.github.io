---
layout: post
title:  "Voltar a programar para Windows"
date:   2020-05-30 00:06:00
categories: windows
tags: visualstudio conan nuget cpp
---

### Primeira impressão

Considerando o tempo fora de programação com alvo em Windows, algumas coisas não mudam.
Existem regras que voltam a cabeça automagicamente. Assim uma compilação que funciona em alguma máquina do infinito e não precisou reconfigurar já fazem alguns anos, precisa ser feita novamente em outro lugar.
O resultado são símbolos bagunçados, reconfiguração de caminhos de bibliotecas, padronização de saída conflitante, problemas de resolução de assinaturas por conta do tipo de compilação utilizada e afins.

As pessoas entendem o caos delas. Organização de uns, caos de outros. Indiferente a plataforma.

---

### Visual studio

Neste ponto, uma vez com o SDK não é obrigatório usar a IDE, mas já estava instalada por algum projeto feito há anos atrás.
As opções de pasta eram gerais, agora são feitas por projeto. Isto deve já fazer um bom tempo que funciona desta forma.
Quando mexe muito em uma versão específica, você espera que todas as versões funcionem da mesma forma.

O Intellisense demora a indexar, mas funciona razoavelmente bem. Considerando o esforço que é feito para gerar o mínimo de resolução de referências no outro projeto Linux, o Visual Studio é bem amigável neste aspecto.

### Gerenciadores de dependências

Esta é a parte onde pode ser complicada no Windows. Era comum fazer os seguintes passos: baixar os fontes, verificar se existia suporte para Windows e torcer para compilar direito (por fim, compilar não significa que funciona).
Utilizando um gerenciador de dependências pode ser mais simples, mas a parte de verificar o suporte para o sistema operacional ainda existe.

### Conan

Não sei se já passou pela sua cabeça que poderia ter um gerenciador de dependências por projeto, ficando desacoplado instalador do sistema operacional e nem tão complicado como um _toolchain_.
Em uma das dependências para resolver, havia um README.md com passos simples utilizando o Conan. Dizia que compilava para Windows e compilou sem erros. Inacreditável.
Nesta hora se imagina que foi feito especificadamente um trabalho árduo para aquele projeto, mas era um código python e um txt listando as dependências daquela biblioteca.

### NuGet

Tem alguns pacotes NuGet em C++. Achei curioso, já que usei NuGet somente com C#. Unica coisa que instalei foi a suite de testes para tentar aparecer no Test Explorer. Não tinha nenhuma dependencia complexa no projeto que mexi, apesar de ter visto até Boost para instalar com Nuget.

### Vcpkg

Este foi a solução usada, baixa o projeto do GitHub e lá tem as receitas para funcionar no seu SO. Diferente do Conan, você não baixa versões específicas de cada biblioteca. Sempre estamos numa espécie de `latest`.
A integração é bem limpa com o Visual Studio, uma vez instalado e feito o comando de integração tudo parece funcionar bem.

---

Mesmo com essas coisas os alguns dos projetos estavam quebrados e foi um tempo pra arrumar os modos de Debug. Coisas a parte do Windows, tem aquele `submodule hell` do Git que não me agrada muito, mas é o que tem pra hoje.
Compilar e versionar todos na `latest` assim como é feito com o vcpkg, poderia ser uma opção. Já que por algum motivo os binários novos não conversavam com os antigos em determinadas atualizações. E lá vamos nós.




