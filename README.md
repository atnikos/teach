# TEACH: Temporal Action Compositions for 3D Humans [3DV-2022]
[![report](https://img.shields.io/badge/arxiv-report-red)](https://arxiv.org/abs/1912.05656)

## Official PyTorch implementation of the paper "TEACH: Temporal Action Compositions for 3D Humans" 

<p float="center">
  <img src="assets/action2.gif" width="49%" />
  <img src="assets/action3.gif" width="49%" />
</p>


Check our YouTube videos below for more details.

### Video 

<!-- | Paper Video                                                                                                | Qualitative Results                                                                                                |
|------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| [![PaperVideo](https://img.youtube.com/vi/rIr-nX63dUA/0.jpg)](https://www.youtube.com/watch?v=rIr-nX63dUA) | -->

## Features


This implementation:
- Instruction on how to prepare the datasets used in the experiments.
- The training code:
  - For both baselines
  - For TEACH method
- A simple interacting demo that given some prompts with texts and durations returns back:
  - a `npy` file containing the vertices of the body generated by TEACH.
  - a video that demonstrates the result.

## Updates

To be uploaded:
- Instructions about the baselines and how to run them.
- Instructions for sampling and evaluating with the code all of the models.
- The rendering code for the blender renderings used in the paper.

## Getting Started
TEACH has been implemented and tested on Ubuntu 20.04 with python >= 3.9.

Clone the repo:
```bash
git clone https://github.com/athn-nik/teach.git
```

After it do this to install DistillBERT:

```shell
cd deps/
git lfs install
git clone https://huggingface.co/distilbert-base-uncased
cd ..
```

Install the requirements using `virtualenv` :
```bash
# pip
source scripts/install_pip.sh
```
You can do something equivalent with `conda` as well.

## Running the Demo

We have prepared a nice demo code to run TEACH on arbitrary videos. 
First, you need download the required data(i.e our trained model from our [website](teach.is.tue.mpg.de)). 

Then, running the demo is as simple as:

```bash

# Run on your description
python interact_teach.py folder=/path/to/experiment output=/path/to/yourfname texts='[text prompt1, text prompt2, text prompt3, <more prompts comma divided>]' durs='[dur1, dur2, dur3, ...]'

```
## Data
Download the data from [AMASS website](amass.is.tue.mpg.de).

```shell
python divotion/dataset/process_amass.py --input-path /path/to/data --output-path path/of/choice --model-type smplh --use-betas --gender male
```

Download the data from [BABEL website](babel.is.tue.mpg.de)[GET IT FROM ME]:
The run this script and change your paths accordingly inside it extract the different babel splits from amass:

```shell
python scripts/amass_splits_babel.py
```

Then create a directory named `data` and put the babel data and the processed amass data in.
You should end up with a data folder with the structure like this:

```
data
|-- amass
|  `-- your-processed-amass-data 
|
|-- babel
|   `-- babel_v2.1
|-- smpl_models
|   `-- smpl
```

Be careful not to push any data! 
Then you should softlink inside this repo. To softlink your data, do:

`ln -s /path/to/data`

## Training
To start training after activating your environment. Do:

```shell
python train.py experiment=baseline logger=none
```

Explore `configs/train.yaml` to change some basic things like where you want
your output stored, which data you want to choose if you want to do a small
experiment on a subset of the data etc.
[TODO]: More on this coming soon.

### Sampling & Evaluation

Here are some commands if you want to sample from the validaiton set and evaluate on the metrics reported
in the paper:

```shell 
python sample_seq.py folder=/path/to/experiment align=full slerp_ws=8
```

In general the folder is: `folder_our/<project>/<dataname_config>/<experimet>/<run_id>`
This folder should contain a `checkpoints` directory with a `last.ckpt` file inside and a `.hydra` directory from which the configuration
will be pulled and the relevant checkpoint. This folder is created during training in the output directory and is provided in our website
for the experiments in the paper.

- `align=trans`: chooses if translation will be aligned or if the global orientation also(`align=full`)
- `slerp_ws`: decides on whether slerp is done or not(`=null`) and what is the size of its window.

Then for the evaluation you should do:

```shell
python eval.py folder=/path/to/experiment align=true slerp=true
```

the two extra parameters decide the samples on which the evaluation will be performed.

### Transition distance

- Without alignment column: ```shell 
python compute_td.py folder=/path/to/experiment align_full_bodies=false align_only_trans=true```

- With alignment column:
```shell python compute_td.py folder=/path/to/experiment align_full_bodies=true align_only_trans=false```

[TODO]: More on this coming soon.

## Citation

```bibtex
@inproceedings{TEACH:3DV:2022,
  title={TEACH: Temporal Action Compositions for 3D Humans},
  author={Athanasiou, Nikos and Petrovich, Mathis and Black, Michael J. and Varol, G\"{u}l },
  booktitle = {International Conference on 3D Vision (3DV)},
  month = {September},
  year = {2022}
}
```

## License
This code is available for **non-commercial scientific research purposes** as defined in the [LICENSE file](LICENSE). By downloading and using this code you agree to the terms in the [LICENSE](LICENSE). Third-party datasets and software are subject to their respective licenses.

## Acknowledgments
We thank [Benjamin Pellkofer](https://is.mpg.de/person/bpellkofer) for his IT support.

## References
Many part of this code were based on the official implementation of [TEMOS](https://github.com/Mathux/TEMOS). Here are some great resources we benefit:

- SMPL models and layer is from [SMPL-X model](https://github.com/vchoutas/smplx).
## Contact

This code repository was implemented mainly by [Nikos Athanasiou](https://is.mpg.de/~nathanasiou) with the help of [Mathis Petrovich](https://mathis.petrovich.fr/).

Give a ⭐  if you like.

For commercial licensing (and all related questions for business applications), please contact ps-licensing@tue.mpg.de.
