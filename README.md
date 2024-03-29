# meekovina

python script easy to use Autodock Vina basic docking simulation

## License

This package is distributed under the MIT License.

## Tutorial

You can try tutorial of meekovina to know how to install and run:  
https://colab.research.google.com/github/mkatouda/meekovina/blob/main/meekovina_tutorial_jp.ipynb

## Requirements

1. python >= 3.7
2. pyyaml (https://pyyaml.org/)
3. numpy (https://numpy.org/)
4. scipy (https://scipy.org/)
5. pandas (https://pandas.pydata.org/)
6. rdkit (https://www.rdkit.org/)
7. meeko >= 0.4.0 (https://github.com/forlilab/Meeko)
8. AutoDock-Vina 1.2.3 binary (https://github.com/ccsb-scripps/AutoDock-Vina/releases/tag/v1.2.3 )  

## Optional

9. vina 1.2.3 python API (https://github.com/ccsb-scripps/AutoDock-Vina )  

## Install

### Install with pip

- Install AutoDock Vina binary

```
wget https://github.com/ccsb-scripps/AutoDock-Vina/releases/download/v1.2.3/vina_1.2.3_linux_x86_64
mkdir ~/bin
mv ./vina_1.2.3_linux_x86_64 ~/bin/vina
chmod 755 ~/bin/vina
export PATH=~/bin/vina:$PATH
```

- Install from github

```bash
pip install git+https://github.com/mkatouda/meekovina.git
```

- Local install

```bash
git clone https://github.com/mkatouda/meekovina.git
cd meekovina
pip install .
```

### Install with conda virtual environment

- Create conda virtual environment  

```
conda create -n py310-meekovina python=3.10   
conda activate py310-meekovina  
```

- Run the following command to install required conda packages  

```
conda install -c conda-forge rdkit vina 
```

- Install from github

```bash
pip install git+https://github.com/mkatouda/meekovina.git
```

- Local install

```bash
git clone https://github.com/mkatouda/meekovina.git
cd meekovina
pip install .
```

## Command usage

```
usage: meekovina [-h] [-i INP] [-l LIGAND] [--input_smiles INPUT_SMILES] 
                 [-r RECEPTOR] [-lr REFLIGAND] [-o OUT] [-od OUTDIR]
                 [-cx CENTER_X] [-cy CENTER_Y] [-cz CENTER_Z]
                 [-sx SIZE_X] [-sy SIZE_Y] [-sz SIZE_Z]
                 [-c CPU] [--scoring SCORING] [--seed SEED]
                 [--exhaustiveness EXHAUSTIVENESS] [--max_evals MAX_EVALS]
                 [--num_modes NUM_MODES] [--min_rmsd MIN_RMSD]
                 [--energy_range ENERGY_RANGE] [--spacing SPACING]
                 [--verbosity VERBOSITY] [--score_only] [--local_only]
                 [--exec EXEC] [--bin_path BIN_PATH] [--boxauto]
                 [--gybox_ratio GYBOX_RATIO] [-v]

optional arguments:
  -h, --help            show this help message and exit
  -i INP, --inp INP     yaml style input file, overwriting argument values (default: None)
  -l LIGAND, --ligand LIGAND
                        ligand (PDBQT, MOL, SDF, MOL2, PDB) (default: None)
  --input_smiles INPUT_SMILES
                        SMILES string (Need to put the atom you want to extend at the end of the string) (default: None)
  -r RECEPTOR, --receptor RECEPTOR
                        rigid part of the receptor (PDBQT) (default: None)
  -lr REFLIGAND, --refligand REFLIGAND
                        reference ligand (PDBQT, MOL, SDF, MOL2, PDB) to determine the box center (default: None)
  -o OUT, --out OUT     output models (PDBQT), the default is chosen based on the ligand file name (default: None)
  -od OUTDIR, --outdir OUTDIR
                        make output directry if argument is specified (default: None)
  -cx CENTER_X, --center_x CENTER_X
                        X coordinate of the center (default: None)
  -cy CENTER_Y, --center_y CENTER_Y
                        Y coordinate of the center (default: None)
  -cz CENTER_Z, --center_z CENTER_Z
                        Z coordinate of the center (default: None)
  -sx SIZE_X, --size_x SIZE_X
                        size in the X dimension (Angstroms) (default: 22.5)
  -sy SIZE_Y, --size_y SIZE_Y
                        size in the Y dimension (Angstroms) (default: 22.5)
  -sz SIZE_Z, --size_z SIZE_Z
                        size in the Z dimension (Angstroms) (default: 22.5)
  -c CPU, --cpu CPU     the number of CPUs to use (the default is to try todetect the number of CPUs or, failing that, use 1) (default: 4)
  --scoring SCORING     force field name: vina(default), ad4, vinardo (default: vina)
  --seed SEED           explicit random seed (default: 0)
  --exhaustiveness EXHAUSTIVENESS
                        exhaustiveness of the global search(roughly proportional to time): 1+ (default: 8)
  --max_evals MAX_EVALS
                        number of evaluations in each MC run (if zero,which is the default, the number of MC steps isbased on heuristics) (default: 0)
  --num_modes NUM_MODES
                        maximum number of binding modes to generate (default: 9)
  --min_rmsd MIN_RMSD   minimum RMSD between output poses (default: 1)
  --energy_range ENERGY_RANGE
                        maximum energy difference between the best bindingmode and the worst one displayed (kcal/mol) (default: 3)
  --spacing SPACING     grid spacing (Angstrom) (default: 0.375)
  --verbosity VERBOSITY
                        verbosity (0=no output, 1=normal, 2=verbose) (default: 1)
  --score_only          evaluate the energy of the current pose or poses without strucutre optimization (default: False)
  --local_only          evaluate the energy of the current pose or poses with local structure optimization (default: False)
  --exec EXEC           select AutoDock-Vina executer (default: bin)
  --bin_path BIN_PATH   AutoDock-Vina binary path (default: vina)
  --boxauto             enable automatic box determination algorithm (default: False)
  --gybox_ratio GYBOX_RATIO
                        scaling factor of radius of gyration to determine of docking box with automatic box determination algorithm (default: 2.5)
  -v, --verbose         verbose mode (default: False)
```

## Examples

Input files of example jobs are loated at "meekovina/example/input". 
You can example jobs using job script files "meekovina_com.sh" and "meekovina_yml.sh" located at "meekovina/example".

### Exmaples of command line usage

- Ligand input from file

```
meekovina -l LIGAND -r RECEPTOR -o OUTPUT -cx CENTER_X -cy CENTER_Y -cz CENTER_Z
```

- Ligand input from SMILES

```
meekovina --input_smiles INPUT_SMILES -r RECEPTOR -o OUTPUT -cx CENTER_X -cy CENTER_Y -cz CENTER_Z
```

### Exmaples of yaml input usage

Prepare input yaml file input.yml:

```
ligand: './inputs/1iep_ligand.mol'
refligand: './inputs/1iep_ligand.mol'
receptor: './inputs/1iep_receptorH.pdbqt'
out: '1iep_ligand'
outdir: 'outputs'
size_x: 20.0
size_y: 20.0
size_z: 20.0
cpu: 2
exhaustiveness: 3
num_modes: 9
seed: 1234
```

Then, run meekivina in command line:

```
meekovina -i input.yml
```

Keywards of yaml file are the same in the name of command line options.  
See above explanation of command line options.  

## Author

- Michio Katouda (katouda@rist.or.jp)
