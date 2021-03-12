# Github Action Hello World

## Accion

Primero creamos el archivo *action.yml*:
```
name: 'Hello World'
description: 'Greet someone and record the time'
inputs:
  who-to-greet:  # id of input
    description: 'Who to greet'
    required: true
    default: 'World'
outputs:
  time: # id of output
    description: 'The time we greeted you'
runs:
  using: 'node12'
  main: 'index.js'
  ```
  - **who-to-greet**: Variable que almacena el saludo, por defecto *World*
  - **time**: Se le asignará el timestamp para que salga la hora a la que fue lanzado
  
Posteriormente se creo el archivo "index.js*:
```
const core = require('@actions/core');
const github = require('@actions/github');

try {
  // `who-to-greet` input defined in action metadata file
  const nameToGreet = core.getInput('who-to-greet');
  console.log(`Hello ${nameToGreet}!`);
  const time = (new Date()).toTimeString();
  core.setOutput("time", time);
  // Get the JSON webhook payload for the event that triggered the workflow
  const payload = JSON.stringify(github.context.payload, undefined, 2)
  console.log(`The event payload: ${payload}`);
} catch (error) {
  core.setFailed(error.message);
}
```
Este fichero nos sirve para lanzar los mensajes y enlazar con la acción.

Para ello que funcionase se tuvo que instalar los paquetes de acciones:
```
npm install @actions/core
npm install @actions/github
```
y añadimos una version a la accion
```
git tag -a -m "v1" v1
git push --follow-tags
```

## Uso de la accion

Creamos los directorios *.github/workflow* y dentro de este ultimo el fichero *main.yml* con el siguiente contenido:
```
name: Using hello world
on: [push]

jobs:
  hello_world_job:
    runs-on: ubuntu-latest
    name: A job to say hello
    steps:
      # To use this repository's private action, you must check out the repository
      - name: Checkout
        uses: actions/checkout@v2
      - name: Hello world action step
        uses: ULL-ESIT-PL-2021/hello-js-action-alu0100615791@v1
        id: hello
        with:
          who-to-greet: 'Procesadores de Lenguajes at ULL'
      # Use the output from the `hello` step
      - name: Get the output time
        run: echo "The time was ${{ steps.hello.outputs.time }}"
```
Este fichero crea un job que se ejecuta en la ultima version de ubuntu y usa el repositorio que ejecutamos anteriormente obteniendo como salida el valor en *who-to-greet* y el tiempo al que fue lanzado

En Actions podemos ver los workflows que tenemos en nuestro repositorio y podemos ejecutar la acción.

## Submodules y Github Pages

En este repositorio se crearon submodulos de los repositorios anteriores, para actualizar la información simplemente hacemos *git pull* en el repositorio del submodulo.

En la página settings creamos la página de github seleccionando el tema Cayman, generandose automáticamente

## Jekyll

Para poder ver en local la pagina con Jekyll instalamos las dependencias de Ruby necesarias; *bundle y jekyll* y en la terminal de comandos de Windows ejecutamos el comando
```
jekyll serve
```
