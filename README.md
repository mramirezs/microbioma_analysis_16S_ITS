# Análisis de Microbioma en Otitis Canina - 16S rRNA e ITS

## 📋 Descripción del Proyecto

Pipeline bioinformático completo para el análisis de microbioma en muestras de oído de perros con y sin otitis, utilizando secuenciación de amplicones:
- **16S rRNA** para caracterización bacteriana
- **ITS** para caracterización fúngica

### Contexto Clínico
La otitis canina es una enfermedad común que afecta el canal auditivo, frecuentemente asociada con dermatitis atópica. Este estudio investiga la composición microbiana del oído en diferentes estados clínicos para identificar biomarcadores y comprender la progresión de la enfermedad.

## 🔬 Diseño Experimental

### Grupos de Muestras (n=10)

**Oído Derecho:**
| Condición | Muestras | N | Historia Clínica |
|-----------|----------|---|------------------|
| Otitis | ECU, SEU, WLU | 3 | Dermatitis atópica previa |
| Sano (Recuperado) | RMU | 1 | Recuperado de dermatitis |
| Control | EEU | 1 | Sin historial de dermatitis |

**Oído Izquierdo:**
| Condición | Muestras | N | Historia Clínica |
|-----------|----------|---|------------------|
| Otitis | SED, LKD | 2 | Dermatitis atópica previa |
| Sano (Recuperado) | RMD, LLD, TBD | 3 | Recuperado de dermatitis |
| Control | EEU* | - | Sin historial de dermatitis |

*EEU: Control compartido para ambos oídos

### Comparaciones Principales
1. **Otitis vs Sano vs Control** - Identificar marcadores de enfermedad
2. **Oído Derecho vs Izquierdo** - Evaluar lateralidad
3. **Con historial vs Sin historial** - Efecto de dermatitis atópica previa

### Estrategia de Secuenciación
- **Plataforma**: Illumina MiSeq (Paired-end 2×300 bp)
- **16S rRNA**: Región V3-V4 (~460 bp)
- **ITS**: Región ITS1/ITS2
- **Profundidad promedio**: ~85,000 reads/muestra
- **Rango de reads**: 70,543 - 103,697 por muestra

## 📁 Estructura del Repositorio
```
microbioma_analysis_16S_ITS/
├── 16S2512872/                      # Análisis bacteriano (16S rRNA)
│   ├── 1_QC/                        # Control de calidad
│   │   ├── 1_RawData/              # Datos crudos (no en GitHub)
│   │   ├── 2_CleanData/            # Datos limpios
│   │   └── QC_Reports/             # Reportes de calidad
│   ├── 2_OTU_Taxa/                 # Clustering y taxonomía
│   │   ├── OTU/                    # Tablas de OTUs
│   │   ├── Abundance/              # Tablas de abundancia
│   │   └── Phylogeny/              # Árboles filogenéticos
│   ├── 3_AlphaDiversity/           # Diversidad alfa
│   ├── 4_BetaDiversity/            # Diversidad beta
│   ├── 5_GroupAnalysis/            # PERMANOVA, ANOSIM, etc.
│   ├── 6_Taxonomic/                # Composición taxonómica
│   ├── 7_Differential/             # Análisis diferencial (LEfSe, DESeq2)
│   ├── 8_Environmental/            # Análisis ambiental (opcional)
│   ├── 9_Network/                  # Redes de co-ocurrencia
│   ├── 10_Function/                # Predicción funcional (PICRUSt2)
│   ├── 11_Reports/                 # Reportes finales
│   ├── metadata/                   # Metadatos de muestras
│   ├── scripts/                    # Scripts específicos 16S
│   └── logs/                       # Logs de ejecución
│
├── ITS2512872/                      # Análisis fúngico (ITS)
│   └── [misma estructura que 16S]
│
├── shared/                          # Recursos compartidos
│   ├── databases/                  # Bases de datos de referencia
│   │   ├── 16S/                   # SILVA, RDP, GTDB
│   │   ├── 18S/                   # SILVA 18S
│   │   ├── ITS/                   # UNITE
│   │   └── functional/            # KEGG, COG, etc.
│   ├── conda_envs/                # Archivos YAML de ambientes
│   ├── references/                # Literatura y referencias
│   └── scripts/                   # Scripts reutilizables
│
├── docs/                           # Documentación
│   ├── tutorials/                 # Tutoriales paso a paso
│   ├── references/                # Referencias bibliográficas
│   ├── figures/                   # Figuras para documentación
│   └── software_versions.md       # Versiones de software
│
└── scripts/                        # Scripts maestros
    ├── setup/                     # Instalación y configuración
    ├── preprocessing/             # Preprocesamiento
    ├── analysis/                  # Análisis principales
    └── utils/                     # Utilidades
```

## 🛠️ Software y Dependencias

### Ambientes Conda Especializados

El proyecto utiliza 7 ambientes conda para organizar las dependencias:

