# Group Assignment: the three problems and their data dictionaries

Each group works on **one** problem (allocated using your group's ranking). Read all three before you rank them.

> All three datasets are **simulated for teaching**. They were built to behave like the real research data described under *Where real data like this comes from*. No real patients or isolates are included.

**For every problem**
- One shared file per problem. Your group's notebook draws your **own sample** from it, using all your members' registration numbers.
- The data dictionary tells you what each column means, its units or allowed values, and **when** it was recorded. Use the "When recorded" column to decide what can be used for prediction.
- Like real data, the files contain mistakes. The dictionary gives the valid range for each column so you can find them.

---

## Problem A. Antimicrobial resistance from bacterial genomes

**The story.** A hospital microbiology laboratory sequences the whole genome of every *Escherichia coli* isolated from patients' urine and blood. Culture-based susceptibility testing takes 2–3 days; sequencing results arrive sooner and are becoming cheaper.

**The question.** *Using information from the genome (and the specimen), can we predict whether an isolate is **resistant to ciprofloxacin**, before the culture result is available?*

**The user of your model:** the microbiologist and the antimicrobial stewardship team, who decide which antibiotic to recommend while culture results are pending.

**File:** `problemA_ecoli_cipro_genomes.csv` · **Your sample:** 2,000 isolates · **One row:** one isolate

| Column | Meaning | Values / units | When recorded |
|---|---|---|---|
| `isolate_id` | laboratory isolate number | text (label only) | at sample receipt |
| `specimen` | type of clinical specimen | urine, blood | at sample receipt |
| `hospital_ward` | ward the patient was on | medical, surgical, paediatric, maternity, ICU, outpatient | at sample receipt |
| `sequence_type` | multilocus sequence type (MLST): the bacterial lineage, assigned from the genome | text such as ST131, ST73 ... | from the genome |
| `gyrA_S83L` | mutation in the *gyrA* gene (DNA gyrase): serine to leucine at position 83 | 1 = present, 0 = absent, blank = position not covered by sequencing reads | from the genome |
| `gyrA_D87N` | mutation in *gyrA*: aspartate to asparagine at position 87 | 1 / 0 / blank | from the genome |
| `parC_S80I` | mutation in *parC* (topoisomerase IV): serine to isoleucine at position 80 | 1 / 0 / blank | from the genome |
| `parC_E84V` | mutation in *parC*: glutamate to valine at position 84 | 1 / 0 / blank | from the genome |
| `qnrS`, `qnrB` | plasmid-borne quinolone-resistance genes (protect DNA gyrase) | 1 = gene detected, 0 = not | from the genome |
| `aac6_Ib_cr` | plasmid gene *aac(6')-Ib-cr*: an enzyme variant that can modify ciprofloxacin | 1 / 0 | from the genome |
| `blaCTX_M_15` | extended-spectrum beta-lactamase gene (resistance to cephalosporins) | 1 / 0 | from the genome |
| `blaTEM_1` | beta-lactamase gene (resistance to ampicillin) | 1 / 0 | from the genome |
| `sul1` | sulfonamide resistance gene | 1 / 0 | from the genome |
| `tetA` | tetracycline resistance gene | 1 / 0 | from the genome |
| `assembly_contigs` | number of pieces (contigs) the genome assembly is in; fewer = better assembly | whole number, at least 1 | from the genome (quality measure) |
| `assembly_N50_bp` | N50 contig length: half the genome is in contigs at least this long | base pairs | from the genome (quality measure) |
| `genome_length_bp` | total assembled genome length | base pairs (*E. coli* is about 4.5–5.6 million bp) | from the genome |
| `cipro_MIC_mg_L` | minimum inhibitory concentration of ciprofloxacin from the culture-based susceptibility test | mg/L | **from the culture test, 2–3 days later** |
| **`cipro_resistant`** | **outcome:** resistant to ciprofloxacin by the culture-based test (EUCAST breakpoint) | 1 = resistant, 0 = susceptible | **from the culture test, 2–3 days later** |

**Background to read**
- How fluoroquinolones such as ciprofloxacin work, and how *gyrA* and *parC* mutations and *qnr* genes affect them.
- Why some resistance genes are often found together on the same plasmid.
- What sequence types (MLST) are, and why ST131 is called a "high-risk clone".

**Where real data like this comes from:** the BV-BRC database links thousands of bacterial genomes to laboratory-tested resistance results ([BV-BRC AMR data](https://p3c.theseed.org/docs/data_protocols/antimicrobial_resistance.html); [data resource paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC8575023)). For an example study, see [Predicting bacterial resistance from whole-genome sequences](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6192184/).

---

## Problem B. Blood gene expression to detect active TB

**The story.** Adults with TB symptoms at three African clinics gave a blood sample. RNA sequencing measured how active each of 156 genes was in their blood. Each patient then had a full reference work-up, and was found to have **active TB**, or **latent TB infection or another illness**.

**The question.** *Can a blood gene-expression test tell **active TB** apart from latent infection and other illnesses?* (Blood is easier to collect than sputum, particularly from people living with HIV.)

**The user of your model:** a diagnostics team deciding which genes to put on a small, cheap blood test.

**File:** `problemB_tb_blood_expression.csv` · **Your sample:** 1,500 patients · **One row:** one patient

| Column | Meaning | Values / units | When recorded |
|---|---|---|---|
| `sample_id` | blood sample number | text (label only) | at enrolment |
| `clinic_site` | clinic and laboratory where the sample was collected and processed | Kampala, Lilongwe, Cape_Town | at enrolment |
| `age_years` | age | years (adults, 18 and over) | at enrolment |
| `sex` | recorded sex | F, M | at enrolment |
| `hiv_positive` | HIV status | 1 = living with HIV, 0 = HIV negative, blank = not recorded | at enrolment |
| `total_reads` | total number of sequencing reads for this sample (sequencing depth) | whole number (usually 5 to 80 million) | from sequencing |
| `RNA_integrity` | RNA integrity number (RIN): quality of the RNA | 1 (degraded) to 10 (intact) | from sequencing (quality measure) |
| `GBP5`, `DUSP3`, `BATF2`, `FCGR1A`, `ANKRD22`, `KLF2`, and 150 further gene columns (`ACTB`, `GAPDH`, `CD3E` ...) | **read counts**: the number of sequencing reads from each gene | whole numbers, 0 or more | from sequencing (blood taken at enrolment) |
| `sputum_xpert_result` | GeneXpert MTB/RIF result on sputum, done as part of the reference work-up | MTB detected, MTB not detected | **during the reference work-up, after enrolment** |
| **`active_tb`** | **outcome:** final reference diagnosis | 1 = active TB, 0 = latent TB infection or other illness | **after the reference work-up** |

**Background to read**
- Why raw read counts depend on sequencing depth, and what counts per million (CPM) and log-transformed counts are.
- What a **batch effect** is in sequencing studies.
- Published blood signatures for TB, e.g. the three-gene GBP5 / DUSP3 / KLF2 signature, and how HIV affects interferon-related genes.

**Where real data like this comes from:** the GEO dataset GSE37250 holds blood gene expression from adults with TB, latent TB and other diseases in Malawi and South Africa, with and without HIV ([dataset](https://www.omicsdi.org/dataset/biostudies-arrayexpress/E-GEOD-37250); [study paper](https://pmc.ncbi.nlm.nih.gov/articles/PMC4838193)). Other studies: [biomarkers for active vs latent TB](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC6930242/).

---

## Problem C. Malaria parasites that clear slowly (artemisinin partial resistance)

**The story.** A therapeutic efficacy study enrolled patients with uncomplicated *Plasmodium falciparum* malaria in eight Ugandan districts. All were treated with artemether-lumefantrine. Parasites were counted on blood films every 6–12 hours to measure how fast they cleared, and the parasite *kelch13* gene was sequenced from the enrolment sample.

**The question.** *Using information available **at enrolment** (patient data and parasite genotype), can we predict which infections will **clear slowly**, a sign of artemisinin partial resistance?*

**The user of your model:** the national malaria control programme, which needs to know where and in whom resistance is emerging.

**File:** `problemC_malaria_clearance.csv` · **Your sample:** 2,500 infections · **One row:** one patient's infection

| Column | Meaning | Values / units | When recorded |
|---|---|---|---|
| `patient_id` | study number | text (label only) | at enrolment |
| `district` | district of the study clinic | Agago, Lamwo, Kitgum (North); Kabarole, Hoima (West); Mubende (Central); Tororo, Busia (East) | at enrolment |
| `age_years` | age | years (from 6 months) | at enrolment |
| `sex` | recorded sex | F, M | at enrolment |
| `weight_kg` | body weight | kg | at enrolment |
| `fever_days_before` | days of fever before coming to the clinic | days | at enrolment |
| `haemoglobin_g_dL` | haemoglobin | g/dL (realistic range about 3–18) | at enrolment |
| `baseline_parasites_per_uL` | parasite density on the enrolment blood film | parasites per microlitre (enrolled patients must have parasites, so more than 0) | at enrolment |
| `kelch13` | *P. falciparum kelch13* genotype: "wild_type" (no mutation) or the amino-acid change found (e.g. C469Y = cysteine to tyrosine at position 469) | text | from the enrolment blood sample |
| `crt_K76T` | *pfcrt* K76T mutation (linked to chloroquine resistance) | 1 = present, 0 = absent | from the enrolment blood sample |
| `mdr1_N86Y` | *pfmdr1* N86Y mutation (linked to sensitivity to several partner drugs) | 1 = present, 0 = absent | from the enrolment blood sample |
| `day3_positive` | parasites still seen on the blood film on day 3 of treatment | 1 = yes, 0 = no | **on day 3 of treatment** |
| **`slow_clearance`** | **outcome:** parasite clearance half-life longer than 5 hours (calculated from all the counts during treatment) | 1 = slow, 0 = normal | **after the first days of treatment** |

**Reference: WHO classification of *kelch13* mutations** (use it if you decide to group mutations)

| Category | Mutations |
|---|---|
| **Validated markers** (linked to delayed clearance in clinical studies and laboratory tests) | F446I, N458Y, C469Y, M476I, Y493H, R539T, I543T, P553L, R561H, P574L, C580Y, R622I, A675V |
| **Candidate markers** (some evidence, not yet confirmed) | P441L, G449A, C469F, A481V, R515K, P527H (and others) |
| Other mutations | any other change, e.g. A578S, which is common in Africa and not linked to delayed clearance |

**Background to read**
- What artemisinin partial resistance is, and why the WHO uses *kelch13* mutations and the clearance half-life to track it ([WHO Q&A](https://www.who.int/news-room/q-a-detail/artemisinin-resistance); [WHO compendium of molecular markers](https://www.who.int/tools/compendium-of-molecular-markers-for-antimalarial-drug-resistance/results)).
- How the parasite clearance half-life is calculated ([WWARN Parasite Clearance Estimator](https://www.iddo.org/sites/default/files/publication/2023-12/WWARN-Parasite-Clearance-Estimator-PCE-Methodology.pdf)).
- Why immunity to malaria builds with age in high-transmission areas.

**Where real data like this comes from:** MalariaGEN Pf7, an open dataset of 20,000 parasite genomes ([Pf7](https://malariagen.net/data_package/open-dataset-plasmodium-falciparum-v70/)), and Ugandan *kelch13* studies such as [Fort Portal, 2024](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC12135530/) and [selection of kelch13 mutations in Uganda](https://pubmed.ncbi.nlm.nih.gov/40112841/).
