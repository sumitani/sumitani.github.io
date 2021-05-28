---
layout: post
title:  "Mudança do tema deste site"
date:   2021-05-27 23:30:00
categories: docker
tags: docker jekyll
---

E os anos passam, desde 2016, quando pensei: `seria bom escrever algo não?` cheguei a começar, mas não mantive cadência de escrever. Acabei esquecendo da existência deste cantinho.

### O tema do jekyll

Consegui achar este tema simples que não tem nenhuma informação super relevante sobre quem escreve, porém serve para as anotações. Agora passarão talvez mais muitos anos até atualizar novamente.
Reparei que existe um bot que perambula pelo GitHub chamado `dependabot`, este abriu duas _Pull Requests_ relacionados a CVE do Ruby.

Tinha instalado há muitos anos atrás o Ruby neste sistema Windows, por onde escrevo, para tentar rodar o Jekyll. Acabei desinstalando por não usar.

### Docker salva o dia

Estes últimos anos a integração máquina virtual + Hyper-V estão melhores, permitindo que eu ative o Hyper-V sem sofrer com o VirtualBox.
Porque lembrar do Hyper-V? O Docker no Windows roda com Hyper-V.
Outra melhora é a questão do Docker no Windows com o WSL2 que parece mais fluido do que as versões que testei em 2016. Também está liberado as imagens para Windows sem precisar da versão server, mas voltemos ao Docker e o Jekyll.

Existe um pequeno `serve.bat` no repositório para rodar o container com o Jekyll sem precisar instalar todas as dependencias no meu sistema:

---

`docker run --volume "%cd%:/srv/jekyll" -p 4000:4000 -it jekyll/jekyll:latest jekyll serve`

---

- `%cd%`: igual o `pwd` ou `$PWD` do Linux, diretório atual.
- `--volume "%cd%:/srv/jekyll"`: mapeamento do windows para dentro do container.
- `-p 4000:4000`: a porta que o Jekyll expõe, mapeio para fora do container.
- `-it`: interativo, quero ver o que está acontecendo.
- `jekyll/jekyll:latest`: a imagem do jekyll na última versão.
- `jekyll serve`: o comando usado pra subir a página na porta 4000, poderia ser qualquer outro comando.

Mesmo sendo conexão Windows com Linux, as páginas renderizaram igual no GitHub Pages. Lembro que em outro serviço uma página NodeJS não subia nem com reza brava, eram vários problemas, mas o principal era problema de permissão no sistema de arquivos.