| Ambiente | Propósito | Software Principal |
|----------|-----------|-------------------|
| `microbiome_qc` | Control de calidad | cutadapt, PEAR, PRINSEQ, FastQC |
| `microbiome_otu` | Clustering OTUs | VSEARCH, mothur, BLAST+ |
| `microbiome_phylo` | Análisis filogenético | MAFFT, FastTree, MUSCLE |
| `microbiome_r` | Análisis estadístico | R, phyloseq, vegan, DESeq2 |
| `microbiome_viz` | Visualización | Krona, GraPhlAn, ETE3 |
| `microbiome_func` | Predicción funcional | PICRUSt2, FAPROTAX |
| `microbiome_diff` | Análisis diferencial | LEfSe, STAMP |

### Software Principal por Categoría

**Control de Calidad:**
- cutadapt 4.4 - Remoción de adaptadores y primers
- PEAR 0.9.6 - Ensamblaje de reads paired-end
- PRINSEQ 0.20.4 - Filtrado por calidad
- FastQC 0.12.1 / MultiQC 1.14 - Reportes de calidad

**Clustering y Taxonomía:**
- VSEARCH 2.22.1 - Clustering de OTUs (alternativa open-source a USEARCH)
- mothur 1.48.0 - Análisis de diversidad y comunidades
- RDP classifier 2.13 - Clasificación taxonómica
- BLAST+ 2.14.0 - Alineamiento de secuencias

**Análisis Estadístico (R):**
- phyloseq 1.42.0 - Análisis integrado de microbioma
- vegan 2.6 - Ecología numérica y análisis multivariado
- DESeq2 1.38.0 - Análisis de abundancia diferencial
- ggplot2 3.4.2 - Visualización de datos
- mixOmics 6.22.0 - Análisis multivariado integrativo

**Predicción Funcional:**
- PICRUSt2 2.5.2 - Predicción de funciones metagenómicas
- Tax4Fun2 - Predicción funcional alternativa
- FAPROTAX 1.2.4 - Funciones de procariontes

**Visualización Especializada:**
- Krona 2.8.1 - Gráficos taxonómicos interactivos
- GraPhlAn 1.1.3 - Árboles filogenéticos anotados
- ETE3 3.1.2 - Manipulación de árboles filogenéticos

Ver [docs/software_versions.md](docs/software_versions.md) para la lista completa y comparación con el reporte de referencia.

## 🚀 Instalación y Configuración

### Requisitos Previos
- Linux/macOS (recomendado) o Windows con WSL2
- Conda/Mamba instalado
- Git instalado
- Mínimo 32 GB RAM recomendado
- ~100 GB espacio en disco

### 1. Clonar el Repositorio
```bash
git clone https://github.com/mramirezs/microbioma_analysis_16S_ITS.git
cd microbioma_analysis_16S_ITS
```

### 2. Crear Estructura de Directorios
```bash
bash scripts/setup/create_directory_structure.sh
```

### 3. Instalar Ambientes Conda
```bash
# Instalar todos los ambientes
bash scripts/setup/install_conda_environments.sh

# O instalar uno por uno
bash scripts/setup/install_individual_env.sh microbiome_qc
```

### 4. Descargar Bases de Datos
```bash
bash scripts/setup/download_databases.sh
```

### 5. Verificar Instalación
```bash
bash scripts/setup/check_installation.sh
```

## 📊 Pipeline de Análisis

### Análisis 16S (Bacterias)
```bash
# Pipeline completo automatizado
bash scripts/run_16S_pipeline.sh

# O ejecutar paso a paso:

# 1. Control de calidad
bash scripts/preprocessing/01_quality_control.sh

# 2. Clustering de OTUs
bash scripts/clustering/02_otu_clustering.sh

# 3. Asignación taxonómica
bash scripts/taxonomy/03_taxonomy_assignment.sh

# 4. Análisis de diversidad
bash scripts/diversity/04_alpha_diversity.sh
bash scripts/diversity/05_beta_diversity.sh

# 5. Comparación de grupos
bash scripts/statistical/06_group_comparison.sh

# 6. Análisis diferencial
bash scripts/statistical/07_differential_abundance.sh

# 7. Predicción funcional
bash scripts/functional/08_functional_prediction.sh

# 8. Reporte final
bash scripts/reporting/09_generate_report.sh
```

### Análisis ITS (Hongos)
```bash
# Pipeline completo para ITS
bash scripts/run_ITS_pipeline.sh
```

## 📈 Resultados Esperados

