# #Leveraging Optimal Decision Trees with Logical Analysis of Data
The Source code for paper:

[**Learning Optimal Decision Trees with Logical Analysis of Data and its Integration into Random Forest**]


## A brief explanation for each folder
-**Optimal_DTs_with_LAD/**: This folder contains all source codes and packages necessary for the reproducible results.
1. `dataframeCP4IM`: folder contains all two-class binary datasets with extensions .csv used in our experiments.
2. `binarytree`: folder to store the solution file related to the information of DT(max) and DT(exact).
3. `Loandra`: folder contains all the necessary files and scripts to run the incomplete MaxSAT solver Loandra. We downloaded from MaxSAT evaluation 2020 via this link: https://maxsat-evaluations.github.io/2020/descriptions.html
4. `Minimal-Hitting-Set-Algorithms`: folder contains all the necessary scripts in C++ to execute the algorithms pMMCS and pRS and others (https://github.com/VeraLiconaResearchGroup/Minimal-Hitting-Set-Algorithms). Note that we slightly modified the algorithm to enumerate 10K MHSes specifically for learning optimal DTs.
5. `Minimal-Hitting-Set-Algorithms-100K`: the same as the previous folder but we modified to generate 100K by using 20 threads to speed up the generating process as well as to have the diversification in terms of the generated support sets.
6. `output_pmmcs`: folder that stores all the results of the binary datasets used in our experiments, each dataset has 3 different folders of depth d = {2, 3, 4}.
7. `scripts`: folder contains all the necessary packages to generate the entire experiments used in our paper which we will briefly describe in the following paragraph.
8. `data.py dt.py maxSATdt_specific.py options.py`: scripts, we took from Hu et al. (https://gepgitlab.laas.fr/hhu/maxsat-decision-trees) by making some adjustments to adapt them with our usage in our experiment settings.
9. `maxSATdt_specific_with_LAD_based.py`: scripts modified to integrate LAD into DT(exact) and DT(max) for running the experiments.
10. `run_LAD_based_technique_with_CART.py`: script to run the comparison between (CART without LAD) vs (CART with LAD)

## A brief explanation for scripts package
-**scripts/**: contains all the necessary packages that we will describe one by one following the alphabet order

1. `dtencoder`: folder taken from Hu et al. (https://gepgitlab.laas.fr/hhu/maxsat-decision-trees) for adapting with their methods of DT(exact) and DT(max).
2. `evaluation`: package contains all the evaluation pipelines to compare our method with the other SOTA methods including Dt(exact) and DT(max). We will describe how to run it in detail in the following section
3. `lad_based`: package contains the script to run our proposed LAD method before integrating it into Optimal DTs
4. `subsets_ranking`: package contains the script to calculate the proposed merit score of a subset (support set) for binary dataset
5. `utility`: package contains some useful scripts to help the scripts in the folder `evaluation`
6. `utils`: contains scripts adapted from (https://gepgitlab.laas.fr/hhu/maxsat-decision-trees)


## Prerequisites before running the experiments
Some necessary python packages needed to execute the codes:
First, we need to unzip all the folders inside **LAD_based_FS**.
In directory **Optimal_DTs_with_LAD/**, 

1. First, create a virtual environment. For example, we want to create a virtual env called venv with python3.9: **python3.9 -m venv venv**
2. Then, activate that environment using: **source venv/bin/activate**
3. Optional choice: **pip install --upgrade pip**
4. Next, please run: **pip install -r requirements.txt**

Before running our approach, first thing first, we need to build c++ script files in **Minimal-Hitting-Set-Algorithm/** and **Minimal-Hitting-Set-Algorithm-100K/** directories.

# In directories `Minimal-Hitting-Set-Algorithms/` and `Minimal-Hitting-Set-Algorithms-100K/`,

Please run command **make** in those directories one by one. (If you have multiple cores or processors, you can build in parallel by running **make -j** instead.)

# In directory: `Optimal_DTs_with_LAD/`

#### Note that for the entire experiments, we initialized a 10-fold cross-validation with 3 repetitions, resulting in a total of 30 iterations

#### Binary dataset information

Binary datasets valid string to run the experiments: `ad`, `anneal`, `audiology`, `australian-credit`, `breast-cancer`, `car`, `drilling`, `german-credit`, `heart-cleveland`, `hepatitis`, `hypothyroid`, `kr-vs-kp`, `lymph`, `pima`, `primary-tumor`, `soybean`, `tic-tac-toe`, `vote`, `wdbc`

First, for DT(exact) and LAD + DT(exact)

## To run DT(exact)

    python3 maxSATdt_specific.py -a MaxSATdtencoding --depth exact-depth --kfold 10 --nrepeat 3 --reduced 1 --seed 2024 --complete 0 --gtimeout 900 dataframeCP4IM/dataset_name.csv

## For example, we want to run DT(exact) using `hepatitis` dataset with depth=`2`

    python3 maxSATdt_specific.py -a MaxSATdtencoding --depth 2 --kfold 10 --nrepeat 3 --reduced 1 --seed 2024 --complete 0 --gtimeout 900 dataframeCP4IM/hepatitis.csv

## Similarly, to run LAD + DT(exact)

    python3 maxSATdt_specific_with_LAD_based.py -a MaxSATdtencoding --depth max-depth --kfold 10 --nrepeat 3 --reduced 1 --seed 2024 --complete 0 --gtimeout 900 dataframeCP4IM/dataset_name.csv

## For example, we want to run LAD + DT(exact) using `hepatitis` dataset with depth=`2`

    python3 maxSATdt_specific_with_LAD_based.py -a MaxSATdtencoding --depth 2 --kfold 10 --nrepeat 3 --reduced 1 --seed 2024 --complete 0 --gtimeout 900 dataframeCP4IM/hepatitis.csv

Second, for DT(max) and LAD + DT(max)

## To run DT(max)

    python3 maxSATdt_specific.py -a MaxSATdtencoding --max_depth max-depth --kfold 10 --nrepeat 3 --reduced 1 --seed 2024 --complete 0 --gtimeout 900 dataframeCP4IM/dataset_name.csv

## For example, we want to run DT(max) using `hepatitis` dataset with depth=`2`

    python3 maxSATdt_specific.py -a MaxSATdtencoding --max_depth 2 --kfold 10 --nrepeat 3 --reduced 1 --seed 2024 --complete 0 --gtimeout 900 dataframeCP4IM/hepatitis.csv

## Similarly, to run LAD + DT(max)

    python3 maxSATdt_specific_with_LAD_based.py -a MaxSATdtencoding --max_depth max-depth --kfold 10 --nrepeat 3 --reduced 1 --seed 2024 --complete 0 --gtimeout 900 dataframeCP4IM/dataset_name.csv

## For example, we want to run LAD + DT(max) using `hepatitis` dataset with depth=`2`

    python3 maxSATdt_specific_with_LAD_based.py -a MaxSATdtencoding --max_depth 2 --kfold 10 --nrepeat 3 --reduced 1 --seed 2024 --complete 0 --gtimeout 900 dataframeCP4IM/hepatitis.csv

### Please feel free to contact us for more details if you have any questions in the provided codes.
