# Semana 14: Análisis shotgun utilizando datos de secuenciación de Nanopore

## Logro de la sesión: 

Al finalizar la sesión, el estudiante conoce y utiliza herramientas bioinformáticas para realizar un análisis shotgun a partir de los datos de secuenciación Nanopore.

# Estructura de la práctica:

1. Acceso al servidor de cómputo
2. Análisis de calidad de la secuenciación Nanopore 
3. Limpieza de los archivos FASTQ
4. Eliminación de la contaminación en los archivos FASTQ
5. Identificación del perfil taxonómico a partir de los archivos FASTQ 
6. Ensamblaje de metagenomas 
7. Anotación de metagenomas 

## Programas requeridos:

### Programas de acceso al servidor:

PuTTY v0.79 https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html
   - **Descripción:** PuTTY es un cliente SSH, Telnet y Rlogin gratuito y de código abierto para Windows y sistemas Unix. Se utiliza principalmente para establecer conexiones seguras de línea de comandos a servidores remotos.

WinSCP v6.1 https://winscp.net/eng/download.php
   - **Descripción:** WinSCP es un cliente SFTP, FTP, WebDAV, Amazon S3 y SCP gratuito y de código abierto para Windows. Permite la transferencia segura de archivos entre un ordenador local y servidores remotos mediante una interfaz gráfica de usuario. 

### Programas bioinformáticos:

Abricate v1.0.1 https://github.com/tseemann/abricate
   - **Descripción:** Abricate es una herramienta bioinformática de línea de comandos para la detección rápida de genes de resistencia a antibióticos (ARG) en genomas bacterianos completos o ensamblajes de contigs. Compara secuencias genómicas con bases de datos de genes de resistencia conocidos (como CARD, ResFinder y NCBI AMR). Abricate solo soporta contigs, no lecturas FASTQ, y detecta genes de resistencia adquiridos, no mutaciones puntuales. Requiere BLAST+ >= 2.7 y any2fasta.

CheckM v1.2.2 https://ecogenomics.github.io/CheckM/
   - **Descripción:** CheckM es una herramienta que evalúa la calidad de los metagenomas ensamblados y de los genomas ensamblados a partir de metagenomas (MAGs). Proporciona estimaciones de completitud y contaminación basadas en marcadores genéticos.

Flye v2.9.2 https://github.com/fenderglass/Flye/
   - **Descripción:** Flye es un ensamblador de novo enfocado en lecturas largas. Se destaca por su velocidad y eficiencia en el uso de memoria, lo que lo hace adecuado para ensamblar genomas grandes en recursos computacionales limitados.
     
Nanofilt v2.8.0 https://github.com/wdecoster/nanofilt
   - **Descripción:** NanoFilt es una herramienta para filtrar datos de secuenciación de Nanopore basándose en la calidad y la longitud de los reads. Permite seleccionar reads de alta calidad para análisis posteriores.

NanoPlot v1.41.6 https://github.com/wdecoster/NanoPlot
   - **Descripción:** NanoPlot es una herramienta para la visualización de datos de secuenciación de Nanopore. Genera varios tipos de gráficos para evaluar la calidad y las características de los reads, como la distribución de longitudes y la calidad a lo largo de los reads.

Minimap2 v2.29.0 https://github.com/lh3/minimap2
   - **Descripción:** es un alineador de secuencias rápido y versátil diseñado para alinear secuencias largas de ADN o ARN, como lecturas de Nanopore y PacBio contra genomas de referencia o ensamblajes, así como lecturas largas entre sí para el ensamblaje de novo y lecturas cortas contra genomas; se destaca por su velocidad, sensibilidad robusta frente a errores en lecturas largas, su formato de salida PAF y una amplia gama de opciones de personalización mediante indexación eficiente basada en minimizadores, siendo una herramienta fundamental para diversas tareas de análisis genómico.

MobileElementFinder v1.1.2 https://pypi.org/project/MobileElementFinder/
   - **Descripción:** MobileElementFinder es un servidor web y una herramienta de Python para detectar elementos genéticos móviles (MGEs) en datos de secuencias ensambladas. Anota su relación con la resistencia a antibióticos, genes de virulencia y plásmidos. Puede detectar MITEs, ISs, ComTns, Tns, ICEs, IMEs y CIMEs. Requiere secuencias contiguas ensambladas como entrada.

MOB-suite v3.1.9 https://github.com/phac-nml/mob-suite
   - **Descripción:** Es una colección de herramientas bioinformáticas para el análisis de elementos genéticos móviles (MGEs) en ensamblajes de genomas bacterianos. Incluye herramientas como MOB-typer y MOB-recon. MOB-typer, dentro de MOB-suite, analiza ensamblajes genómicos para identificar y clasificar plásmidos basándose en la presencia de genes clave como replicones y sistemas de movilización, prediciendo así su tipo, capacidad de transferencia y posible rango de huéspedes. Por otro lado, MOB-recon utiliza esta información, junto con bases de datos de referencia, para intentar reconstruir las secuencias completas de los plásmidos a partir de ensamblajes fragmentados, agrupando contigs que se cree que pertenecen al mismo plásmido y proporcionando informes detallados sobre sus características.

