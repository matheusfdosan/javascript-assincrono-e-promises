<h1 align="center"> Async/Await </h1>

<div align="center">

[Voltar para a Home](README.md)

[< Página anterior](promises.md)

</div>

---

<p align="center">
  <a href="#asyncawait-com-fetch">async/await com fetch</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#asyncawait-com-axios">async/await com Axios</a>&nbsp;&nbsp;&nbsp;
</p>

Async/Await é uma maneira de se escrever promessas, de forma mais "Syntactic Sugar" (= uma maneira mais bonita de se escrever). Vamos ao exemplo:

```js
const promise = new Promise(function (resolve, reject) {
  return resolve("All right!")
  // return reject("Something wrong is not right!")
})
```

Acima temos uma promessa normal, que retorna "All right!", porém, há uma maneira de simplicar o uso de `.then()`, `.catch()` e `.finally()`, com _Async/Await_.

```js
async function start() {
  const result = await promise
  console.log(result)
}

start()
```

Criamos uma função assíncrona (async), chamada `start()`, nessa função vamos pegar o resultado da _espera (await)_ da promessa, imprimí-la no console e por fim chamar a função. Esse código é como se fosse o `.then`, pois, apenas nos trará o resultado certo.

> await significa esperar, no contexto, vai esperar a promessa dar uma resposta de resolvida ou rejeitada.

Porém, como fazemos para capturar um erro, como o `.catch` e ter uma finalização, como o `.finally`? Para isso, vamos fazer um _try/catch/finally_, o bloco `try {...}` executa como se fosse o `.then`. E com essa mesma sintaxe, criaremos o bloco `catch(error) {...}` e `finally {...}`:

```js
async function start() {
  try {
    const result = await promise
    console.log(result)
  } catch (error) {
    console.log(error)
  } finally {
    console.log("Finish!")
  }
}

start()
```

> Resumindo, a função start(), possui 3 blocos, o try, catch e o finally. Se a promessa for resolvida (`resolve(...)`), o código dentro do bloco try é executado. Se não for concluída, então há um erro, e esse erro é capturado e cairá no bloco catch. E o finally executa sempre, independente se ocorrer um erro ou não.

E assim, usamos o _Async/Await_ para termos uma maneira mais bonita, de forma mais _Syntactic Sugar_ de se escrever código.

## Async/Await com fetch

E também podemos usar o Async/Await com o fetch, dessa forma:

```js
async function start() {
  // Definimos que essa função é assíncrona

  try {
    // O "try" é usado como se fosse um ".then()", só contém o código que dá certo

    const getUser = await fetch("https://api.github.com/users/matheusfdosan")
    // Fazemos a requisição dos dados com o fetch e utilizamos "await" para fazer uma espera, ou seja, só irá definir o valor da variável depois que o fetch dar uma resposta

    const user = await getUser.json()
    // Passamos o "getUser" para JSON, fazendo assim, com que tenhamos acesso aos dados da URL. Com a variável "user", já é possível pegar valores, como name, login, id, followers, e etc.

    const getRepos = await fetch(user.repos_ulr)
    // Agora, para acessarmos os repositórios do usuário, precisamos fazer mais um fetch, utilizando o "user" (que é onde estão os dados do usuário), seguido do nome da propriedade que queremos entrar, que no caso é o "repos_url"

    const repos = await getRepos.json()
    // Passamos o "getRepos" para JSON, fazendo assim, com que entre nos dados que estão dentro do link passado no fetch(user.repos_url)

    console.log(repos[0]) // Como a variável "repos" guarda um array com 30 repositórios, podemos pegar os dados do repositório, colocando apenas o index (posição) dele, nesse código estou pegando o primeiro repositório do array
  } catch (err) {
    // O "catch" é usado para pegar erros

    console.log("Gave an error! " + err)
    // Imprimindo o erro no console
  } finally {
    // O "finally" sempre executará, mesmo se o código deu certo ou não

    console.log("End of task!")
    // Imprimindo a mensagem de "Fim da tarefa" no console
  }
}

start()
// E por fim, chamamos a função assíncrona
```

Mas, isso não significa que podemos usar o `.then()` e o `.catch()`:

```js
async function start() {
  const url = "https://api.github.com/users/matheusfdosan"
  const user = await fetch(url).then((r) => r.json())
  const repos = await fetch(user.repos_url).then((r) => r.json())
  console.log(repos[0])
}

start().catch((err) => console.log("Deu um errinho aí! " + err))
// Vemos que essa função é uma promessa, portanto, podemos usar um ".catch()" para capturar algum erro que poderá haver na função, e também é possível usar um "finally()"
```

## Async/Await com Axios

Vamos ver as formas de se utilizar o Async/Await com Axios:

```js
const axios = require("axios")
// Importamos o axios, para que possamos usar seus métodos

async function getUserLogin() {
  // Criamos uma função assíncrona

  const getUser = await axios.get("https://api.github.com/users/matheusfdosan")
  // Fazemos uma requisição dos dados com o "axios.get()". E, por enquanto, "getUser" não guarda os dados de usuário que queremos, para isso teriamos que entrar na propriedade "data", "getUser.data" dessa forma

  console.log(getUser.data.login)
  // Como "getUser.data" guardar todos os dados, então, só precisamos acessá-los
}

getUserLogin().catch((err) => console.log("Houve um eqívoco! " + err))
// E por fim, chamamos a função, e utilizamos o ".catch()" para capturar qualquer tipo de erro
```

E para acessar as propriedades que contém links, fazemos dessa forma:

```js
async function getRepos() {
  const getUser = await axios.get("https://api.github.com/users/matheusfdosan")
  // Requisição do usuário
  const repos = await axios.get(getUser.data.repos_url)
  // A partir dos dados do usuário, acessamos o repos_url
  console.log(repos.data[0])
  // "repos.data[0]" pega o primeiro repositório
}

getRepos().catch((err) => console.log("Algo de errado não está certo! " + err))
// Chama a função e captura os erros
```
