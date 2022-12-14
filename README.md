# Welcome to Enzymit's Grand Challenge!

## Background
Today you will help us investigate [Polyphosphate Glucokinases](https://en.wikipedia.org/wiki/Polyphosphate%E2%80%94glucose_phosphotransferase). These are enzymes that catalyze the phosphorylation of glucose with polyphosphates. Most kinases perform phosphorylation reactions with ATP, which is an expensive co-factor that imposes high costs on phosphorylation reactions in industrial settings. Having the ability to utilize polyphosphates for phosphorylation reactions can dramatically lower costs of many chemical reactions that are currently not cost effective.

Let's start!

## The Challenge

### Part 1: Initial Setup (Linux Machine)

**Note**: If you prefer working with a google colab instance, you can find a boilerplate notebook in [this link](https://colab.research.google.com/drive/1-czCT3czlhkPIPQlf9im-9LqmC5uD9FM?authuser=1). Create a copy of this notebook in your personal drive and continue straight to Part 2

1. First thing's first. Your first task is to clone this repo to your local machine (or the machine that you were provided with). You can do this by running the following command in your terminal:

    ```
    git clone <repo_url>
    ```

    where `<repo_url>` is the url of this repo. You can find this url by clicking on the green `Code` button on the top right of this page. You can also download the repo as a zip file by clicking on the `Download ZIP` button.



2. Next, you will need to install the dependencies for this project. You can do this by running the following command in your terminal:

    ``` conda env create -f environment.yml```

    This will create a conda environment called `enzymit` with all the dependencies installed. You can activate this environment by running the following command:

    ``` conda activate enzymit```

    You can deactivate this environment by running the following command:

    ``` conda deactivate```

### Part 2: Data Acquisition

Now let's start to gather our data. 
1. In your repo, there's a DNA sequence of an enzyme that was identified as a potential polyphosphate glucokinase. The sequence is stored in the file `data/sequence.fasta`. Your first task is to find the protein structure that corresponds to this DNA sequence. 
We'll develop a short pipeline programatically using the package [Bioservices](https://github.com/cokelaer/bioservices) and [Biopython](https://biopython.org/) Those are python packages that will enable us to work with various biological databases in one scripted pipeline. 

1. Let's use the [Blast](https://blast.ncbi.nlm.nih.gov/Blast.cgi) service to find the protein structure that corresponds to this DNA sequence. You may use the [NCBI](https://bioservices.readthedocs.io/en/main/_modules/bioservices/ncbiblast.html#NCBIblast) service in BioServices to do this. 

Several tips:
- Use the PDB as the database to search against. The PDB Blast database contain protein sequences, you'll need to use the appropriate blast application to search for a protein sequence that matches your **DNA** sequence.
- The query may take some time to complete. In order to avoid running it multiple times, you may cache it and skip the Blast phase once you have it.
- Since the query may take several minutes to run, you will need to query the job status every few seconds in a loop (or use the ```wait``` function of NCBIblast). Use the ```get_status(jobid)``` of the Ncbiblast service to do this. You can find more information about this method [here](https://bioservices.readthedocs.io/en/main/_modules/bioservices/ncbiblast.html#NCBIblast.get_status).
- Choose ```stype="dna"``` in the run function
- Once the job reaches the status of FINISHED, you can retrieve the results using the ```get_result(jobid,result_type='ids')``` method of the Ncbiblast service. You can find more information about this method [here](https://bioservices.readthedocs.io/en/main/_modules/bioservices/ncbiblast.html#NCBIblast.get_results).
The results will be a string of PDB ids separated by newline characters:
```
PDB:1A0A_A
PDB:1A0A_B
PDB:3VGL_A
...
```
Choose the first PDB id in the list (without the chain id, meaning, without the underscore and the characters that follow it) , as it is the most similar to your DNA sequence.
2. Use the urllib package to download the PDB file directly from RCSB. The URL is of the form: ```https://files.rcsb.org/download/<PDB_ID>.pdb```
You can find more information about this package [here](https://docs.python.org/3/library/urllib.html).

3. Use the [Biopython](https://biopython.org/) package to parse the PDB file and extract the sequence of the protein. You can find more information about this package [here](https://biopython.org/wiki/Documentation). Look for the PDBParser class.

4. Extract Chain A from the Structure object. (Hint: use the ```[0]``` operator on the structure object to get the first model, and then use the ```['A']``` operator on the model object to get the first chain).

5. Search for all the phosphate molecules in the chain. (Hint: use the ```get_residues``` method of the chain object to get all the residues in the chain. Then use the ```filter``` method of the atoms object to filter the residues by their name. The phosphate molecules appears in the PDB file as "PO4".

6. Next, find all the residues that interact with the phosphate molecules (Interaction is defined by the residues being < 4A away from the phosphate molecule, and are of a type that is capable of interacting with the charged phosphate). Hint: Use the ```NeighborSearch``` class from Biopython's Bio.PDB. 

7. Repeat the same process (steps 5-6) for finding all residues in the active site. (Hint: find the active site by looking for the residues that are closest to the substrate - glucose which is named "BGC" in the PDB file.

8. Save the results from steps 5-7 in a CSV file called `data/interactions.csv`,  The CSV should have the following format:

```
RESIDUE_NAME,RESIDUE_NUM,LIGAND
ASN,1,PO4
LYS,33,BGC
...
```
Sort the entries in an ascending order by residue number.

Use diff to compare your results to the results in the file `data/interactions_enzymit.csv` in the repo. If you get an empty output, you've successfully completed the coding part of the challenge!

### Part 3: Finding more polyphosphate kinases
The following questions can be answered conceptually only, without writing code. 
In your answers you may assume you have access to Alphafold and can model the 3D structure of all proteins with high accuracy

1. There are very few experimentally validated polyphosphate kinases. How would you find more glucose polyphosphate kinases (from the same fold)? What databases would you use? What would be the steps in your pipeline?
2. How would you find polyphosphate kinases of other substrates (e.g. fructose, sucrose, etc.)? What databases would you use? What would be the steps in your pipeline?


# Good luck!
