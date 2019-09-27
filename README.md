# garnett-cli
Command line interface for the [Garnett](https://cole-trapnell-lab.github.io/garnett/) cell type classification tool. 
Graphical representation of the general workflow:


![](https://github.com/ebi-gene-expression-group/garnett-cli/blob/master/garnett_pipeline.png)

## Commands 
Currently available Garnett functions are listed in the following section. Each script has usage instructions available via --help, consult function documentation in Garnett for more dertail. 

### Import test data 
In order to test the tool, you will need to extract the CellDataSet (CDS) object holding example expression data in .rds format and the marker text file which are used as initial input to for the workflow. This is done via the following command:

``` 
garnett_import_test_data.R --cds-object <output path to CDS object in .rds format>\
                           --marker-file <putput path to marker file in .txt format>
```

### Build CDS object input from raw expression matrix
If you are using raw experiment data, you can build a CDS object using the following script:

```
parse_expr_data.R --expression-matrix <numeric matrix with expression values in .mtx or .txt format>\
                  --phenotype-data <table of phenotype data in .txt format>\
                  --feature-data <table of gene features in .txt format>\
                  --output-file <path to output file in .rds format> 
```

Please refer [here](http://cole-trapnell-lab.github.io/monocle-release/docs/#the-celldataset-class) for description of required file file structure.

### Check marker file 
In order to verify that markers provide an accurate representation of corresponding cell types, run the following script:

```
garnett_check_markers.R --cds-object <path to CDS object in .rds format>\
                        --marker-file-path <path to marker file in .txt format>\
                        --database <name of gene database (e.g. org.Hs.eg.db for Homo Sapiens genes)>\
                        --cds-gene-id-type <Format of the gene IDs in your CDS object>\
                        --marker-file-gene-id-type <Format of the gene IDs in your marker file>\
                        --marker-output-path <Output path for marker analysis file in .txt format>\
                        --plot-output-path <output path for the plot in .png format>
```
If the flag ` --plot-output-path` is used, graphical representation of marker quality will be produced automatically. 

### Train the classifier 
Although a range of [pre-trained classifiers](https://cole-trapnell-lab.github.io/garnett/classifiers/) are available for usage, you can train your own via the following command: 

```
garnett_train_classifier.R --cds-object <path to CDS object in .rds format>\
                           --marker-file-path <path to marker file in .txt format>\
                           --database <name of gene database (e.g. org.Hs.eg.db for Homo Sapiens genes)>\
                           --output-path <output path for the trained classifier in .rds format>
```

### Get genes used as features in the classification model
In some cases, it might be of interest to investigate which genes are deemed important by the classification model and thus used as features for classification.

```
garnett_get_feature_genes.R --classifier-object <path to a classifier object 
                            (pre-trained or trained _de novo_) in .rds format>\
                             --database <name of gene database (e.g. org.Hs.eg.db for Homo Sapiens genes)>\
                             --output-path <path to output file in .txt format>
```

### Classify cells 
```
garnett_classify_cells.R --cds-object <path to CDS object in .rds format>
                         --classifier-object <path to a classifier object 
                            (pre-trained or trained _de novo_) in .rds format>
                         --database <name of gene database (e.g. org.Hs.eg.db for Homo Sapiens genes)>
```
Classification column will be added to the CDS object's metadata as an additional column. **Note**: to repeat classification on the same CDS object you will need first to delete the column with previous classification result. 

### Decompose CDS object into raw data
In case you would like to extract raw data from the CDS object, run: 
```
make_test_data.R --input-file <path to input CDS object in .rds format>\
                 --expr-matrix <output path for extracted expression matrix in .txt format>\
                 --pheno-data <output path for phenotype data in .txt format>
                 --feature-data <output path for gene features in .txt format>
```





