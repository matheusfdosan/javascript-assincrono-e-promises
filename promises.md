<h1 align="center"> Promises </h1>

<div align="center">

[Voltar para a Home](README.md)

[Próxima página >](async-await.md)

[< Página anterior](javascript-assincrono.md)

</div>

---

<p align="center">
  <a href="#promises-na-prática">Promises na prática</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#promises-com-fetch">Promises com fetch</a>&nbsp;&nbsp;&nbsp;|&nbsp;&nbsp;&nbsp;
  <a href="#axios">Promises com Axios</a>&nbsp;&nbsp;&nbsp;
</p>

Promise do inglês, signfica promessa, no caso do JavaScript, é a promessa de que algo vai acontecer.

Um exemplo: quando você chama um Uber pelo aplicativo, o aplicativo irá te prometer uma corrida. Pode ser que o motorista aceite, ou não aceite. No momento em que você está esperando o motorista dar uma resposta, a promessa está **pending (pendente)**. Se o motorista aceitar, a promessa foi **fulfilled (realizada)**. Mas, se não aceitou, a promessa foi **rejected (rejeitada)**.

Portanto, uma promessa poderá ter 4 estágios:

- <span style="color: #aaa;">Pending</span>: Estado inicial, o momento que se inicia a promessa

- <span style="color: #0f7;">Fulfilled</span>: A promessa foi concluída com sucesso

- <span style="color: #f55c5c;">Rejected</span>: A promessa foi rejetada, pois houve um erro

- <span style="color: #0bf;">Settled</span>: A promessa foi finalizada, seja com sucesso ou com erro

A promise é um objeto JavaScript, usado em operações assíncronas, como carregar um arquivo ou fazer a leitura de dados de uma API.

## Promises na prática

A sintaxe da promessa é iniciada dessa forma `new Promise()`, e é colocada dentro de uma varável. Dentro de `Promise()` vamos passar uma função de callback, criando assim uma nova promessa. Dessa forma:

<code>const promessa = new Promise(() => {})</code>

Mas essa promessa ainda não faz nada, se rodarmos o `console.log(promessa)`, irá imprimir no console `Promise {<pending>}`, ou seja, a promessa está pendente e é um objeto vazio. Então, precisamos resolvê-lá.

Na função callback, que está dentro do objeto `Promise()`, ao ser executada, precisamos saber se ela vai ser resolvida (resolve) ou rejeitada (reject), então, vamos adicionar dois parâmetros a ela, **resolve** e **reject** (claro que o programador pode escolher os nomes dos parâmetros).

```js
const promessa = new Promise((resolve, reject) => {})
```

Se a função retornar `resolve()`, significa que tudo deu certo nesse momento, se a função retornar `reject()`, significa que alguma coisa deu errado.

```js
const promessa = new Promise((resolve, reject) => {
  return resolve("tudo numa boa!")
  return reject("tem parada errada aí irmão!")
})
```

Portanto, precisamos capturar o resultado da promessa, utilizando o `.then()`, `.catch()` e o `.finally()`:

```js
promessa // é a variável que guarda a promessa
  .then((result) => console.log(result))
  .catch((erro) => console.log(erro))
  .finally(() => console.log("finalizada"))
```

- <span style="color: #0f0;">.then()</span> é usado quando é retornado `resolve()` na promessa, significa que tudo deu certo.
- <span style="color: #f12525;">.catch()</span> é usado para capturar a resposta de erro quando é retornado `reject()` na promessa.

- <span style="color: #3225f1;">.finally()</span> a utilidade dele é dizer que a promessa foi finalizada, tanto com `resolve()` quanto com `reject()`.

### Exemplo real:

```js
console.log("Pedi um Uber.")

const aceitarViajem = true

const uber = new Promise((aceita, rejeitada) => {
  if (aceitarViajem) {
    return aceita("> O Uber aceitou a viajem!")
  }

  return rejeitada("> O Uber rejeitou fazer a viajem!")
})

console.log("Esperando a resposta do Uber...")

uber
  .then((resultado) => console.log(resultado))
  .catch((erro) => console.log(erro))
```

Esse código demonstra o funcionamento do processo de um pedido de viajem a um motorista Uber. Primeiro, há uma mensagem "Pedi um Uber", após isso, há uma promise `uber`, que vai responder ao cliente se o motorista aceitou ou rejeitou fazer a viajem.

Logo em seguida, há uma mensagem de "Esperando a resposta do Uber...", e depois, a promessa é realizada com `then` e `catch`, que dará a resposta do motorista, essa resposta depende do valor da variável `aceitarViajem`, se for `true`, o motorista aceitou a viajem, e se for `false` o motorista rejeitou fazer a viajem.

## Promises com fetch

