# Discovery

## Design

I'm building an ML service that detects emotions. The motivation behind this project is to learn about distributed ML patterns, Ray, MLOps, and LLMs. I'm using Streamlit as a frontend framework, FastAPI for building APIs, MLFlow for experiment tracking, etc. The idea about the project came to me when I saw Apple adding this feature to their health app. We have to log our emotions in the app and we can then monitor them. So I'm building an app where we can just journalize our thoughts and the app will then predict what's on your mind, how's your mood, and then maybe can recommend things (games, videos, etc.).

## Setup

**_Clusters_**

Now to do distributed ml, I need to have a cluster/group of machines to effortlessly scale the workloads. I'm using Ray here. I'll create a cluster using Ray. In this cluster, one is a head node that manages the cluster and will be connected to worker nodes that will execute workloads. We can then implement auto-scaling based on our application's computing needs.

I'm going to create our cluster by defining a computing configuration and an environment.

**_Environment_**

I'm using a Mac and python 3.10 here. I'm using `pyenv` to switch between Python versions easily.

```bash
pyenv install 3.10.11 # install 
pyenv global 3.10.11 # set default
```

Once I have Python version, I can create a virtual environment to install the dependencies.

```bash
mkdir classification 
cd classification 
python3 -m venv venv # create virtual environment 
source venv/bin/activate
python3 -m pip install --upgrade pip setuptools wheel
```

**_Compute_**

I can define the compute configuration in the `cluster_compute.yaml`, which will specify the hardware dependencies that I'll need to execute workloads.

If you're using resources from the cloud computing platforms like AWS, you can define those configurations here such as region, instance_type, min_workers, max_workers, etc.

I'm doing this on my personal laptop and so my laptop will act as a cluster where one CPU will be the head node and some of the remaining CPU will be the worker nodes.

**_Workspace_**

I'm using VS Code

**_Git_**

I have created a repository in my GitHub account and cloned it

```bash
export GITHUB_USERNAME="aniket-mish"
git clone https://github.com/aniket-mish/discovery.git . 
git remote set-url origin https://github.com/$GITHUB_USERNAME/discovery.git 
git checkout -b dev 
export PYTHONPATH=$PYTHONPATH:$PWD
```

Now I'm going to install the Python packages that are required and add them to our `requirements.txt` file. You can find the requirements.txt here.

```bash
python3 -m pip install -r requirements.txt
```

I will also install and update the pre-commit hooks.

```bash
pre-commit install
pre-commit autoupdate
```

Now I can launch the jupyter notebook to develop our ML application

```bash
jupyter lab notebooks/emotions.ipynb
```

**_Ray_**

I'm using Ray to build the scalable ML application

```python
import ray

# Initialize Ray
if ray.is_initialized():
	ray.shutdown()
ray.init()
```

I can also view cluster resources

```python
ray.cluster_resources()
```

## Systems

<img width="1080" alt="image" src="https://github.com/aniket-mish/discovery/assets/71699313/a532af67-0fab-477b-91c2-200e27196b20">
CI/CD and automated ML pipeline

## Data

```python
from datasets import load_dataset

emotions_dataset = load_dataset("emotions", "simplified")
```

Prepare
Explore
Preprocess
Distributed Ingest

## Model

Train
Track
Tune
Evaluate
Serve

## Developing

Scripts
CLI

## Utilities

Logs
Docs
Pre-commit

## Testing

Code - pytest
Data - great expectations

## Reproducibility

DVC, MLFlow

## Production

Jobs - Ray, Grafana dashboards
CICD - GitHub actions
Monitoring - Grafana
Data engineering - Airbyte, Dbt cloud

## References

[1] [Made With ML](https://madewithml.com)

[2] [Demystifying the Process of Building a Ray Cluster](https://medium.com/sage-ai/demystifying-the-process-of-building-a-ray-cluster-110c67914a99)

[3] [Building a Modern Machine Learning Platform with Ray](https://medium.com/samsara-engineering/building-a-modern-machine-learning-platform-with-ray-eb0271f9cbcf)

[4] [Learning Ray - Flexible Distributed Python for Machine Learning](https://maxpumperla.com/learning_ray/)

[5] [Last Mile Data Processing with Ray](https://medium.com/pinterest-engineering/last-mile-data-processing-with-ray-629affbf34ff)

[6] [Distributed Machine Learning at Instacart](https://www.instacart.com/company/how-its-made/distributed-machine-learning-at-instacart/)
