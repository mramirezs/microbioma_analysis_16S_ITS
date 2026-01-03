# Análisis de Microbioma

## 📋 Descripción del Proyecto

Este repositorio contiene el pipeline completo para el análisis de microbioma en muestras de oído de perros con y sin otitis, utilizando secuenciación de amplicones 16S rRNA (bacterias) e ITS (hongos).

### Objetivos
1. Caracterizar la microbiota bacteriana (16S) y fúngica (ITS) en oídos caninos
2. Comparar la composición microbiana entre:
   - Perros con otitis vs sin otitis
   - Oído derecho vs oído izquierdo
   - Perros con historial de dermatitis atópica vs controles
3. Identificar biomarcadores asociados con otitis
4. Predecir funciones metabólicas de las comunidades microbianas

## 🔬 Diseño Experimental

### Grupos de Muestras

| Oído | Condición | Muestras | Historia Clínica |
|------|-----------|----------|------------------|
| Derecho | Otitis | ECU, SEU, WLU | Dermatitis atópica previa |
| Derecho | Sano | RMU | Recuperado de dermatitis |
| Derecho | Control | EEU | Sin historial de dermatitis |
| Izquierdo | Otitis | SED, LKD | Dermatitis atópica previa |
| Izquierdo | Sano | RMD, LLD, TBD | Recuperado de dermatitis |
| Izquierdo | Control | EEU | Sin historial de dermatitis |

**Total: 10 muestras** (EEU es control compartido)

### Estrategia de Secuenciación
- **Plataforma**: Illumina MiSeq (Paired-end)
- **16S rRNA**: Región hipervariable V3-V4
- **ITS**: Región ITS1 o ITS2
- **Profundidad**: ~70,000-100,000 reads por muestra

## 📁 Estructura del Repositorio
```
ECUADOR/
├── 16S2512872/                 # Análisis bacteriano (16S)
│   ├── 1_QC/                   # Control de calidad
│   ├── 2_OTU_Taxa/             # Clustering y taxonomía
│   ├── 3_AlphaDiversity/       # Diversidad alfa
│   ├── 4_BetaDiversity/        # Diversidad beta
│   ├── 5_GroupAnalysis/        # Análisis de grupos
│   ├── 6_Taxonomic/            # Composición taxonómica
│   ├── 7_Differential/         # Análisis diferencial
│   ├── 8_Environmental/        # Análisis ambiental (opcional)
│   ├── 9_Network/              # Análisis de redes
│   ├── 10_Function/            # Predicción funcional
│   ├── 11_Reports/             # Reportes finales
│   ├── metadata/               # Metadatos de muestras
│   ├── scripts/                # Scripts de análisis
│   └── logs/                   # Logs de ejecución
│
├── ITS2512872/                 # Análisis fúngico (ITS)
│   └── [misma estructura]
│
└── shared/                     # Recursos compartidos
    ├── databases/              # Bases de datos de referencia
    ├── references/             # Literatura y referencias
    ├── scripts/                # Scripts reutilizables
    └── conda_envs/             # Ambientes conda exportados
```

## 🛠️ Software y Dependencias

### Ambientes Conda

Este proyecto utiliza 7 ambientes conda especializados:

1. **microbiome_qc**: Control de calidad y preprocesamiento
2. **microbiome_otu**: Clustering de OTUs y taxonomía
3. **microbiome_phylo**: Análisis filogenético
4. **microbiome_r**: Análisis estadístico en R
5. **microbiome_viz**: Visualización avanzada
6. **microbiome_func**: Predicción funcional
7. **microbiome_diff**: Análisis diferencial (LEfSe)

### Software Principal

| Categoría | Software | Versión | Propósito |
|-----------|----------|---------|-----------|
| **QC** | cutadapt | 4.4 | Remoción de adaptadores |
| | PEAR | 0.9.6 | Ensamblaje paired-end |
| | PRINSEQ | 0.20.4 | Filtrado de calidad |
| | FastQC | 0.12.1 | Reporte de calidad |
| **Clustering** | VSEARCH | 2.22.1 | Clustering de OTUs |
| | mothur | 1.48.0 | Análisis de diversidad |
| **Taxonomía** | RDP classifier | 2.13 | Clasificación taxonómica |
| | BLAST+ | 2.14.0 | Alineamiento de secuencias |
| **Filogenética** | MAFFT | 7.505 | Alineamiento múltiple |
| | FastTree | 2.1.11 | Construcción de árboles |
| **Estadística** | R | 4.2.3 | Análisis estadístico |
| | phyloseq | 1.42.0 | Análisis de microbioma |
| | vegan | 2.6 | Ecología numérica |
| | DESeq2 | 1.38.0 | Análisis diferencial |
| **Funcional** | PICRUSt2 | 2.5.2 | Predicción de funciones |
| | FAPROTAX | 1.2.4 | Funciones procariontes |
| **Visualización** | Krona | 2.8.1 | Gráficos interactivos |
| | GraPhlAn | 1.1.3 | Árboles filogenéticos |

