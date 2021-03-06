## gtsrb.torch

This repo illustrates how to use [Torch](http://torch.ch/) to train a
convolutional neural network on the [GTSRB](http://benchmark.ini.rub.de/?section=gtsrb&subsection=news)
dataset (German Traffic Sign Recognition Benchmark) and how to improve the
state-of-the-art with a [Spatial Transformer](http://arxiv.org/abs/1506.02025) layer.

### Requirements

* install Torch (see [this guide](http://torch.ch/docs/getting-started.html))
* (optional) install the [Spatial Transformer](https://github.com/qassemoquab/stnbhwd)
module:

```bash
git clone https://github.com/qassemoquab/stnbhwd.git
cd stnbhwd && luarocks make stnbhwd-scm-1.rockspec
```

CUDA is not mandatory unless you use the Spatial Transformer (see below).

### Usage

To train a network with all default options:

```bash
luajit main.lua
```

Use `luajit main.lua --help` to see all the available options.

In particular use the `--no_cuda` option to work in CPU mode.

### Results

| Method | Accuracy |
| --- | --- |
| two spatial transformers with idsia-like network (1) | **99.67** |
| single spatial transformer with idsia-like network (2) | 99.46 |
| winner of original contest: [idsia network](http://people.idsia.ch/~juergen/nn2012traffic.pdf) | **99.46** |
| single idsia-like network (3) | 98.85 |
| human performances ([corresponding paper](http://image.diku.dk/igel/paper/MvCBMLAfTSR.pdf)) | 98.84 |

(1) `luajit main.lua -n -1 --st --net idsia_net.lua --cnn 150,200,300,350 --locnet 200,300,200 --locnet3 150,150,150 -e 26`

(2) `luajit main.lua -n -1 --net idsia_net.lua --cnn 200,250,350,400 --st --locnet 200,300,200`

(3) `luajit main.lua -n -1 --net idsia_net.lua --cnn 150,200,300,350`

### Code description

#### gtsrb module

The `gtsrb` module is a wrapper around all other modules.

It contains:

* `gtsrb.dataset` the [data loader](docs/dataset.md).
* `gtsrb.networks` the [network builder](docs/networks.md).
* `gtsrb.trainer` the [trainer](docs/trainer.md).

#### Benchmark module and scripts

To be able to test as much configurations as possible, we use some automatic
benchmarking scripts.

See [here](docs/bench.md) for details.

#### Plotting script

This script allows to have an idea of the visual impact of the spatial
transformer when it is placed at the input of the network.

Use `qlua plot.lua --help` to see the available options.

This script is meant to be used with networks generated by the `main.lua` script.
