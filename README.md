<p align="center">
  <a href="https://iacongreso.unam.mx">
    <img src="assets/header.png">
  </a>
</p>

<h1 align="center">
  Aprendizaje automático para datos genómicos:<br/>
  Predicción de bacterias resistentes a antibióticos
</h1>

Bienvenido al taller introductorio sobre **aprendizaje automático para datos genómicos**. Conoce las fuentes de datos, los formatos de archivos y los análisis comunes en datos genómicos, y cómo conectarlos con modelos de aprendizaje automático, a través de un ejemplo relacionado a la **predicción de bacterias resistentes a antibióticos**.

<p align="center">
  <a href="https://antismash-db.secondarymetabolites.org/output/GCF_000007265.1/index.html#r1c2">
    <img src="assets/cluster.png">
  </a>
</p>

## :computer: Comienzo

### :sparkles: Desde GitHub Codespaces (recomendado)

La forma **recomendada** de seguir este taller es abrir un **Codespace** de
GitHub preconfigurado con todas las dependencias que necesitarás. Para
ello, sigue estas instrucciones:

1. Crea un **fork** en tu cuenta de GitHub de este repositorio dando click
[en este enlace](https://github.com/aapashkov/biotaller2025/fork) (puedes usar
las opciones por defecto).
2. Desde **tu repositorio** da click sobre el botón verde que dice `<> Code`
localizado en la parte de arriba de la página principal, y en el menú de
`Codespaces` presiona el botón `Create codespace on main`.

  <p align="center">
    <img width="400px" src="https://docs.github.com/assets/cb-49943/mw-1440/images/help/codespaces/who-will-pay.webp">
  </p>

3. Después de haber terminado de trabajar, visita https://github.com/codespaces y
asegúrate de detener el Codespace para evitar gastos de recursos. En cualquier
momento, podrás regresar al mismo Codespace con tu progreso guardado. Cuando ya
no lo necesites, puedes eliminarlo desde esa misma página.

  <p align="center">
    <img src="https://docs.github.com/assets/cb-89126/mw-1440/images/help/codespaces/delete-codespace.webp">
  </p>

### :house: Local

Si deseas trabajar desde tu propia computadora, necesitarás un dispositivo con
MacOS o Linux (o WSL en Windows). Sigue estas instrucciones:

1. Instala [Git](https://git-scm.com/downloads) y un gestor de paquetes como
[Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html), [Mamba](https://mamba.readthedocs.io/en/latest/installation/mamba-installation.html) o
[Micromamba](https://mamba.readthedocs.io/en/latest/installation/micromamba-installation.html).
2. Crea un **fork** de este repositorio dando click en
[en este enlace](https://github.com/aapashkov/biotaller2025/fork) (puedes usar
las opciones por defecto).
3. Clona este repositorio y utiliza el archivo [`env.yml`](/env.yml)
para reproducir el ambiente que se usará en el taller:
  ```shell
  $ git clone https://github.com/aapashkov/biotaller2025.git
  $ cd biotaller2025
  $ conda env create -n bio -f env.yml # Puedes reemplazar 'conda' por 'mamba' o 'micromamba'
  $ conda activate bio
  ```
