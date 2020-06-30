# AutoCD: Automated synthetic micobial Community Designer
Automated equation generation of candidate models fo synthetic microbial communities and performs model selection using ABC SMC for customisable objective population behaviour. 

AutoCD is written in C++ and Python 3.4+, data analysis scripts require R. This has been tested on Linux only.

## Requirements

#### Python dependencies:
matplotlib, sympy, seaborn, pandas, scipy, numpy, PyYAML.
See `requirements.txt` for details of versions used and tested with.

#### C++ dependencies:
g++ (tested with g++-8)
Boost >=  v1.67
OpenMP

Boost must be installed for the version of python being used to run AutoCD. Ensure boost has been correvtly added to paths. Run `build_test.sh` to test on a simple build example and identify issues with library flags and ensuring the correct Python includes. Changes should be made to `build_pop_modules.sh` which is used to the build necessarry Boost Python libarary for a newly generated model space.

#### R packages: ####
ggplot2, ggisoband, gridExtra, sm

## Usage
Experiments are run using run_AutoCD.py. The Model Space Generator, Model Selection and Data Analysis parts of AutoCD are called using optional 
config file arguments

#### Model Space Generator
The `--MSG_config` is an optional command-line argument is used to invoke the Model Space Generator and requires a config file as input.

Examples running Model Space Generator for two and three species model spaces:
`python3 run_AutoCD.py --MSG_config MSG_two_strain.yaml`
`python3 run_AutoCD.py --MSG_config MSG_three_strain.yaml`

`MSG_two_strain.yaml` and `MSG_three_strain.yaml` contain examples of how to specify a model space config file.

#### ABC Model Selection

The `--ABC_config` is an optional command-line argument used to invoke the model selection using ABC SMC or ABC rejection algorithms, requiring a config file as input.

`ABC_two_strain.yaml` and  `ABC_three_strain.yaml` contain examples of how to specify an ABC config file. `--exp_num` is used to differentiate betweet repeats 
of using the same input files. Each experiment is stored in a subdirectory, all subdirectories are used by data analysis scripts

Examples running ABC SMC model selection for two and three species model spaces:
`python3 run_AutoCD.py --ABC_config ABC_two_strain.yaml --exp_num 0`
`python3 run_AutoCD.py --ABC_config ABC_three_strain.yaml --exp_num 0`

#### Data Analysis

The `--DA_config` is an optional command-line argument used to invoke data analysis of a set of finished ABC experiments, requiring a config file as input.

Examples running data analysis for  two and three species model spaces:
`python3 run_AutoCD.py --DA_config DA_two_strain.yaml --exp_num 0`

## Example
Here is a typical bash script that first generates a model space, runs four repeats of ABC SMC model selection, then performs data analysis on these repeat experiments
```
# Set number of repeat experiments
n_repeats=4

# Run model space generator
python3 run_AutoCD.py --MSG_config MSG_two_strain.yaml

# Conduct n repeat model selection experiements
for i in $(seq 0 $n_repeats); do 
	python3 run_AutoCD.py --ABC_config ABC_two_strain.yaml --exp_num $i
	echo $i; 
done

# Run data analysis
python3 run_AutoCD.py --DA_config DA_two_species.yaml
```