# Semana 14: Análisis shotgun utilizando datos de secuenciación de Nanopore

## Logro de la sesión: 

Al finalizar la sesión, el estudiante conoce y utiliza herramientas bioinformáticas para realizar un análisis shotgun a partir de los datos de secuenciación Nanopore.

# Estructura de la práctica:

1. Acceso al servidor de cómputo
2. Análisis de calidad de la secuenciación Nanopore 
3. Limpieza de los archivos FASTQ
4. Eliminación de la contaminación en los archivos FASTQ
5. Identificación del perfil taxonómico a partir de los archivos FASTQ 
6. Ensamblaje de contigs
7. Binning (Agrupación y evaluación de MAGs)
8. Anotación funcional global de los contigs
9. Identificación de genes de resistencia y virulencia en los contigs
10. Identificación de plásmidos en los contigs
11. Identificación de rutas metabólicas en los contigs
12. Identificación de clústeres de genes de biosíntesis de metabolitos secundarios en los contigs

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

gunzip -c b01_porechop.fastq.gz | NanoFilt -q 10 --length 1000 | gzip > b01_nanofilt.fastq.gz

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

kraken2 -db /data/db/kraken2/k2_pluspf --threads 15 --use-names cd ~/shotgun/contamination/b01_clean_hsa.fastq --output b01.kraken --report b21.report

kreport2krona.py -r b01.report -o b01.krona

ktImportText b01.krona -o b01_krona_report.html
```

```bash
# Exportar el archivo HTML y visualizarlo
```

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/ab5980cc-7f58-4033-9bab-e3c019ba930c" />

```bash
# Exportar el archivo REPORT y visualizar en el programa pavian (https://fbreitwieser.shinyapps.io/pavian/)
```

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/b4807b30-5959-4206-812b-b45fe612fe63" />

<img width="1000" alt="image" src="https://github.com/user-attachments/assets/7247fdcb-6c44-4c80-a1ea-39375ceb0c26" />
   
## 6. Ensamblaje de contigs

```bash
cd ~/shotgun/

mkdir assembly

cd assembly

flye --nano-hq ~/shotgun/contamination/b01_clean_hsa.fastq --meta --threads 15 --out-dir b01

mv b01/assembly.fasta b01_assembly.fasta

metaquast.py -m 1000 --gene-finding -r /data/BL16/nanopore/shotgun_24_1/sacha/GCA_000146045.2_R64_genomic.fna -o b01_assemble_stats b01_assembly.fasta
```

## 7. Binning (Agrupación y evaluación de MAGs)

```bash
cd ~/shotgun/

mkdir binning

cd binning

minimap2 -t 15 -ax map-ont ~/shotgun/assembly/b01_assembly.fasta ~/shotgun/contamination/b01_clean_hsa.fastq | samtools sort -O BAM - > b01.aln.sort.bam

samtools index -@ -b b24.aln.sort.bam

jgi_summarize_bam_contig_depths --outputDepth b01_depth.txt b01.aln.sort.bam

metabat2 -m 1500 -t 15 -i ~/shotgun/assembly/b01_assembly.fasta -a b21_depth.txt -o .

conda activate checkm

checkm lineage_wf -t 15 -x fa . --tab_table -f b01_checkm_out.txt .
```

## 8. Anotación funcional global de los contigs

```bash
cd ~/shotgun/

mkdir annotation

cd annotation

conda activate pgcgap

prokka --outdir prokka --cpus 15 --metagenome --prefix b01 ~/shotgun/assembly/b01_assembly.fasta
```

### Identificar los códigos y descripción de las rutas metabólicas presentes en los contigs con el programa minpath:

```bash
egrep "eC_number=" prokka/b01.gff |cut -f9 | cut -f1,2 -d ';'| sed 's/ID=//g'| sed 's/;eC_number=/\t/g' > b01_ec.txt