Prinseq v0.20.4 https://prinseq.sourceforge.net/
   - **Descripción:** Es un programa bioinformático escrito en Perl que se utiliza para el control de calidad, el filtrado y el formateo de datos de secuencias de alto rendimiento (high-throughput sequencing), como las lecturas obtenidas de secuenciadores Illumina, PacBio o Nanopore. La versión lite es una versión "ligera" de PRINSEQ (PRocessing and INformation of SEQuence data). En esencia, PRINSEQ-lite te permite limpiar y preparar tus datos de secuenciación antes de realizar análisis posteriores, como el ensamblaje de genomas, el mapeo de lecturas o el análisis de expresión génica.
     
Prokka v1.14.6 https://github.com/tseemann/prokka
   - **Descripción:** Prokka es una herramienta para la anotación rápida de genomas bacterianos, arqueales y virales. Predice genes codificantes de proteínas, ARNr, ARNt, regiones CRISPR y otras características genómicas. Prokka genera archivos de salida listos para la presentación a GenBank/ENA/DDBJ. Puede usar bases de datos BLAST específicas del género y mejorar las predicciones de genes para genomas altamente fragmentados.
     
Quast v5.2.0 http://quast.sourceforge.net/
   - **Descripción:** QUAST (Quality Assessment Tool for Genome Assemblies) es una herramienta que calcula una amplia variedad de métricas para evaluar la calidad de los ensamblajes de genomas. Proporciona estadísticas detalladas sobre la contigüidad, la precisión y la completitud del ensamblaje.

RGI v6.0.3 https://card.mcmaster.ca
   - **Descripción:** RGI (Resistance Gene Identifier) predice genes de resistencia a antibióticos (ARG) y sus mecanismos en datos de proteínas o nucleótidos, basándose en la base de datos CARD. Puede analizar datos genómicos y metagenómicos. Utiliza Prodigal para la predicción de ORF y DIAMOND para la detección de homólogos.

### Herramientas bioinformáticas en línea:

AntiSMASH https://antismash.secondarymetabolites.org/#!/start
   - **Descripción:** AntiSMASH (antibiotics & Secondary Metabolite Analysis SHell) es una herramienta web para la identificación, anotación y análisis de clústeres de genes de biosíntesis de metabolitos secundarios en genomas bacterianos y fúngicos. Integra varias herramientas in silico de análisis de metabolitos secundarios, como NCBI BLAST+, HMMer 3 y Muscle 3.

KAAS https://www.genome.jp/kegg/kaas/
   - **Descripción:** KAAS (KEGG Automatic Annotation Server) es un servicio web que asigna información funcional a los genes de un genoma consultado, comparándolos con la base de datos KEGG. Permite obtener anotaciones de vías metabólicas, enzimas y otras funciones biológicas.
     
## Metodología:

## 1.	Acceso al servidor de cómputo

### Abrir el programa PuTTY, colocar el hostname ( 10.142.250.66 ) y port ( 22 ), y dar clic en Open:

<img width="500" alt="image" src="https://github.com/user-attachments/assets/92f89dbb-1a21-411d-adb5-38fe486a5567" />



### En la terminal abierta, escribir su usuario y contraseña correspondiente para tener acceso al servidor de cómputo Tensor:
 
<img width="700" alt="image" src="https://github.com/user-attachments/assets/4d246e93-c59c-4749-a2dd-03db25c53654" />


### Abrir el programa WinSCP, colocar el hostname ( 10.142.250.66 ) y port ( 22 ), escribir su usuario y contraseña correspondiente para tener acceso al servidor de cómputo Tensor, y hacer clic en Login:

<img width="500" alt="image" src="https://github.com/user-attachments/assets/ef4dc253-ce4a-417d-b761-39692d2a011a" />

<img width="500" alt="image" src="https://github.com/user-attachments/assets/577debca-6085-47c5-9bbd-73688bfa8bb0" />

## 2. Análisis de calidad de la secuenciación Nanopore

```bash
cd

mkdir shotgun

cd shotgun

mkdir quality

cd quality

conda activate metataxonomic

NanoPlot -t 15 --fastq /data/2025_1/sequencing/shotgun/fastq/b01.fastq -p b01_raw --only-report --maxlength 1000000 -o .
```

## 3. Limpieza de los archivos FASTQ 

```bash
cd ~/shotgun

mkdir trim

cd trim

porechop -t 15 -i /data/2025_1/sequencing/shotgun/fastq/b01.fastq -o b01_porechop.fastq.gz

gunzip -c b01_porechop.fastq.gz | NanoFilt -q 10 --length 200 | gzip > b01_nanofilt.fastq.gz

conda activate shotgun

seqkit stats -a -j 15 *.fastq > b01_trim_stats.txt
```

## 4. Eliminación de la contaminación en los archivos FASTQ 

