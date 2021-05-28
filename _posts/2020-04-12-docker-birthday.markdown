---
layout: post
title:  "Notas do lab do 7º Docker"
date:   2020-04-12 23:15:00
categories: docker
tags: docker container dockerbday
---

### Aniversário do Docker

Este ano por conta da quarentena (COVID-19), não houve a comemoração presencial em muitos lugares, onde há palestras e bolo, afinal é um aniversário. No site comemorativo era possível fazer sete atividades em formato de lab para entender melhor a ferramenta Docker.

A seguir as notas sobre as atividades propostas nos hands-on labs.

> **Hands-on lab:** de forma grosseira quando é oferecido uma ferramenta praticamente configurada sem precisar instalar o software no nosso computador, necessitando apenas do browser com acesso a Internet.

### Formatação

Nos comandos do Docker, foi possível passar o parâmetro {% raw %} ``--format '{{json .}}' | jq`` {% endraw %}para conseguir exibir a saída da informação em formato _JSON_. O ``| jq`` serve para embelezar a saída com novas linhas e tabulações.

Para ter uma noção do que passar para o parâmetro, use o modelo [Golang de formatação (em inglês)](https://golang.org/pkg/html/template/) como referência para montar a saída.

Alguns exemplos de formatação (são as respostas de algumas das atividades propostas no lab de aniversário):

Tabela:
{% raw %}
```
'table {{.CPUPerc}}\t{{.MemPerc}}'
```
{% endraw %}

Uso de iteradores:
{% raw %}
```
docker container inspect --format '{{range .Config.Env}}{{with split . "="}}{{if eq (index . 0)"PATH"}}{{with split ( index . 1 ) ":"}}{{range .}}{{printf "%s\n" .}}{{end}}{{end}}{{end}}{{end}}{{end}}' $(docker container ls -lq)
```
{% endraw %}

Quando utilizado iteradores, a quantidade de {% raw %} ``{{end}}`` {% endraw %} não agrada muito, mas funciona.

### Criação de contexto

No Docker 18.09 é possível usar a variável de ambiente ``DOCKER_HOST`` para conectar em uma instância docker por SSH (conexão _tcp_ ou _sock_). Novas versões como a 19.03 suporta criação de contexto para poder configurar qual contexto será utilizado de forma simples.

### Volume para dentro do container

Para persistir os dados são utilizados os _volumes_. Quando referimos a Linux, uma referência para arquivo pode ser qualquer coisa, como uma conexão para o Docker.

```
/var/run/docker.sock:/var/run/docker.sock
```

Existem outros exemplos que podem compartilhar som e a interface gráfica X11.

O exemplo acima para expor a conexão do Docker para dentro do container está relacionado ao Jenkins para não precisar criar um ambiente _Docker on Docker_.

### Root

- Não utilizar portas aleatoriamente, algumas tem regras, como no Linux portas menores que 1024 são reservadas ao sistema e o usuário root.

- Para executar o container com o usuário root, após ter mudado o usuário padrão:
```docker exec --user root```

### Rede

Quando iniciamos um container é possível anexar na mesma rede de outro container somente utilizando a referência do que está sendo executado, o comando a seguir inicia um ```alpine``` na mesma rede do container ```container:broken```.
```
docker container run -it --network container:broken alpine
```

#### Copy-pasta dos hands-on labs

Segue o modo que foi criado os arquivos Dockerfile, copy-pasta na linha de comando pra não sofrer tentando fechar o vi:

```
cat >Dockerfile <<EOF
FROM foo
WORKDIR /bar
ENTRYPOINT ['./baz']
EOF
```
