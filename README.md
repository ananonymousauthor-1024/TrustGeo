# TrustGeo
![](https://img.shields.io/badge/python-3.8.13-green)![](https://img.shields.io/badge/pytorch-1.12.1-green)![](https://img.shields.io/badge/cudatoolkit-11.6.0-green)![](https://img.shields.io/badge/cudnn-7.6.5-green)

This folder provides a reference implementation of **TrustGeo**, as described in the paper: "Trustworthy IP Geolocation with Dynamic Graph Uncertainty Awareness",  which is submitted to WWW 2023 for anonymous review.


## Basic Usage

### Requirements

The code was tested with `python 3.8.13`, `pytorch 1.12.1`,  `cudatoolkit 11.6.0`, and `cudnn 7.6.5`. Install the dependencies via [Anaconda](https://www.anaconda.com/):

```shell
# create virtual environment
conda create --name TrustGeo python=3.8.13

# activate environment
conda activate TrustGeo

# install pytorch & cudatoolkit
conda install pytorch torchvision torchaudio cudatoolkit=11.6 -c pytorch -c conda-forge
# install other requirements
conda install numpy pandas
pip install scikit-learn
```

### Run the code
```shell
### we run our model on the New York dataset as a demo.
# Open the "TrustGeo" folder
cd TrustGeo

# loda data from the region and execute IP clustering. 
python preprocess.py --dataset "New_York" 

# run the model TrustGeo
python main.py --dataset "New_York" --dim_in 30
```

## Folder Structure

```tex
└── TrustGeo
	├── datasets # contains demo dataset sampled from New_York dataset for street-level IP geolocation	
	│	|── New_York
	├── lib # contains model implementation files
	│	|── layers.py # The code of the attention mechanism.
	│	|── model.py # The core source code of proposed TrustGeo
	│	|── sublayers.py # The support file for layer.py
	│	|── utils.py # Auxiliary functions, including the code of view fusion
	├── preprocess.py # Load dataset and execute IP clustering the for model running
	├── main.py # Run model for training and test
	└── README.md
```

## Dataset Information

The "datasets" folder contains one subfolder corresponding to the demo dataset collected from New York. There are three files in the subfolder:

- data.csv    *# features and labels for street-level IP geolocation* 
- ip.csv    *# IP addresses*
- last_traceroute.csv    # last four routers and corresponding delays for efficient IP host clustering

**PS:** Since GitHub limits the size of files allowed in repositories (less than 50MB), we can't upload our complete datasets. And considering the double-blind request of WWW for paper reviewing, in this version, we add a small proportion of data, e.g., 6000 samples to fulfill our code architecture. 

The detailed **columns and description** of New_York dataset are as follows:

#### New York 

| Column Name                     | Data Description                                             |
| ------------------------------- | ------------------------------------------------------------ |
| ip                              | The IPv4 address                                             |
| as_mult_info                    | The ID of the autonomous system where IP locates             |
| country                         | The country where the IP locates                             |
| prov_cn_name                    | The state/province where the IP locates                      |
| city                            | The city where the IP locates                                |
| isp                             | The Internet Service Provider of the IP                      |
| vp900/901/..._ping_delay_time   | The ping delay from probing hosts "vp900/901/..." to the IP host |
| vp900/901/..._trace             | The traceroute list from probing hosts "vp900/901/..." to the IP host |
| vp900/901/..._tr_steps          | #steps of the traceroute from probing hosts "vp900/901/..." to the IP host |
| vp900/901/..._last_router_delay | The delay from the last router to the IP host in the traceroute list from probing hosts "vp900/901/..." |
| vp900/901/..._total_delay       | The total delay from probing hosts "vp900/901/..." to the IP host |
| longitude                       | The longitude of the IP (as label)                           |
| latitude                        | The latitude of the IP host (as label)                       |