```bash
cd ~/shotgun

mkdir contamination

cd contamination

minimap2 -ax map-ont /data/2025_1/database/reference/manihot.mmi ~/shotgun/trim/b01_nanofilt.fastq.gz | samtools fastq -n -f 4 - > b01_clean_mes.fastq

minimap2 -ax map-ont /data/BL16/nanopore/shotgun_24_1/homo/homo_index.mmi b01_clean_mes.fastq | samtools fastq -n -f 4 - > b01_clean_hsa.fastq

seqkit stats -a -j 15 *.fastq > b01_contamination_stats.txt
```

## 5. Identificación del perfil taxonómico a partir de los archivos FASTQ

```bash
cd ~/shotgun/

mkdir taxonomy

cd taxonomy

kraken2 -db /data/db/kraken2/k2_pluspf --threads 15 --use-names cd ~/shotgun/contamination/b01_clean_hsa.fastq --output b01.kraken –report b01.report

kreport2krona.py -r b01.report -o b01.krona

ktImportText b01.krona -o b01_krona_report.html
```

```bash
# Exportar el archivo HTML y visualizarlo
```

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/ab5980cc-7f58-4033-9bab-e3c019ba930c" />


















## 5. Obtención del archivo BIOM

```bash
awk 'BEGIN{ FS = OFS = "\t" } { print "", $0 }' emu-combined-tax_id.tsv > masato_its_emu.tsv

awk '{if (NR == 1) {print $0} else {print NR-2, $0}}' masato_its_emu.tsv > tmp && mv tmp masato_its_emu.tsv

tr '\t' ',' < masato_its_emu.tsv | sed 's/_nanofilt.fastq//g' > masato_its_emu.csv

awk 'BEGIN{FS=OFS=","}{sub(/\r$/,"");print $1,$9,$8,$7,$6,$5,$4,$3,$2,$10,$11,$12,$13,$14,$15,$16,$17,$18,$19}' masato_its_emu.csv > tmp && mv tmp masato_its_emu.csv

sed 's/,$//' masato_its_emu.csv | sed 's/superkingdom/Kingdom/g' | sed 's/phylum/Phylum/g' | sed 's/class/Class/g' | sed 's/order/Order/g' | sed 's/family/Family/g' | sed 's/genus/Genus/g' | sed 's/species/Species/g' | sed 's/_its.fastq//g' > masato_its_emu_final.csv

## Crear en Windows la carpeta metataxonomic, copiar el archivo CSV final y descomprimir el archivo biom.rar que está en el aula virtual.

## Abrir RStudio y seguir las indicaciones del docente.

## Cargar el archivo generado en el programa Easy16S (https://shiny.migale.inrae.fr/app/easy16S) y exportarlo en BIOM (junto con su metadata en CSV).
```

## 6. Análisis de diversidad  

Abrir el archivo BIOM en el programa microbiomeanalyst(https://www.microbiomeanalyst.ca/MicrobiomeAnalyst/ModuleView.xhtml) y seguir las indicaciones del docente

## 7. Analisis metataxonómico de las secuencias 16S

```bash
La localización de las secuencias 16S esta en /data/2025_1/sequencing/metataxonomica/16s_curso/

Distribución de los barcodes: Grupo 1: b01, b02, b03, b04, b18 | Grupo 2: b05, b06, b07, b08, b17 | Grupo 3: b09, b10, b11, b12, b19 | Grupo 4: b13, b14, b15, b16, b20

Realizar la visualización de calidad de las secuencias de sus respectivos barcodes

Realizar la limpieza de los FASTQ utilizando los siguientes criterios: un Q minimo de 10, una longitud minima de secuencia de 1000 y una longitud máxima de 2000

Calcular el número total de lecturas limpias

Realizar el análisis metataxonómico de sus respectivos barcodes utilizando la base de datos localizada en /data/db/emu/emu/

Copiar los archivos *_16s.fastq_rel-abundance.tsv a la carpeta /data/2025_1/16s_alumnos

Copiar los archivos *_16s.fastq_rel-abundance.tsv que estan en la carpeta /data/2025_1/16s_alumnos segun el siguiente esquema: Grupo 1 copiara los archivos del Grupo 2, Grupo 2 copiara los archivos del Grupo 4, el Grupo 3 copiara los archivos del Grupo 1, Grupo 4 copiara los archivos del Grupo 3

Generar el archivo emu-combined-tax_id.tsv y su respectivo archivo BIOM (reemplazar masato_its por masato_16s)

Realizar el analisis de diversidad
```

# Bitácora de la práctica:

## Resultados

```bash
•	Flujograma de todo el pipeline realizado 
•	Valores de calidad y numero de lecturas en la data cruda y después de la limpieza 
•	Análisis de abundancia en todos los niveles taxonómicos
•	Análisis de diversidad alfa (seleccionar 3 métricas) a nivel de género y familia 
•	Análisis de beta diversidad (PCoA y NMDS) a nivel de género y familia
```

## Discusión

```bash
•	Explicar que métrica de la diversidad alfa representa mejor la diversidad de las muestras
•	Explicar cual metodo de ordinación de la diversidad beta explica mejor la similaridad/disimilaridad de las muestras
```