### Métricas de Calidad
- **Reads totales**: ~850,000 (10 muestras)
- **Reads por muestra**: 70,543 - 103,697
- **Longitud promedio**: 270-302 bp
- **Calidad (Phred)**: >Q30
- **Cobertura estimada**: >99% (Good's coverage)

### Análisis de Diversidad
- **Índices alfa**: Shannon, Simpson, Chao1, ACE, Coverage
- **Diversidad beta**: PCoA, NMDS, PERMANOVA
- **Curvas de rarefacción**: Saturación de muestreo
- **Comparaciones estadísticas**: ANOSIM, PERMANOVA, PLSDA

### Composición Taxonómica
- **Gráficos de barras**: Por filo, clase, orden, familia, género
- **Heatmaps**: Abundancia de taxa principales
- **Diagramas circulares**: Composición por muestra
- **Árboles filogenéticos**: Con abundancia anotada

### Análisis Diferencial
- **LEfSe**: Biomarcadores por condición (LDA score >2)
- **DESeq2**: Taxa diferencialmente abundantes (padj <0.05)
- **Random Forest**: Importancia de variables
- **ROC curves**: Poder discriminatorio

### Predicción Funcional
- **KEGG Pathways**: Vías metabólicas predichas
- **COG categories**: Categorías funcionales
- **Enzyme classes**: Distribución de enzimas
- **Differential functions**: Funciones enriquecidas por grupo

## 📊 Metadatos de Muestras

Los metadatos completos están en:
- `16S2512872/metadata/sample_metadata.txt`
- `ITS2512872/metadata/sample_metadata.txt`

**Columnas incluidas:**
- `SampleID`: Identificador único de muestra
- `Group`: Grupo experimental (Otitis_Right, Healthy_Left, etc.)
- `Ear`: Lateralidad (Right/Left)
- `Condition`: Condición clínica (Otitis/Healthy/Control)
- `History`: Historia clínica (Atopic_Dermatitis/Recovered/Never_Atopic)
- `SeqNum`: Número de secuencias
- `BaseNum`: Número de bases
- `MeanLen`: Longitud promedio
- `Barcode`: Código de barras

## 🔍 Control de Calidad

### Criterios de Filtrado
- **Calidad mínima (Phred)**: Q20
- **Longitud mínima**: 200 bp
- **Longitud máxima**: 500 bp
- **Contenido N's**: 0
- **Complejidad**: Low complexity sequences removed
- **Quimeras**: Removidas con VSEARCH

### Umbral de Clustering
- **OTU clustering**: 97% similaridad (especies aproximadas)
- **ASV detection**: Resolución de nucleótido único (opcional)

## 📚 Bases de Datos de Referencia

### Para 16S rRNA
- **SILVA 138**: Base de datos principal (SSU Ref NR99)
- **RDP 11.5**: Clasificación alternativa
- **GTDB r207**: Taxonomía basada en genomas (para full-length)

### Para ITS
- **UNITE 9.0**: Base de datos principal para hongos
- **RDP ITS**: Clasificación alternativa

### Para Predicción Funcional
- **KEGG**: Kyoto Encyclopedia of Genes and Genomes
- **COG**: Clusters of Orthologous Groups
- **MetaCyc**: Base de datos de vías metabólicas

## 👥 Equipo del Proyecto

- **Investigador Principal**: [Nombre del Veterinario]
- **Análisis Bioinformático**: Mario Ramirez (mramirezs)
- **Secuenciación**: [Laboratorio/Servicio]
- **Institución**: [Nombre de la Institución]

## 📄 Licencia

Este proyecto está bajo licencia MIT - ver [LICENSE](LICENSE) para detalles.

## 🙏 Agradecimientos

- **BTS CONSULTORES S.A.C.** por el reporte de referencia que guió este análisis
- Comunidades de **Bioconductor**, **QIIME2** y **mothur**
- Desarrolladores de todas las herramientas bioinformáticas utilizadas
- [Servicio de Secuenciación]

## 📖 Referencias Clave

### Metodología
1. Callahan et al. (2016) DADA2: High-resolution sample inference. *Nat Methods* 13:581–583
2. Rognes et al. (2016) VSEARCH: a versatile open source tool. *PeerJ* 4:e2584
3. McMurdie & Holmes (2013) phyloseq: An R package for microbiome analysis. *PLoS ONE* 8(4):e61217

### Análisis Estadístico
4. Love et al. (2014) Moderated estimation of fold change: DESeq2. *Genome Biol* 15:550
5. Segata et al. (2011) Metagenomic biomarker discovery: LEfSe. *Genome Biol* 12:R60

### Predicción Funcional
6. Douglas et al. (2020) PICRUSt2 for metagenome functions. *Nat Biotechnol* 38:685–688
7. Louca et al. (2016) FAPROTAX: Decoupling function and taxonomy. *Science* 353:1272–1277

### Bases de Datos
8. Quast et al. (2013) The SILVA ribosomal RNA gene database. *Nucleic Acids Res* 41:D590–D596
9. Nilsson et al. (2019) The UNITE database for molecular identification of fungi. *Nucleic Acids Res* 47:D259–D264

## 📌 Estado del Proyecto

- [x] Configuración inicial del repositorio
- [x] Diseño experimental definido
- [x] Estructura de directorios creada
- [ ] Instalación de ambientes conda
- [ ] Descarga de bases de datos
- [ ] Preprocesamiento de datos 16S
- [ ] Análisis 16S completado
- [ ] Preprocesamiento de datos ITS
- [ ] Análisis ITS completado
- [ ] Integración de resultados 16S + ITS
- [ ] Reporte final y manuscrito

## 📮 Contacto y Contribuciones

Para preguntas, sugerencias o colaboraciones:
- **Issues**: https://github.com/mramirezs/microbioma_analysis_16S_ITS/issues
- **Email**: [tu email]

---

**Última actualización**: 2 Enero 2026  
**Versión del pipeline**: 1.0.0  
**DOI**: [Pendiente]
