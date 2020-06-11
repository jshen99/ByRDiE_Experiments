# Byzantine Resilient Decentralized Learning

This codebase includes the code used to implement **BRIDGE: Byzantine-resilient Decentralized Gradient Descent** and **ByRDiE: Byzantine-resilient Distributed Coordinate Descent for decentralized learning** in order to learn a linear multiclassifier on the MNIST dataset using a 1 layer neural network similar to the decentralized experiment found in 

[Adversary-resilient Distributed and Decentralized
Statistical Inference and Machine Learning](https://arxiv.org/abs/1908.08649)

Z. Yang, A. Gang and W. U. Bajwa, "Adversary-Resilient Distributed and Decentralized Statistical Inference and Machine Learning: An Overview of Recent Advances Under the Byzantine Threat Model," in IEEE Signal Processing Magazine, vol. 37, no. 3, pp. 146-159, May 2020, doi: 10.1109/MSP.2020.2973345.


For all these experiments we setup a decentralized architecture with 20 nodes and 2,000 training samples from the MNIST dataset allocated to each node. 
## Requirements and Dependencies
To reproduce the environment with necessary dependencies create a conda environment using the `environment.yml` provided.

```
conda env create -f environment.yml 
```

## Compute Power
All of our computational experiments were carried out on a Linux high-performance computing (HPC) cluster provided by Rutgers Office of Advanced Research Computing specifically all of our experiments were run on:

Lenovo NextScale nx360 servers

- 2 x 12-core Intel Xeon E5-2680 v3 "Haswell" processors
- 128 GB RAM
- 1 TB local scratch disk

However we only allocated 4GB of RAM and 1 CPU when submitting our jobs. 

For the BRIDGE experiments and all its variants it took about 100 seconds to finish running.

For the ByRDiE exepriments the runtime was about 2 days.

## Data
All MNIST data we used can be found in the `./data` folder the `./data/MNIST/raw` contains the raw MNIST data and we used the `./data/MNIST_read.py` to turn the data into numpy arrays and pickle the numpy array objects for use in the algorithms.

## BRIDGE

```
usage: dec_BRIDGE.py [-h] [-b BYZANTINE] [-gb GOBYZANTINE]
                     [-s {BRIDGE,Median,Krum,Bulyan}]
                     monte_trial

positional arguments:
  monte_trial           Specify which monte carlo trial to run

optional arguments:
  -h, --help            show this help message and exit
  -b BYZANTINE, --byzantine BYZANTINE
                        Number of Byzantine nodes to defend against, if none
                        defaults to 0
  -gb GOBYZANTINE, --goByzantine GOBYZANTINE
                        Boolean to indicate if the specified number of
                        Byzantine nodes actually send out faulty values
  -s {BRIDGE,Median,Krum,Bulyan}, --screening {BRIDGE,Median,Krum,Bulyan}
                        Screening method to use (BRIDGE,Median, Krum, Bulyan),
                        default no screening is done regular gradient descent
```

Example: BRIDGE defending against 2 Byzantine nodes with no faulty nodes.

`python dec_BRIDGE.py 0 -b=2 -s=BRIDGE`

With `b` faulty nodes

`python dec_BRIDGE.py 0 -b=2 -gb=True -s=BRIDGE`

Run 10 of those in parallel with `monte_trial` varying between 0-9 to run 10 independent trials of the experiment. 

# ByRDiE
```
usage: dec_ByRDiE.py [-h] [-b BYZANTINE] [-gb GOBYZANTINE] monte_trial

positional arguments:
  monte_trial           Specify which monte carlo trial to run

optional arguments:
  -h, --help            show this help message and exit
  -b BYZANTINE, --byzantine BYZANTINE
                        Number of Byzantine nodes to defend against, if none
                        defaults to 0
  -gb GOBYZANTINE, --goByzantine GOBYZANTINE
                        Boolean to indicate if the specified number of
                        Byzantine nodes actually send out faulty values
```
Example: ByRDiE defending against 2 Byzantine nodes with no faulty nodes
`python dec_ByRDiE.py 0 -b=2`

with `b` faulty nodes

`python dec_ByRDiE.py 0 -b=2 -gb=True`

Run 10 of those in parallel with `monte_trial` varying between 0-9 to run 10 independent trials of the experiment. 

# Plotting

All results get saved in `./result` folder, you run 10 independent trials you can then run the `plot.py` script to plot accuracy vs. number of scalar broadcasts.

The `plot.py` script can be easily modified to run only 1 trial of any of the algorithms instead.

**Note:** Running this code will not perfeclty reproduce the results obtained in the paper mentioned above, rather this provides an implementation based on the original codebase.

# Contributors
The original implementation was provided by the author of the paper:
[Zhixiong Yang](https://www.linkedin.com/in/zhixiong-yang-67139152/)

The publicization and distribution of the code was made possible by:
[Joseph Shenouda](https://github.com/joeshenouda)