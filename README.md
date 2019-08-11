# Generative Modeling by Estimating Gradients of the Data Distribution

This repo contains the official implementation for the paper 
[Generative Modeling by Estimating Gradients of the Data Distribution](https://arxiv.org/abs/1907.05600),

by __Yang Song__ and __Stefano Ermon__. Stanford AI Lab.

-------------------------------------------------------------------------------------
We describe a new method of generative modeling based on estimating the derivative of the log density 
function (_a.k.a._, Stein score) of the data distribution. We first perturb our training data by different Gaussian noise with progressively smaller variances. Next, we estimate the score function for each perturbed data distribution, by training a shared neural network named the _Noise Conditional Score Network (NCSN)_ using _score matching_. We can directly produce samples from our NSCN with _annealed Langevin dynamics_.

## Dependencies

* PyTorch

* PyYAML

* tqdm

* pillow

* tensorboardX

* seaborn


## Running Experiments

### Project Structure

`main.py` is the common gateway to all experiments. Type `python main.py --help` to get its usage description.

```
usage: main.py [-h] [--runner RUNNER] [--config CONFIG] [--seed SEED]
               [--run RUN] [--doc DOC] [--comment COMMENT] [--verbose VERBOSE]
               [--test] [--resume_training] [-o IMAGE_FOLDER]

optional arguments:
  -h, --help            show this help message and exit
  --runner RUNNER       The runner to execute
  --config CONFIG       Path to the config file
  --seed SEED           Random seed
  --run RUN             Path for saving running related data.
  --doc DOC             A string for documentation purpose
  --verbose VERBOSE     Verbose level: info | debug | warning | critical
  --test                Whether to test the model
  --resume_training     Whether to resume training
  -o IMAGE_FOLDER, --image_folder IMAGE_FOLDER
                        The directory of image outputs
```

There are four runner classes.

* `AnnealRunner` The main runner class for experiments related to NCSN and annealed Langevin dynamics.
* `BaselineRunner` Compared to `AnnealRunner`, this one does not anneal the noise. Instead, it uses a single fixed noise variance.
* `ScoreNetRunner` This is the runner class for reproducing the experiment of Figure 1 (Middle, Right)
* `ToyRunner` This is the runner class for reproducing the experiment of Figure 2 and Figure 3.

Configuration files are stored in  `configs/`. For example, the configuration file of `AnnealRunner` is `configs/anneal.yml`. Log files are commonly stored in `run/logs/doc_name`, and tensorboard files are in `run/tensorboard/doc_name`. Here `doc_name` is the value fed to option `--doc`.

### Training

The usage of `main.py` is quite self-evident. For example, we can train an NCSN by running

```bash
python main.py --runner AnnealRunner --config anneal.yml --doc cifar10
```

Then the model will be trained according to the configuration files in `configs/anneal.yml`. The log files will be stored in `run/logs/cifar10`, and the tensorboard logs are in `run/tensorboard/cifar10`.

### Sampling

Suppose the log files are stored in `run/logs/cifar10`. We can produce samples to folder `samples` by running

```bash
python main.py --runner AnnealRunner --test -o samples
```

## Checkpoints

We provide pretrained checkpoints [run.zip](https://drive.google.com/file/d/1BF2mwFv5IRCGaQbEWTbLlAOWEkNzMe5O/view?usp=sharing). Extract the file to the root folder. You should be able to produce samples like the following using this checkpoint.

| Dataset | Sampling procedure |
| :------------ | :-------------------------: |
| MNIST |  ![MNIST](assets/mnist_large.gif)|
| CelebA |  ![Celeba](assets/celeba_large.gif)|
|CIFAR-10 |  ![CIFAR10](assets/cifar10_large.gif)|



## References

Large parts of the code are derived from [this Github repo](https://github.com/ermongroup/sliced_score_matching) (the official implementation of the [sliced score matching paper](https://arxiv.org/abs/1905.07088))

If you find the code / idea inspiring for your research, please consider citing the following

```
@article{song2019generative,
  title={Generative Modeling by Estimating Gradients of the Data Distribution},
  author={Song, Yang and Ermon, Stefano},
  journal={arXiv preprint arXiv:1907.05600},
  year={2019}
}
```

and / or

```
@article{song2019sliced,
  title={Sliced Score Matching: A Scalable Approach to Density and Score Estimation},
  author={Song, Yang and Garg, Sahaj and Shi, Jiaxin and Ermon, Stefano},
  journal={arXiv preprint arXiv:1905.07088},
  year={2019}
}
```