No JavaScript temos o `fetch()` (que significa "buscar"), que é uma função que recebe a URL que queremos buscar (`fetch('https://exemplo.com/')`).

Se colocarmos o fetch com a URL que queremos buscar, dentro de um _console.log()_, vemos que o fetch é uma **promise** em estado pendente.

> No exemplo abaixo, foi colocado a URL da API do github com meus dados em forma de JSON, como nome de usuários, localização, seguidores, bio, e etc.

```js
console.log(fetch("https://api.github.com/users/matheusfdosan"))
// Promise { <pending> }
```

Portanto, podemos usar um `.then()` para ver o que há dentro da fetch, que no caso, irá nos retornar alguns dados HTTP:

```js
fetch("https://api.github.com/users/matheusfdosan").then((response) =>
  console.log(response)
)
```

Mas se pegarmos esses dados e passarmos eles para o formato JSON, teremos uma nova promessa:

```js
fetch("https://api.github.com/users/matheusfdosan").then(
  (response) => console.log(response.json())
  // Promise { <pending> }
)
```

E se usarmos o `.then()` novamente, para ver o que há dentro do `response.json()`, vamos ter todo o conteúdo que há dentro da URL, como nome de usuário, localização, seguidores, biografia, e etc.

```js
fetch("https://api.github.com/users/matheusfdosan").then((response) =>
  response.json().then((data) => console.log(data))
)
```

O código acima, nos retornará todos os dados que contém naquela url:

```json
{
  "login": "matheusfdosan",
  "id": 104006185,
  "html_url": "https://github.com/matheusfdosan",
  "followers_url": "https://api.github.com/users/matheusfdosan/followers",
  "following_url": "https://api.github.com/users/matheusfdosan/following{/other_user}",
  "subscriptions_url": "https://api.github.com/users/matheusfdosan/subscriptions",
  "repos_url": "https://api.github.com/users/matheusfdosan/repos",
  "type": "User",
  "name": "Matheus Faustino",
  "location": "São Paulo, Brasil",
  "followers": 5,
  "following": 15
  // E há muito mais
}
```

Para mostrar no console, o nome de usuário, por exemplo, colocamos, `.login` no `data`, dessa forma: `console.log(data.login)`.

```js
fetch("https://api.github.com/users/matheusfdosan").then((response) =>
  response.json().then((data) => console.log(data.login))
) // matheusfdosan
```

Mas, vemos que no JSON há muito mais detalhes sobre o usuário, como seus repositórios, que estão em uma outra URL. Que também é possível ser acessada, apenas seguimos o padrão:

1. Usar o fetch para pregar o conteúdo no corpo da URL:

```js
fetch("https://api.github.com/users/matheusfdosan").then((response) => {
  response
})
```

2. Já que o `response` nos dá apenas dados HTTP, vamos passar ele para JSON, e usar o `.then()` mais uma vez para vermos o que há de conteúdo nesses dados HTTP:

```js
fetch("https://api.github.com/users/matheusfdosan").then((response) => {
  response.json().then((data) => {
    data
  })
})
```

3. Após isso, vamos fazer uma nova requisição com o fetch, utilizando o `data` e o nome da propriedade que desejamos entrar, que no caso é o _repos_url_ (essa propriedade guarda um link para os repositórios no Github), assim: `fetch(data.respos_url)`:

```js
fetch("https://api.github.com/users/matheusfdosan").then((response) => {
  response.json().then((data) => {
    fetch(data.repos_url)
  })
})
```

4. Depois de fazer a requisição, é só seguir o processo de usar o `.then()`, passar a resposta desse `.then` para JSON com `.json()`, e usar o `.then()` novamente para pegar o corpo da URL:

```js
fetch("https://api.github.com/users/matheusfdosan").then((response) => {
  response.json().then((data) => {
    fetch(data.repos_url).then((response) => {
      response.json().then((respositories) => console.log(respositories))
    })
  })
})
```

Que, finalmente, nos dará os dados de todos os repositórios públicos do meu usuário.

Porém, vemos que o código está um pouco confuso de se entender, mas podemos fazer algumas alterações para ficar um pouco mais legivel:

```js
const url = fetch("https://api.github.com/users/matheusfdosan") // requisição da URL

url
  .then((getUser) => getUser.json()) // pegando a resposta do servidor e passando para JSON
  .then((user) => fetch(user.repos_url)) // pegando o JSON e acessando o corpo da página, fazendo a requisição da URL que está dentro da propriedade repos_url
  .then((getRepos) => getRepos.json()) // pegando a resposta do servidor e passando para JSON
  .then((repos) => console.log(repos)) // imprimindo o corpo da página no console
  .catch((err) => console.log(err)) // capturando qualquer erro no código
```

## Axios

