# Classifying Antigen Receptors (Antibodies and T-Cell Receptors) with Immunoprofiling

Immunoprofiling is used to assess the diversity of antigen receptors (ARs: antibodies and T-Cell receptors) and how this diversity changes in response to allergens, infections, or vaccines. Understanding AR diversity helps scientists develop new vaccines, improve existing vaccines, and treat allergies and disease. Diversity is assessed by sequencing DNA corresponding to the variable regions of ARs.  

The repository contains the work from a one-week hackathon involving college instructors, high school teachers and students that was focused on leaning immunoprofiling concepts. Hands on activities demonstrated how immunoprofiling works and how it is used in research. 

One part of the project utilized precomputed datasets to learn about the statistical methods that are used in determining AR diversity and clonality, and how clonality is used to immune responses in cancer. Precomputed datasets are tabular data containing sample meta data, and annotate seqences from v-gene, d-gene, and j-gene segments, commonly in [Adaptive Immune Receptor Repertoire(AIRR)](https://docs.airr-community.org/en/stable/) format.  

Another part explored bioinformatics workflows needed to prepare AR sequences for statistical analyses. Team members will work with datasets from different databases like [OPIG](https://opig.stats.ox.ac.uk/webapps/covabdab/), the NCBI short read archive ([example influenza data](https://www.ncbi.nlm.nih.gov/sra/?term=PRJNA349143). A small number of datasets (2 to 4) were prepared and processed with command line tools that include sequence alignment via IgBLAST, data parsers, and statistical counting tools. Python scripts and R will be used to convert IgBLAST output data into tables that can be used to determine immune receptor diversity that includes annotated variable genes and their abundance using counting statistics. We will use a variety of Python libraries, R packages, and work with Jupyter notebooks.  


Learn more at https://digitalworldbiology.com/blog/what-immunoprofiling.

## Resources
### Jetstream
### Software
### Datasets

#### Presentations:
  - link to our google drive progress and final presentations
  - https://drive.google.com/drive/folders/15qxfXJJsoxryRs0C9ubxTFMYgtg8upKx

#### Scripts
  - jupyter notebooks and other affiliated scripts created during the hackathon

#### Datasets

#### What we learned, goals for future
1. Immunoprofiling data are excellent for teaching bioinformatics, raw data needs to be processed and aligend to reference data: teaches workflow and scripting commands concepts. The output, very large tables, require programs to view and analyze data: teaches data science and statistics concepts (what do various plots mean). Working with real life data has purpose, some data are deposited but not published. Curated data have new questions that can be asked. 
2. Be able to accomidate beginning programmers - table manipulations, run programs,
3. Individuals with more experiece can help with infrastructure and workflows for larger data processing activities.
4. Prehackathon, in addition to preparing instances for computing, set up exercises, with small datasets, in Google CoLab. Encourage new programmers to try those. Connect google sheets (small datasets) to pandas to help new programmers understand the data.
5. Work toward scaling projects to incorporate database concepts and machine learning. 