Ver [software_versions_comparison.md](docs/software_versions_comparison.md) para detalles completos.

## 🚀 Instalación

### 1. Clonar el repositorio
```bash
git clone https://github.com/tu-usuario/canine-otitis-microbiome.git
cd canine-otitis-microbiome
```

### 2. Crear estructura de directorios
```bash
bash scripts/setup/create_directory_structure.sh
```

### 3. Instalar ambientes conda
```bash
bash scripts/setup/install_conda_environments.sh
```

### 4. Descargar bases de datos
```bash
bash scripts/setup/download_databases.sh
```

### 5. Verificar instalación
```bash
bash scripts/setup/check_installation.sh
```

## 📊 Pipeline de Análisis

### Análisis 16S (Bacterias)
```bash
# 1. Preprocesamiento
bash scripts/preprocessing/01_quality_control.sh

# 2. Clustering de OTUs
bash scripts/clustering/02_otu_clustering.sh

# 3. Asignación taxonómica
bash scripts/taxonomy/03_taxonomy_assignment.sh

# 4. Análisis de diversidad
bash scripts/diversity/04_alpha_diversity.sh
bash scripts/diversity/05_beta_diversity.sh

# 5. Análisis estadístico
bash scripts/statistical/06_group_comparison.sh
bash scripts/statistical/07_differential_abundance.sh

# 6. Predicción funcional
bash scripts/functional/08_picrust2_analysis.sh

# 7. Generación de reportes
bash scripts/reporting/09_generate_report.sh
```

### Análisis ITS (Hongos)
```bash
# Pipeline similar adaptado para ITS
bash scripts/its_pipeline/run_its_analysis.sh
```

## 📈 Resultados Esperados

### Análisis de Diversidad
- Índices alfa (Shannon, Simpson, Chao1, ACE)
- Análisis beta (PCoA, NMDS, PERMANOVA)
- Curvas de rarefacción

### Composición Taxonómica
- Gráficos de barras por filo/género
- Heatmaps de abundancia
- Diagramas circulares (Circos)
- Árboles filogenéticos anotados

### Análisis Diferencial
- Taxa diferencialmente abundantes (LEfSe)
- Biomarcadores potenciales
- Random Forest para clasificación

### Predicción Funcional
- Funciones metabólicas (KEGG)
- Procesos funcionales (COG)
- Vías metabólicas enriquecidas

## 📝 Metadatos

Los metadatos de las muestras se encuentran en:
- `16S2512872/metadata/sample_metadata.txt`
- `ITS2512872/metadata/sample_metadata.txt`

Columnas incluidas:
- SampleID, Group, Ear, Condition, History
- SeqNum, BaseNum, MeanLen, Barcode

## 🔍 Control de Calidad

### Criterios de Filtrado
- Calidad mínima (Phred): Q20
- Longitud mínima: Variable según región
- Remoción de quimeras: Sí
- Cobertura mínima: Coverage > 0.99

### Estadísticas de Secuenciación

Ver `1_QC/QC_Reports/` para reportes detallados.

## 📚 Referencias

### Bases de Datos
- **SILVA 138**: 16S/18S rRNA
- **UNITE**: ITS fúngico
- **GTDB**: Taxonomía genómica
- **KEGG**: Funciones metabólicas

### Literatura Clave
1. Callahan et al. (2016) DADA2: High-resolution sample inference
2. Douglas et al. (2020) PICRUSt2 for metagenome function prediction
3. McMurdie & Holmes (2013) phyloseq for microbiome analysis
4. Segata et al. (2011) LEfSe for biomarker discovery

## 👥 Autores y Contacto

- **Investigador Principal**: [Nombre]
- **Bioinformática**: [Tu Nombre]
- **Institución**: [Institución]
- **Contacto**: [email]

## 📄 Licencia

Este proyecto está bajo licencia MIT. Ver [LICENSE](LICENSE) para detalles.

## 🙏 Agradecimientos

- BTS CONSULTORES S.A.C. por el reporte de referencia
- Comunidad de Bioconductor y QIIME2
- Desarrolladores de herramientas bioinformáticas

## 📌 Notas Importantes

### USEARCH vs VSEARCH
Por licenciamiento, utilizamos **VSEARCH** como alternativa open-source a USEARCH. Los resultados son comparables.

### Reproducibilidad
Todos los ambientes conda pueden ser exportados:
```bash
conda env export -n microbiome_qc > shared/conda_envs/microbiome_qc.yml
```

### Actualizaciones
Este repositorio se actualiza regularmente. Ver [CHANGELOG.md](CHANGELOG.md) para cambios.

---

**Última actualización**: Enero 2026
**Versión del pipeline**: 1.0.0
