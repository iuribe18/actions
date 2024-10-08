# GitHub Actions
Este repositorio contiene una colección de ejemplos, flujos de trabajo, plantillas y mejores prácticas para implementar y gestionar GitHub Actions. Está diseñado para automatizar tareas comunes de CI/CD, pruebas y despliegues.

## Resources
Git GitHub Actions, Buenas Prácticas de Integración Continua

https://www.udemy.com/course/git-github-actions-buenas-practicas-de-integracion-continua/

GitHub Actions - The Complete Guide

https://www.udemy.com/course/github-actions-the-complete-guide/

## Contenido del Repositorio:
Flujos de Trabajo (Workflows): Ejemplos de flujos de trabajo de CI/CD para diferentes lenguajes y plataformas.
Acciones Personalizadas: Guías y ejemplos para crear y usar acciones personalizadas.
Mejores Prácticas: Recomendaciones y prácticas recomendadas para optimizar el uso de GitHub Actions.
Integraciones: Configuración para integraciones con otros servicios y herramientas como AWS, Docker, Kubernetes.
Ejemplos de Automatización: Scripts y configuraciones para automatizar tareas repetitivas en los repositorios.

## Proyectos Incluidos
- [Java API Project](cars-api): Proyecto Java con su propio pipeline de GitHub Actions.
- [React Project](React): Proyecto React con su propio pipeline de GitHub Actions.
- [Node-Express](node): API Rest construida en Node y Express con su propio pipeline de GitHub Actions.

## Adicionar un Repo (Submódulos)

1. Adicionar el repositorio (node): `git submodule add git@github.com:iuribe18/node.git`
[Node-Express](Resources/submodules.png)

## Cómo Trabajar con Submódulos

1. Clona este repositorio: `git clone https://github.com/usuario/actions.git`
2. Inicializa los submódulos: `git submodule init`
3. Actualiza los submódulos: `git submodule update`

Cada submódulo es un repositorio independiente. Puedes navegar a cada proyecto y trabajar en él de forma normal.

# Resources
github-actions-course-resources: https://github.com/academind/github-actions-course-resources

GitHub Actions Documentation: https://docs.github.com/es/actions

Utilizar los ejecutores hospedados en GitHub: https://docs.github.com/es/actions/using-github-hosted-runners/using-github-hosted-runners

Acceso a información contextual sobre ejecuciones de flujo de trabajo: https://docs.github.com/es/actions/writing-workflows/choosing-what-your-workflow-does/accessing-contextual-information-about-workflow-runs

Evaluación de expresiones en flujos de trabajo y acciones: https://docs.github.com/es/actions/writing-workflows/choosing-what-your-workflow-does/evaluate-expressions-in-workflows-and-actions

Marketplace: https://github.com/marketplace?type=actions

Eventos que desencadenan flujos de trabajo: https://docs.github.com/es/actions/writing-workflows/choosing-when-your-workflow-runs/events-that-trigger-workflows

Store information in variables: https://docs.github.com/en/actions/writing-workflows/choosing-what-your-workflow-does/store-information-in-variables#default-environment-variables

## Key Elements
**Workflows**: es el archivo YAML que define un proceso automatizado que se ejecuta en un evento específico dentro de un repositorio de GitHub. Los workflows se encuentran en la carpeta 
.github/workflows/ de tu repositorio.

Archivo de Workflow: Es un archivo .yml o .yaml en el que defines uno o más jobs que deben ejecutarse en respuesta a un evento.
Eventos que Inician un Workflow: Puede ser cualquier evento de GitHub como push, pull_request, schedule (cron), workflow_dispatch (manual), entre otros.
Ejemplo de un Workflow:
name: CI Pipeline

```bash
on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Run a one-line script
        run: echo "Hello, world!"
```

**Jobs**: es un conjunto de steps que se ejecutan en el mismo entorno. Cada job se ejecuta de manera aislada en una máquina virtual (runner) y, de manera predeterminada, los jobs se ejecutan en paralelo.

