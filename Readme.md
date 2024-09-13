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

### Environmental Setup

Install mamba (or conda) if you do not have one. See [conda-forge](https://github.com/conda-forge/miniforge#install).

```shell
curl -L -O "https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-$(uname)-$(uname -m).sh"
bash Miniforge3-$(uname)-$(uname -m).sh
```

Make a virtual env and install libraries.

```shell
mamba create -n canival
mamba activate canival
mamba install -y python=3.10 pandas numpy matplotlib ipython scikit-learn jupyter tqdm pyarrow seaborn
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

## Citation

If you found this source code helpful, please cite the following paper.

**Text:**
```text
Hyunjae Kang, Thanh Vo, Huy Kang Kim, Jin B. Hong,
CANival: A multimodal approach to intrusion detection on the vehicle CAN bus,
Vehicular Communications,
2024,
100845,
ISSN 2214-2096,
https://doi.org/10.1016/j.vehcom.2024.100845.
```

**BibTex:**
```text
@article{KANG2024100845,
title = {{CANival}: A multimodal approach to intrusion detection on the vehicle {CAN} bus},
journal = {Vehicular Communications},
pages = {100845},
year = {2024},
issn = {2214-2096},
doi = {https://doi.org/10.1016/j.vehcom.2024.100845},
url = {https://www.sciencedirect.com/science/article/pii/S2214209624001207},
author = {Hyunjae Kang and Thanh Vo and Huy Kang Kim and Jin B. Hong}
}
```