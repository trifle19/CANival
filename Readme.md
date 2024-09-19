# CANival: A multimodal approach to intrusion detection on the vehicle CAN bus

**CANival** is a multimodal learning framework1 that integrates both the time interval-based and the signal-based models
simultaneously to detect intrusions in the CAN bus. This integration allows the system to leverage the strengths of both
models to detect a wider range of attacks effectively. To demonstrate the effectiveness of the multimodal approach, we
use two models as components of the CANival. The first model is a novel model we designed ourselves named Time Interval
Likelihood (TIL) as the time interval-based approach, and the second model is a revised version
of [CANet](https://doi.org/10.1109/ACCESS.2020.2982544) as the signal-based approach for processing vehicle data stream
signals.

This repository provides the source code of **CANival**, which is accepted by Vehicular Communications. The paper is
available at [here](https://doi.org/10.1016/j.vehcom.2024.100845).

## Implementation

1. **CANival** is evaluated using two
   datasets: [X-CANIDS Dataset](http://ieee-dataport.org/open-access/x-canids-dataset-vehicle-signal-dataset)
   and [SynCAN](https://github.com/etas/SynCAN). Please download them from the linked websites.
2. It was tested on Linux and the deep learning model was built with Tensorflow.
3. ⚡️ Make sure you have enough time and computing resource because X-CANIDS Dataset is large (6.5 GB raw data, 60.6 GB
   after processing).

### Environmental Setup

Install mamba (or conda) if you do not have one. See [conda-forge](https://github.com/conda-forge/miniforge#install).

```shell
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-$(uname)-$(uname -m).sh
```

Make a virtual env and install libraries

```shell
mamba create -n canival
mamba activate canival
mamba install -y python=3.10 pandas numpy==1.26.4 matplotlib ipython scikit-learn jupyter tqdm pyarrow seaborn
pip install cantools pympler
```

TensorFlow for Linux with GPU

```shell
pip install tensorflow[and-cuda]==2.15.0rc0 --extra-index-url https://pypi.nvidia.com

# make-up env. variables see https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#macos-and-linux
cat << 'EOF' > $CONDA_PREFIX/etc/conda/activate.d/env_vars.sh
#!/bin/sh

export PATH="$PATH:$CONDA_PREFIX/lib/python3.10/site-packages/nvidia/cuda_nvcc/bin"
## UNCOMMENT BELOW IF NECESSARY
# export TF_CPP_MIN_LOG_LEVEL=3
EOF

cat << 'EOF' > $CONDA_PREFIX/etc/conda/deactivate.d/env_vars.sh
#!/bin/sh

export PATH=`echo $PATH | tr ":" "\n" | grep -v "cuda_nvcc" | tr "\n" ":"`
EOF
mkdir -p $CONDA_PREFIX/etc/conda/activate.d
mkdir -p $CONDA_PREFIX/etc/conda/deactivate.d
```

TensorFlow when you are using CPU

```shell
pip install tensorflow==2.15.0rc0
```

## Running the Program

Follow the steps below.

1. Place all data files of SynCAN in [Dataset/Syncan](Dataset/Syncan) directory.
2. There are two kinds of data in X-CANIDS Dataset, i.e., signal and raw. Place only the **raw** data files
   in [Dataset/X-CANIDS/raw](Dataset/X-CANIDS/raw) directory.
3. Run [Notebooks/Data-preparing/Format X-CANIDS Dataset like SynCAN.ipynb](Notebooks/Data-preparing/Format X-CANIDS Dataset like SynCAN.ipynb)
to preprocess the dataset before training the revised CANet.
4. Run [Notebooks/CANET/CANet Train.ipynb](Notebooks/CANET/CANet Train.ipynb) _for each dataset_ to train the revised
   CANet. You can skip this step and use the pretrained models [models/CANet](models/CANet).
5. Run [Notebooks/CANET/CANet Test.ipynb](Notebooks/CANET/CANet Test.ipynb) _for each dataset_ to save test results (
   reconstruction errors) of the revised CANet. Please run only the cells associated with the selected dataset; there is
   a markdown like "If X-CANIDS dataset" in the notebook.
6. Run [Notebooks/TIL/Interval Likelihood.ipynb](Notebooks/TIL/Interval Likelihood.ipynb) _for each dataset_ to save
   test results (z values) of TIL.
7. Run [Notebooks/Plotting/Plotting results.ipynb](Notebooks/Plotting/Plotting results.ipynb) to see the evaluation
   results.

## Citation

If you found this source code helpful, please cite the following paper.

**Text:**

Hyunjae Kang, Thanh Vo, Huy Kang Kim, Jin B. Hong. "CANival: A multimodal approach to intrusion detection on the vehicle
CAN bus." _Vehicular Communications_ 50 (2024): 100845. https://doi.org/10.1016/j.vehcom.2024.100845.

**BibTex:**

```text
@article{KANG2024100845,
title = {{CANival: A multimodal approach to intrusion detection on the vehicle CAN bus}},
journal = {Vehicular Communications},
volume = {50},
pages = {100845},
year = {2024},
issn = {2214-2096},
doi = {https://doi.org/10.1016/j.vehcom.2024.100845},
url = {https://www.sciencedirect.com/science/article/pii/S2214209624001207},
author = {Hyunjae Kang and Thanh Vo and Huy Kang Kim and Jin B. Hong}
}
```