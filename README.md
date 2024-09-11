# actions
Este repositorio contiene una colección de ejemplos, flujos de trabajo, plantillas y mejores prácticas para implementar y gestionar GitHub Actions. Está diseñado para automatizar tareas comunes de CI/CD, pruebas y despliegues.
# GitHub Actions
Este repositorio contiene una colección de ejemplos, flujos de trabajo, plantillas y mejores prácticas para implementar y gestionar GitHub Actions. Está diseñado para automatizar tareas comunes de CI/CD, pruebas y despliegues.

## Resources
1. Course: GitLab CI/CD: Pipelines, CI/CD and DevOps for Beginners

https://www.udemy.com/course/gitlab-ci-pipelines-ci-cd-and-devops-for-beginners/?couponCode=OF83024E

2. GitHub Actions - The Complete Guide

https://www.udemy.com/course/github-actions-the-complete-guide/

## Contenido del Repositorio:
Flujos de Trabajo (Workflows): Ejemplos de flujos de trabajo de CI/CD para diferentes lenguajes y plataformas.
Acciones Personalizadas: Guías y ejemplos para crear y usar acciones personalizadas.
Mejores Prácticas: Recomendaciones y prácticas recomendadas para optimizar el uso de GitHub Actions.
Integraciones: Configuración para integraciones con otros servicios y herramientas como AWS, Docker, Kubernetes.
Ejemplos de Automatización: Scripts y configuraciones para automatizar tareas repetitivas en los repositorios.

## Proyectos Incluidos
- [Java Project](cars-api): Proyecto Java con su propio pipeline de GitHub Actions.
- [Node.js Project](nodejs-project): Proyecto Node.js con su propio pipeline de GitHub Actions.
- [Python Project](python-project): Proyecto Python con su propio pipeline de GitHub Actions.

## Cómo Trabajar con Submódulos

1. Clona este repositorio: `git clone https://github.com/usuario/actions.git`
2. Inicializa los submódulos: `git submodule init`
3. Actualiza los submódulos: `git submodule update`

Cada submódulo es un repositorio independiente. Puedes navegar a cada proyecto y trabajar en él de forma normal.

# Resources
1. github-actions-course-resources: https://github.com/academind/github-actions-course-resources

2. GitHub Actions Documentation: https://docs.github.com/es/actions

3. Utilizar los ejecutores hospedados en GitHub: https://docs.github.com/es/actions/using-github-hosted-runners/using-github-hosted-runners

4. Marketplace: https://github.com/marketplace?type=actions

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
    needs: build  # Este job depende del job 'build'
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