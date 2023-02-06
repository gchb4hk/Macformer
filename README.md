# Macformer
This repo implements macrocyclization of linear molecules to generate macrocycles with chemical diversity and structural novelty. 

## Setup
Install Macformer from the .yaml file  
```
conda env create -f Macformer_env.yaml  
conda activate Macformer  
```

## Quick Start
### 1. Data processing
The acyclic-macrocyclic SMILES pairs extracted from ChEMBL and ZINC database, respectively, can be found in the ***data/*** folder. Or researcher can process their own macrocyclic compounds from scratch using scripts in the ***utils/*** folder.  

```
fragmentation.py:  generate unique acyclic-macrocyclic SMILES pairs  
data_split.py:  split the acyclic-macrocyclic SMILES pairs into train, validation, and test datasets 
data_augmentation.py:  implement substructure-aligned data augmentation  
```

### 2. Input files generation
The ***preprocessing.sh*** script will generate following input files necessary to train the model.  

```
*.train.pt: serialized PyTorch file containing training data  
*.valid.pt: serialized PyTorch file containing validation data  
*.vocab.pt: serialized PyTorch file containing vocabulary data  
```

### 3. Model training
Run the ***training.sh*** script to start model training.   
The saved checkpoints can be averaged by running the ***average_models.sh*** script.  

### 4. Model evaluation
Run the ***testing_beam_search.sh*** script to obtain predicted molecules.  
The ***utils/model_evaluation.py*** script can be used to calculate the evaluation metrics, including recovery, validity, uniqueness, novelty, and macrocyclization.  

To compare our model with previously reported non-deep learning approaches, we proposed a pipeline to construct macrocycles from three-dimensional (3D) structures of linear compounds through linker database searching (termed as MacLS). The detailed script can be found in the MacLS.py of the Utils fold.

### 5. Pre-trained models and results reproduction
The models pretrained with ChEMBL dataset can be found in the ***models/*** folder.  
The metrics can be reproduced by the pre-trained models using internal ChEMBL test dataset (***data/ChEMBL/a10/src-testa10***) and external ZINC test dataset (***data/ZINC/src-external-zinc-a10***).

Tabel 1. Comparison of Macformer with different augmentation numbers on ChEMBL test dataset.
| Training data augmentation   | Recovery(%)   | Validity(%)   | Uniqueness(%)   | Novelty(%)   | Macrocyclization(%)   |
|------------------------------|---------------|---------------|-----------------|--------------|-----------------------|
| None                         | 54.85±14.28   | 66.74±2.29    | 63.18±6.38      | 89.30±1.94   | 95.00±0.74            |
| ×2                           | 96.09±0.61    | 80.34±1.38    | 64.43±0.23      | 91.58±0.15   | 98.62±0.17            |
| ×5                           | 97.54±0.16    | 81.94±1.42    | 65.36±0.13      | 91.79±0.16   | 98.80±0.11            |
| ×10                          | 97.02±0.05    | 82.59±1.57    | 64.44±0.46      | 91.76±0.22   | 98.46±0.04            |
|Method   |Training data augmentation   | Recovery(%)   | Validity(%)   | Uniqueness(%) | Novelty(mol,%) |Novelty(linker,%)|Macrocyclization(%)|        
|---------|-----------------------------|---------------|---------------|---------------|----------------|-----------------|-------------------|        
|Macformer|None                         | 54.85±14.28   | 66.74±2.29    | 63.18±6.38    | 89.30±1.94     | 40.56±2.33      | 95.00±0.74        |  
|Macformer|×2                           | 96.09±0.61    | 80.34±1.38    | 64.43±0.23    | 91.58±0.15     | 58.91±0.36      | 98.62±0.17        | 
|Macformer|×5                           | 97.54±0.16    | 81.94±1.42    | 65.36±0.13    | 91.79±0.16     | 62.11±0.65      | 98.80±0.11        | 
|Macformer|×10                          | 97.02±0.05    | 82.59±1.57    | 64.44±0.46    | 91.76±0.22     | 60.27±0.96      | 98.46±0.04        | 
|MacLS    |×5                           | 0.01±0.01     | 17.05±0.29    | 95.33±0.01    | 100±0.00       | 0.00±0.00       | 100±0.00          | 
|MacLS    |×10                          | 4.16±0.20     | 89.65±0.03    | 96.32±0.06    | 99.65±0.02     | 0.00±0.00       | 100±0.00          | 


Tabel 2. Comparison of Macformer with different augmentation numbers on ZINC test dataset.
| Training data augmentation   | Recovery(%)   | Validity(%)   | Uniqueness(%)   | Novelty(%)   | Macrocyclization(%)   |
|------------------------------|---------------|---------------|-----------------|--------------|-----------------------|
| None                         | 2.70±1.31     | 72.91±2.05    | 47.74±8.98      | 96.10±0.81   | 96.39±0.71            |
| ×2                           | 76.37±3.23    | 81.97±1.20    | 44.99±5.37      | 99.31±0.19   | 99.48±0.08            |
| ×5                           | 81.86±0.75    | 84.73±1.01    | 45.14±4.60      | 99.39±0.09   | 99.53±0.05            |
| ×10                          | 84.25±0.84    | 85.35±1.33    | 45.26±0.46      | 99.43±0.09   | 99.27±0.07            |