Definición: Un job está definido dentro de un workflow bajo la clave jobs.
Runner: Cada job se ejecuta en un runner, que puede ser un entorno proporcionado por GitHub (como ubuntu-latest, windows-latest, etc.) o un runner auto-hospedado.
Dependencias: Puedes establecer dependencias entre jobs para que se ejecuten en secuencia utilizando la clave needs.
Ejemplo de Jobs en un Workflow:
```bash
name: Deploy Project
on: [push, workflow_dispath] # Multiple triggers
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Build the project
        run: echo "Building project..."

  test:
    runs-on: ubuntu-latest
    needs: build  # Este job se ejecuta si build se ejecuta de manera satisfactoria (Secuencial)
    # needs: [build, deploy]  # Es posible limitar a varios jobs
    steps:
      - name: Checkout code
        uses: actions/checkout@v2
      - name: Run tests
        run: echo "Running tests..."
```
En el ejemplo anterior: build y test son dos jobs.

**NOTA**: test depende de que build se complete antes de ejecutarse.

**Steps**: Un step es una tarea individual que se ejecuta como parte de un job. Un step puede ejecutar un comando o una acción de GitHub que es una aplicación reutilizable.
Definición: Los steps se definen dentro de un job bajo la clave steps.
Acciones de GitHub (uses): Puedes usar acciones de GitHub predefinidas o crear las tuyas propias.
Comandos (run): Puedes ejecutar comandos de shell directamente en un step.
Ejemplo de Steps en un Job:

```bash
steps:
  - name: Checkout code
    uses: actions/checkout@v2  # Acción de GitHub para clonar el repositorio

  - name: Set up Node.js
    uses: actions/setup-node@v2  # Acción de GitHub para configurar Node.js
    with:
      node-version: '14'

  - name: Install dependencies
    run: npm install  # Comando de shell para instalar dependencias

  - name: Run tests
    run: npm test  # Comando de shell para ejecutar pruebas
```
En el ejemplo anterior: Cada **- name:** define un step dentro del job.
Se utilizan tanto acciones de GitHub (uses) como comandos de shell (run).


## Notas
on:
  pull_request:
    types:
      - opened
  workflow_dispatch:
  push:
    branches:
      - main
      - dev
      - 'feat/**' # All branches that start with feat
    paths-ignore:
      - '.github/workflows/*' # 

* El workflow se activará para eventos push en los branches main, dev y aquellos que comiencen con feat/.
* Sin embargo, si el cambio que ocurre en esos branches solo afecta archivos dentro de .github/workflows/* (es decir, archivos de configuración de workflows de GitHub), no se disparará el workflow, ya que esas rutas están excluidas.
* Este comportamiento es útil para evitar la ejecución innecesaria del workflow cuando solo se modifican archivos de configuración del propio workflow, lo que podría no afectar directamente el código de la aplicación.

# Skipping workflow runs
You can skip workflow runs triggered by the push and pull_request events by including a command in your commit message.
Workflows that would otherwise be triggered using on: push or on: pull_request won't be triggered if you add any of the following strings to the commit message in a push, or the HEAD commit of a pull request:
[skip ci]
[ci skip]
[no ci]
[skip actions]
[actions skip]
https://docs.github.com/en/actions/managing-workflow-runs-and-deployments/managing-workflow-runs/skipping-workflow-runs

# Ejecución de variaciones de trabajos en un flujo de trabajo
Crea una matriz a fin de definir variaciones para cada trabajo.
Una estrategia de matriz permite usar variables en una definición de trabajo para crear automáticamente varias ejecuciones de trabajos basadas en las combinaciones de las variables. Por ejemplo, puedes usar una estrategia de matriz para probar el código en varias versiones de un lenguaje o en varios sistemas operativos.

```bash
jobs:
  example_matrix:
    strategy:
      matrix:
        version: [10, 12, 14]
        os: [ubuntu-latest, windows-latest]
```

Se ejecutará un trabajo para cada combinación posible de las variables. En este ejemplo, el flujo de trabajo ejecutará seis trabajos, uno por cada combinación de las variables os y version.

De forma predeterminada, GitHub maximizará el número de trabajos ejecutados en paralelo en función de la disponibilidad del ejecutor. El orden de las variables de la matriz determina el orden en el que se crean los trabajos. La primera variable que definas será el primer trabajo que se cree en tu ejecución de flujo de trabajo. Por ejemplo, la matriz anterior creará los trabajos en el orden siguiente:

{version: 10, os: ubuntu-latest}
{version: 10, os: windows-latest}
{version: 12, os: ubuntu-latest}
{version: 12, os: windows-latest}
{version: 14, os: ubuntu-latest}
{version: 14, os: windows-latest}