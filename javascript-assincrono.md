<h1 align="center"> Javascript Assíncrono </h1>

<div align="center">

[Voltar para a Home](README.md)

[Próxima página >](promises.md)

</div>

---

## Síncrono vs Assíncrono

Um sistema **síncrono (synchronous)**, é uma tarefa que é conclucuída após a outra, ou seja, em sequência, a tarefa anterior precisa ser concluída, para então iniciar a próxima tarefa. Inclusive, o JavaScript, por padrão, já é síncrono.

Já o sistema **assíncrono (asynchronous)**, as tarefas executam de maneira independente uma da outra, ou seja, cada tarefa se vira para executar. Então a tarefa é tratada como entidades separadas e podem continuar com sua execução enquanto outras tarefas estão em andamento.

## Conceito de CallBack

Antes de tudo, precisamos entender que, as funções do JavaScript aceitam qualquer tipo de dado como parâmetro, como números, strings, arrays, objetos, booleanos e até funções.

Callback significa "chame de volta", ou seja, é uma função que recebe outra função como parâmetro, e depois ela é chamada de volta.

Veja o exemplo:

```js
function imprimirDado(dado) {
  console.log(dado())
}

imprimirDado(function mensagem() {
  return "Olá, Mundo!"
}) // Olá, Mundo!
```

Nesse exemplo, a função `imprimirDado()` recebe a função `mensagem()` como parâmetro, contudo, `imprimirDado()` vai imprimir no console o dado do retorno da função `mensagem()`.

## API com HTTPS, callback e o assincronismo

```js
const https = require("https")

https.get("https://jsonplaceholder.typicode.com/users?_limit=2", (res) => {
  console.log(res.statusCode)
})

console.log("Conectando API...")
```

No código acima, temos uma requisição de um módulo, que é o `https`, esse módulo trás funcionalidades do HTTP, como o método GET.

Depois, com o módulo _https_, é utilizado o método GET para fazer requisições de informações, o GET é uma função que recebe dois parâmetros, primeiro passamos o local onde estão essas informações (uma URL), depois passamos uma função, essa função tem um parâmetro que armazena a resposta do servidor, essa resposta vem em forma de objeto, então podemos acessar dados do servidor a partir desse parâmetro, que no código acima, estamos pedimos o status code como resposta.

Quando executamos esse código no terminal do Node, teremos a seguinte resposta:

```bash
Conectando API...
200
```

Vemos que, primeiro foi imprimido "Conectando API..." e depois o status code, sendo que, no código, os dois estão em ordens contrárias. Isso ocorre pois a função `https.get()` é assíncrona, então, há um processo de esperar pela resposta, e quando a resposta é recebida, a função de callback é chamada. Esse sistema assíncrono faz com que outras tarefas sejam executadas enquanto essa está em andamento. Sendo assim, tendo uma demora a mais do que imprimir "Conectando API...".
