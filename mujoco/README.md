# MuJoCo Tasks

## Training clean agents:

- Please run 
```
python mujoco_cql.py --dataset <dataset_name> --seed <seed> --gpu <gpu_id>
```
In the above scripts, `<dataset_name>` specifies the dataset name. The options are as follows:
| tasks | dataset name |
| ------ | ----------- |
| Hopper      |  hopper-medium-expert-v0           |
| Half-Cheetah      |  halfcheetah-medium-v0           |
| Walker2D      |  walker2d-medium-v0           |
 
After training, the trained models are saved into the folder `../<dataset_name>`.

## Training poisoned agents:

The hyper-parameters settings of offline RL algorithms are recorded in the folder './params'.

If you get the error:

```
gym.error.NameNotFound: Environment `halfcheetah-medium-expert` doesn't exist. Did you mean: `bullet-halfcheetah-medium-expert`?
```
please input the codes:

```
import d4rl.gym_mujoco 
```

For training:
```
python poisoned_mujoco_cql.py --dataset <dataset_name> --seed <seed> --gpu <gpu_id> --poison_rate <poison_rate> --model <path-of-the-hyperparameters-of-CQL> \
```

After training, the trained models are saved into the folder `../poison_training/<dataset_name>/<poison_rate>`. 

## Retraining poisoned agents:

The poisoned agents used for the retraining experiments are in the `../poison_training/<dataset_name>/<poison_rate>/` folder. The weights of the poisoned agents are named as model.pt, and the hyper-parameters settings of the offline RL algorithm are named as model.json.
```
python retrain_mujoco_cql.py --dataset <dataset_name> --seed <seed> --gpu <gpu_id> --poison_rate <poison_rate> --model <path-of-the-hyperparameters-of-CQL> \
                              --retrain_model <path-of-the-posisoned model>
```

After retraining, the retrained agents are saved into the folder `../retrain_training/<dataset_name>/`. 


## Evaluation:

Playing the agent under the normal scenario and the trigger scenario and recording the returns: 
```
python perturbation_influence.py
```

## Backdoor Detection:

First, please enter to './d3rlpy/models/torch/encoders.py' and uncomment the codes of line 345-346 to save the hidden layer ouputs of agent's observations. These outputs are saved in folder './detection/'

For activation clustering:
```
python bacdoor_detection.py --dataset <dataset_name> --seed <seed> --gpu <gpu_id> --poison_rate <poison_rate> \
```
Please edit the path of model parameters (e.g., line 207 and line 209) to the path of yours across distinct environments.

