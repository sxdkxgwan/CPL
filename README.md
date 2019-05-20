# CPL: Collaborative Policy Learning

This is the code for paper "Collaborative Policy Learning for Open Knowledge Graph Reasoning".

## Running

You can use the following command to run this code:

```
CUDA_VISIBLE_DEVICES=0,1 python3 joint_trainer.py \
            --total_iterations=400 \
            --use_replay_memory=1 \
            --train_pcnn=1 \
            --bfs_iteration=200 \
            --use_joint_model=1 \
            --pcnn_dataset_base="data" \
            --pcnn_dataset_name="FB60K+NYT10/text" \
            --pcnn_max_epoch=250 \
            --base_output_dir="output" \
            --gfaw_dataset_base="data" \
            --gfaw_dataset="FB60K+NYT10/kg" \
            --load_model=1 \
            --load_pcnn_model=1 \
            --batch_size=64 \
            --hidden_size 100 --embedding_size 100 \
            --random_seed=55 \
            --eval_every=100 \
            --model_load_dir="experiments/FB60K+NYT10/kg/3_100_100_0.01_83_True_200_400_02130056/model"

```

usable configurations:

    total_iteations: total iterations for the model to run

    use_replay_memory: usage of replay memory or not

    train_pcnn: train pcnn in the process of joint reasoning or use a static model

    bfs_iteration: first how many iterations are for BFS path search

    use_joint_model: usage of joint model or not

    pcnn_dataset_base: base folder containing all relation extraction corpuses

    pcnn_dataset_name: which to use

    base_output_dir: what directory to save output to

    gfaw_dataset_base: base folder containing all KG data

    gfaw_dataset: which to use 

    load_model: whether to load a previously trained model (MINERVA) first

    load_pcnn_model: whether to load a previously trained PCNN model first

    batch_size, hidden_size, embedding_size: evidently

    random_seed: fix a random seed. RL methods heavily fluctuate in sircumstances

    eval_every: enter evaluation session (use validation set) for every what epochs

    model_load_dir: directory where the model will be loaded


You can find results in the experiments folder's scores.txt, under each model labeling its parameters.

Results will be saved to a folder named "experiments" as a subdirectory. Another folder "_preprocessed_data" will save the preprocessed datasets for quicker code running. 

## Datasets

We offer two datasets: FB15K-NYT10 and UMLS-PubMed. Download it here: 

https://drive.google.com/file/d/1hCyPBjywpMuShRJCPKRjc7n2vHpxfetg/view?usp=sharing

if you want to create your own dataset, it should like this:

the knowledge graph dataset should look like ones for [MINERVA](https://github.com/shehzaadzd/MINERVA). take a look: [Here](https://github.com/shehzaadzd/MINERVA/tree/master/datasets/data_preprocessed/FB15K-237)

the corpus should look like ones for [OpenNRE](https://github.com/thunlp/OpenNRE). An example is below:

```
[
    {
        'sentence': 'Bill Gates is the founder of Microsoft .',
        'head': {'word': 'Bill Gates', 'id': 'm.03_3d', ...(other information)},
        'tail': {'word': 'Microsoft', 'id': 'm.07dfk', ...(other information)},
        'relation': 'founder'
    },
    ...
]
```

## File Structure

/nrekit: the Relation Extraction Agent kit.

/rl_code: the reasoner.

/model/trainer.py: training code.

/model/tester.py: testing code.

/joint_trainer.py: main running code. loads both models and runs sessions.

/bfs_example.cpp: a BFS widget for searching paths across the knowledge graph.

## Cite this paper