---
marp: true
style: |
  section.lead {
    text-align: center
  }
  .two-unequal-cols {
    display: grid;
    grid-template-columns: 2fr 1fr;
    grid-template-rows: 1fr;
    grid-column-gap: 20px;
    grid-row-gap: 0px;
  }
  .two-equal-cols {
    display: grid;
    grid-template-columns: 1fr 1fr;
    grid-template-rows: 1fr;
    grid-column-gap: 20px;
    grid-row-gap: 0px;
  }
  .two-unequal-cols-reverse {
    display: grid;
    grid-template-columns: 1fr 2fr;
    grid-template-rows: 1fr;
    grid-column-gap: 20px;
    grid-row-gap: 0px;
  }
---

<!-- _class: lead -->
<!-- _header: '![width:1225px](../assets/header.png)' -->
<!-- _footer: '![width:1225px](../assets/cluster.png)' -->

## Aprendizaje automático para datos genómicos:<br/>**predicción de bacterias resistentes a antibióticos**

[GitHub](https://github.com/aapashkov/biotaller2025/) | [Etherpad](https://board.net/p/biotaller2025)

---

![bg right width:600px](https://docs.github.com/assets/cb-49943/mw-1440/images/help/codespaces/who-will-pay.webp)

# **Preámbulo**:<br/>Prueba de cordura

0. Asegúrate de tener una cuenta de **GitHub**
1. Visita [aapashkov/biotaller2025](https://github.com/aapashkov/biotaller2025)
2. Crea un [fork](https://github.com/aapashkov/biotaller2025/fork]) en tu cuenta
3. Desde <u>tu repositorio</u>, abre un **GitHub Codespace**
4. Conoce el [Etherpad](https://board.net/p/biotaller2025)

---

# **Contenidos**: ¿qué vamos a aprender?

**10 de abr.**

1. Introducción: biología molecular y resistencia antimicrobiana **`Teoría`**
2. Explorando la base de datos de NCBI **`Práctica`**
3. Reconstrucción de genomas: de lecturas a anotaciones **`Práctica`**

**11 de abr.**

4. Modelo basado de funciones **`Práctica`**
5. Integrando ortología al modelo **`Práctica`**
6. Agregando SNPs al modelo (si da tiempo) **`Práctica`**
7. Más recursos **`Teoría`**

---

<!-- _class: lead -->

# Introducción: **biología molecular** y **resistencia antimicriobiana**

<!-- _footer: '![width:1225px](../assets/cluster.png)' -->

---

<!-- header: Introducción: biología molecular y resistencia antimicrobiana -->
<!-- _footer: Adaptado de OpenStax Microbiology CC-BY-SA -->

# Los organismos vivos almacenamos **información** en forma de tres **moléculas**

<div class="two-unequal-cols">
  <img src="../assets/molecules.png">
  <div>

A la **totalidad** de cada molécula se le asigna un nombre que termina en **oma**:

DNA → Genoma <small><small>(compuesto por <u>cromosomas</u>)</small></small>
    
RNA → Transcriptoma

Proteína → Proteoma

  </div>
</div>

---

# La **localización** del DNA depende del **tipo celular**

<div style="text-align: center">

![width:850px](../assets/cell-types.jpg)

</div>

---

# El **dogma central** describe el paso del DNA al RNA y a la proteína

<!-- _footer: 'Adaptado de Khan Academy CC-BY-SA' -->

![bg left:55% width:700px](../assets/central-dogma.png)

El **gen** es:

- una región del DNA que se **transcribe** en RNA
- una unidad funcional **discreta**

---

<!-- _footer: 'Galaxy Project CC-BY-SA' -->

# **No** es posible obtener el genoma completo de manera directa

<div class="two-equal-cols">
  <img src="../assets/assembly.png">
  <div>

[**Secuenciación**](https://en.wikipedia.org/wiki/Sequencing): extracción de fragmentos del genoma llamados **lecturas**, cuya longitud depende de la tecnología:
- **Illumina**: 150pb a 300pb
- **Nanopore**, **PacBio**: hasta >4Mpb

[**Ensamblado**](https://en.wikipedia.org/wiki/Sequence_assembly): alineamiento de lecturas en **contigs**, formando el genoma

**pb**: par de bases

  </div>
</div>

---

# **FASTA** y **FASTQ** son los formatos de archivos más comunes para **secuencias**

<div class="two-equal-cols">
  <div>

[**FASTA**](https://en.wikipedia.org/wiki/FASTA_format) (eg. **FNA** y **FAA**) → genomas, transcriptomas, proteomas, genes, etc.

```
>prot001 beta-tubulin
MRLLQAFNVQRNVNEDGSCGEKINFSKTCIRSDFGDCC
KCGSGKEFSGDNCQDGNYSGSAVTTS
>prot002 transcription factor II B
FICKGSKFDDCCLAASYCRDDKYYCGKYLGCQPEFGKY
STNRDYACNSGSITAKPSATKPPLSISTNRDYACNSGF
SKFDDCCLAASYCRDDKYYCGKYLGCQPEFGKYT
>prot003 laminin
MAIYAVSARFASPDVLGGPASSKPFAQFAMNLNQGSDL
LKASLLLYVYEMSETVRWDTVAEIARITRMAELYYALH
PGEPRKASTFDDGMSLRSQEREISSGLAAEEWNSVWWC
DTSCSAVASFSNPITDNLQAKMALPATSVSEF
```

  </div>
  <div>

[**FASTQ**](https://en.wikipedia.org/wiki/FASTQ_format) → lecturas de secuenciación (FASTA + calidad)

```
@read001
CATTAGCATGGATTGCTCACCAGGTGGAGCGGCCACGA
+
1)(&""*/.$$$,0,((*''%$"*5/-,06789G666<
@read002
AGTGAAGTCGCCGAGGCTTGTGTACTCGTCTGCCGCAC
+
7A?9?DC8.+,976:F<D>.-.0$240,,,1$$()*(+
@read003
TGTTATGTGTAACATTTTCTTCGTTCAGTTCTTCTGTT
+
&(177:31')%&&%%%++((,$""&)*,,.'%)'%%%*
```

  </div>
</div>

---

<!-- _footer: 'Wikimedia Commons CC-BY-SA' -->

![bg right width:600px](../assets/chloroplot.png)

# La **anotación** describe la estructura y función de un **genoma**

Identifica **genes**, **secuencias codificantes** (CDS) de proteínas, **regiones repetitivas**, etc.

---

# **GFF** y **GenBank** almacenan **anotaciones**

<small>

<div class="two-equal-cols">
  <div>

[**GFF**](https://en.wikipedia.org/wiki/General_feature_format) (General Feature Format)

![](../assets/gff3.png)

  </div>
  <div>

[**GenBank**](https://en.wikipedia.org/wiki/GenBank) (o **EMBL**)

![](../assets/gbk.png)

  </div>
</div>

</small>

---

<!-- _class: lead -->

# La **IA** ayuda a relacionar **genotipo** con **fenotipo**

![width:900px](../assets/aibio.jpg)

---

<!-- _footer: '(McArthur _et al_, 2013; Reygaert, 2018)' -->

<small>

# Ha habido una carrera armamentística entre los **antibióticos** y las **bacterias** en los últimos 100 años

<div class="two-equal-cols">
  <div>

**Mecanismos de resistencia**
Alteración de diana antibiótica
Reemplazo de diana antibiótica
Protección de diana antibiótica
Inactivación de antibiótico
Reducción de permeabilidad al antibiótico
Resistencia por ausencia
Modificación de morfología celular
Por adquisición de nutrientes del huésped
Sobreexpresión de diana antibiótica

  </div>
  <div>

**Mecanismos de los antibióticos**
<u>Inhibición de la síntesis de la pared celular</u>: beta-lactamos y glicopéptidos
<u>Despolarización de la pared celular</u>: lipopéptidos
<u>Inhibición de síntesis de proteínas</u>: por unión a ribosoma
<u>Inhibición de la síntesis de DNA/RNA</u>: quinolonas
<u>Inhibición de rutas metabólicas</u>: sulfonamidas, trimetoprima

  </div>
</div>

</small>

---

<!-- _footer: LibreTexts CC-BY-SA -->

# La resistencia antimicrobiana es producto de la **fijación de mutaciones**

![bg right width:600px](../assets/mutations.jpg)

Dos tipos de mutaciones:

- **Puntuales**: cambios en una base
- **Cromosómicas**: cambios en segmentos grandes

---

<!-- _footer: '(Hindler _et al._, 2015; Weinstein _et al._, 2020)' -->

# La **susceptibilidad** a los antibióticos se prueba de una forma **estandarizada**

<div class="two-equal-cols">
<div>

**MIC**: la menor concentración de una sustancia que previene el crecimiento de un organismo

Se evalúa en potencias de **2 mg/L** y se interpreta con un **estándar**

![](../assets/mic.png)

</div>

<div>

Por ejemplo, por el estándar [**CLSI**](https://em100.edaptivedocs.net/dashboard.aspx):

<table style="text-align: center">
  <tr>
    <th rowspan="2">Especie </th>
    <th colspan="2">Ciprofloxacin</th>
    <th colspan="2">Meropenem</th>
  </tr>
  <tr>
    <th>Sus.</th>
    <th>Res.</th>
    <th>Sus.</th>
    <th>Res.</th>
  </tr>
  <tr>
    <td><i>A. baumannii</i></td>
    <td>≤1</td>
    <td>≥1</td>
    <td>≤2</td>
    <td>≥8</td>
  </tr>
    <td><i>C. jejuni</i></td>
    <td>≤1</td>
    <td>≥1</td>
    <td>-</td>
    <td>-</td>
  <tr>
    <td><i>E. coli</i></td>
    <td>≤0.25</td>
    <td>≥1</td>
    <td>≤1</td>
    <td>≥4</td>
  </tr>
  <tr>
    <td><i>N. gonorrhoeae</i></td>
    <td>≤0.06</td>
    <td>≥1</td>
    <td>-</td>
    <td>-</td>
  </tr>
  <tr>
    <td><i>S. enterica</i></td>
    <td>≤0.06</td>
    <td>≥1</td>
    <td>≤1</td>
    <td>≥4</td>
  </tr>
</table>

</div>

</div>

---

<!-- _footer: (Hu *et al.*, 2024) -->

# Los modelos de **IA** son una **alternativa** prometedora para pruebas de **susceptibilidad**

|Modelo|Atributo|Patógenos / antibióticos|Etiqueta|Rend.|
|:--|:--|:--|:--|:--|
|XGBoost|Clústeres ARG|5 / 8|Fenotipo|AUC ~0.9|
|Árboles|k-meros|12 / 56|Fenotipo|Acc >0.9|
|XGBoost|k-meros|*Salm.* / 15|MIC|Acc >0.9|
|Forest|Variantes|3 / 30|Fenotipo|AUC ~0.8|
|Reg. lineal|Det. resistencia|*N. gon.* / 5|MIC ±1|Acc >0.99|
|AdaBoost|k-meros|4 / 9|Fenotipo|Acc ~0.88|

---

# Usaremos los datos de ***Mycoplasmoides genitalium*** de [Fookes, *et al.* (2017)](https://doi.org/10.1186/s12864-017-4399-6)

![](../assets/paper.png)

---

<!-- _class: lead -->
<!-- header: '' -->

# Explorando la base de datos de **NCBI**

<!-- _footer: '![width:1225px](../assets/cluster.png)' -->

---

<!-- header: 'Explorando la base de datos de NCBI' -->

# El **INSDC** es un acuerdo entre tres bases de datos para funcionar como **espejos**

|EUA: [NCBI](https://ncbi.nlm.nih.gov)|Europa: [EBI ENA](https://www.ebi.ac.uk/ena)|Japón: [DDBJ](https://www.ddbj.nig.ac.jp/)|
|:-:|:-:|:-:|
|![](../assets/ncbi.png)|![](../assets/ena.png)|![](../assets/ddbj.jpg)|

Las mismas secuencias, pero no los mismos metadatos

---

# El NCBI organiza los datos genómicos de manera **jerárquica**

![bg right:60% width:700px](../assets/ncbi-structure.png)

- [**NCBI Datasets**](https://www.ncbi.nlm.nih.gov/datasets)
- [**NCBI SRA**](https://www.ncbi.nlm.nih.gov/sra)

---

# Comandos para **descargar** genomas y lecturas

Para genomas, usar [NCBI Datasets CLI](https://www.ncbi.nlm.nih.gov/datasets/docs/v2/command-line-tools/download-and-install/):

```bash
$ datasets download genome accession GENOME... # Por accession
$ datasets download genome taxon TAXON...      # Por taxón
```

Para lecturas, usar [SRA Tools](https://github.com/ncbi/sra-tools):

```bash
$ prefetch SRA...       # Paso 1: precargar lecturas
$ fasterq-dump FILE...  # Paso 2: extraer lecturas
```

---

<!-- _class: lead -->
<!-- header: '' -->

# Reconstrucción de **genomas**: de lecturas a anotaciones

<!-- _footer: '![width:1225px](../assets/cluster.png)' -->

---

<!-- header: 'Reconstrucción de genomas: de lecturas a anotaciones' -->

---

<!-- header: '' -->

# ¡Tarea!

<div class="two-equal-cols">

<div>

0. Lee sobre cómo leer línea por línea de un archivo con [while read](https://bashcommands.com/bash-while-read).
1. Descarga todas las lecturas de `metadata.tsv` en `data/reads`.
2. Ensambla las lecturas en contigs y guárdalas en `data/genomes`.
3. Anota los genomas ensamblados y almacénalos en `data/annotations`.

Si no lo logras, no te preocupes. La respuesta y los resultados se proporcionarán mañana.

</div>

<div>

```bash
$ tree -L 2 data/
data/
├── annotations/
│   ├── M2282/M2282.*
│   ├── M2300/M2282.*
│   └── .../
├── genomes/
│   ├── M2282/contigs.fasta
│   ├── M2300/contigs.fasta
│   └── .../
└── reads/
    ├── ERR486841/ERR486841.sra
    ├── ERR486833/ERR486833.sra
    ├── .../
    ├── ERR486841_1.fastq
    ├── ERR486841_2.fastq
    └── ...
```

</div>

</div>

---

<!-- _class: lead -->
<!-- _footer: '![width:1225px](../assets/cluster.png)' -->

# Fin de día 1:<br/>¿Qué aprendiste y qué podría mejorar?

Contesta en el [Etherpad](https://board.net/p/biotaller2025)