python /data/db/minpath/MinPath.py -any b01_ec.txt -map /data/db/minpath/data/ec2path -report b01_ec.report
```

### Visualizar las rutas metabólicas más relevantes en MetaCyc (https://metacyc.org/):

<img width="543" alt="image" src="https://github.com/user-attachments/assets/e6ef2663-f50a-4197-af0e-23f4279099d3" />

<img width="445" alt="image" src="https://github.com/user-attachments/assets/e1edce45-58bd-4bfd-a4d3-500420b88c17" />

<img width="694" alt="image" src="https://github.com/user-attachments/assets/d0ed055d-6987-40e0-8bb0-9f81edb65919" />

## 9. Identificación de genes de resistencia y virulencia en los contigs

```bash
cd ~/shotgun/annotation

mkdir abricate

cd abricate

conda activate abricate

abricate --db vfdb ~/shotgun/annotation/prokka/b01.fna > b01_vfdb.tab
```

```bash
cd ~/shotgun/annotation

mkdir rgi

cd rgi

conda activate resistance

rgi load --card_json /data/db/card/card.json  --local

rgi main --input_sequence ~/shotgun/annotation/prokka/b01.fna --output_file b01_rgi --clean --local --num_threads 15
```

## 10. Identificación de plásmidos en los contigs

```bash
cd ~/shotgun/annotation

conda activate mob

mkdir plasmid

cd plasmid

mob_recon --infile ~/shotgun/annotation/prokka/b01.fna --outdir b01_plasmid
```

## 11. Identificación de rutas metabólicas en los contigs

### Ir a la pagina web de KAAS (https://www.genome.jp/kegg/kaas/)

### Hacer clic en KAAS job request

<img width="681" alt="image" src="https://github.com/user-attachments/assets/79333efb-e92d-4ec5-ad25-1cdc4ba7be86" />

### Clic en seleccionar archivo y escoger el archivo FAA del genoma, colocar en query name e e-mail address lo que corresponda, seleccionar en GENES data set la opción for Prokaryotes, y luego presionar en Compute

<img width="521" alt="image" src="https://github.com/user-attachments/assets/a9bd91b6-8d9e-4ded-b9e7-706373a7ad6a" />

### Revisar su e-mail y dar clic en Submit

### Hacer clic en html

<img width="287" alt="image" src="https://github.com/user-attachments/assets/4e8a1196-68d9-4560-8028-052c5fd9023d" />

### Explorar los resultados

<img width="330" alt="image" src="https://github.com/user-attachments/assets/1bcea43b-3b75-4916-907a-6fd170767772" />

### Explorar los resultados

<img width="427" alt="image" src="https://github.com/user-attachments/assets/3c0a54d4-f110-4ac7-b3b1-ee38251c2b61" />

<img width="694" alt="image" src="https://github.com/user-attachments/assets/0e07f44b-355d-40e2-b5a3-613aaeb8f993" />

## 12. Identificación de clústeres de genes de biosíntesis de metabolitos secundarios en los contigs

### Ir a la pagina web de antiSMASH (https://antismash.secondarymetabolites.org/#!/start)

### Clic en Upload file y escoger el archivo FASTA del genoma, colocar el Detection strictness en strict, seleccionar All on la opción Extra features, y luego presionar en Submit

<img width="534" alt="image" src="https://github.com/user-attachments/assets/3f1b86f4-3628-4f25-a5cf-5d261ba743df" />

### Explorar los resultados

<img width="509" alt="image" src="https://github.com/user-attachments/assets/06580a7f-3c4d-44d5-95ad-82f9397a69f1" />

<img width="695" alt="image" src="https://github.com/user-attachments/assets/6b030610-5686-4ebc-acd7-cd9be363f14d" />

# Bitácora de la práctica:

## Resultados

```bash
•	Flujograma de todo el pipeline realizado 
•	Valores de calidad y numero de lecturas en la data cruda y después de la limpieza
•	Analisis taxonómico 
•	Estadisticas del ensamblaje
•	Valores de integridad y contaminación de los MAGs obtenidos
•	Estadisticas de la anotación global
•	Rutas metabólicas más abundantes
•	Genes de resistencia y virulencia identificados
•	Plásmidos identificados
•	Clústeres de metabólitos identificados
```

## Para el trabajo final

```bash
•	Análisis de la anotación de los 3 MAGs con mejor calidad
```