Axios é uma biblioteca JavaScript, que serve como uma HTTP client baseado em promessas, ou seja, ele permite enviar solicitações HTTP (como GET, POST, DELETE...) para servidores web, seja no navegador ou no Node.js, e interagir com APIs de forma assíncrona.

No browser já temos a API `fetch` por padrão (= não precisamos instalar nada). Já no Node.js, começa a ficar confuso dependendo da maneira de como usar o HTTP, pois o Node.js não possui uma API fetch como padrão.

## Instalação do Axios

Para instalar o axios, vamos digitar no terminal `npm install axios` (ou usar algum dos processos de instalação desse [link](https://axios-http.com/docs/intro)). Após isso, criaremos um arquivo `index.js`, para utilizarmos o _Axios_.

Para vermos as respostas do Axios no terminal, só precisamos rodar o arquivo `index.js` no terminal do Node.js (`node index.js`).

## Axios na prática

No arquivo `index.js`, vamos fazer um _require_, requisição do módulo Axios, dessa forma:

```js
const axios = require("axios")
```

Usamos o `axios.get(URL)`, para fazer uma solicitação HTTP com GET, é como se tivessemos fazendo um `fetch` de uma URL:

```js
const axios = require("axios")

axios.get("https://api.github.com/users/matheusfdosan")
```

E como o Axios é baseado em `promises`, então podemos usar o `.then()` para obtermos dados da API. Mas o Axios, nos permite acessar esses dados de forma mais eficiente:

```js
const axios = require("axios")

axios
  .get("https://api.github.com/users/matheusfdosan")
  .then((response) => console.log(response.data))
```

No código acima, pegamos o parâmetro do `response` (que é um objeto) e acessamos a propriedade `.data`, para pegar os dados que há dentro da URL. Uma forma bem mais fácil do que seguir as práticas do `fetch`.

A partir do `response.data` já temos todos os dados da API, sendo assim, podemos obter o nome do usuário, login, localização, seguidores entre outros. Apenas acessando as propriedades a partir do `response.data`. Dessa forma `response.data.name`, e todas as outras propriedades também podem ser acessadas dessa mesma forma:

```js
const axios = require("axios")

axios.get("https://api.github.com/users/matheusfdosan").then((response) => {
  const name = response.data.name
  const location = response.data.location
  const followers = response.data.followers

  console.log(name, location, followers)
  // Matheus Faustino São Paulo, Brasil 5
})
```

Porém, temos também propriedades que contém URLs para outros tipos de dados, como repositórios, lista de seguidores, URL do avatar entre outros, exemplo:

```json
{
  "avatar_url": "https://avatars.githubusercontent.com/u/104006185?v=4",
  "url": "https://api.github.com/users/matheusfdosan",
  "html_url": "https://github.com/matheusfdosan",
  "followers_url": "https://api.github.com/users/matheusfdosan/followers",
  "repos_url": "https://api.github.com/users/matheusfdosan/repos"
  // Há muito mais links
}
```

Para acessar essas URLs precisamos fazer uma nova requisição `axios.get(URL)`, utilizando como link o `response.data` seguido da propriedade, exemplo, `response.data.repos_url` (para entrar nos respositórios). Em seguida, é só pegar o valor com o `.then(valor => valor.data)`:

```js
const axios = require("axios")

axios
  .get("https://api.github.com/users/matheusfdosan")
  // requisição da URL
  .then((response) => axios.get(response.data.repos_url))
  // usa os dados da URL para acessar os dados dos repositórios
  .then((repos) => console.log(repos.data))
  // imprimi os dados no console
  .catch((error) => console.log(error.message))
// captura o erro
```

### Executando Promessas em Paralelo com Promise all

Podemos fazer uma promessa assíncrona utilizando o método `Promise.all([])`. Ele pode fazer várias requisições diferentes de forma assíncrona. Dentro do `.all([])` tem um array, e é dentro desse array que será colocado cada requisição com o `axios.get(URL)`. Em seguida é possível pegar cada resposta com o `.then()`:

```js
const axios = require("axios")

Promise.all([
  axios.get("https://api.github.com/users/matheusfdosan"),
  // URL do usuário
  axios.get("https://api.github.com/users/matheusfdosan/repos"),
  // URL para os repositórios do usuário
])
  .then((responses) => {
    // a propriedade `responses` é um array, que contém os dados da URL do usuário e dos repositórios
    console.log(responses[0].data.login) // matheusfdosan
    console.log(responses[1].data[3].name) // countdown-timer
  })
  .catch((error) => console.log("Deu ruim! " + error.message))
```

Nesse exemplo, cada requisição é obtida através de seu index (`responses[1]`), e apartir dela, é adquirida alguma propriedade, como `.login`.
