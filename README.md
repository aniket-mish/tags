# e2e-mlops

## Design

I'm building an ML service that detects emotions. The motivation behind this project is to learn about distributed ML patterns, Ray, MLOps, and LLMs. I'm using Gradio as a frontend framework, FastAPI for building APIs, MLFlow for experiment tracking, etc.

## Setup

**_Clusters_**

Now to do distributed ml, I need to have a cluster/group of machines to effortlessly scale the workloads. I'm using Ray here. I'll create a cluster using Ray. In this cluster, one is a head node that manages the cluster and will be connected to worker nodes that will execute workloads. We can then implement auto-scaling based on our application's computing needs.

I'm going to create our cluster by defining a computing configuration and an environment.

**_Environment_**

I'm using a Mac and Python 3.10 here. I'm using `pyenv` to create the virtual environments and switch between Python versions easily.

```bash
pyenv install 3.10.11 # install 
pyenv global 3.10.11 # set default
```

Once the pyenv is installed, I can create a virtual environment to install the dependencies.

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

I'm using VS Code with GitHub Copilot 😆

**_Git_**

I have created a repository on GitHub

```bash
export GITHUB_USERNAME="aniket-mish"
git clone https://github.com/aniket-mish/discovery.git . 
git remote set-url origin https://github.com/$GITHUB_USERNAME/discovery.git 
git checkout -b dev 
export PYTHONPATH=$PYTHONPATH:$PWD
```

Next, I clone the repo and install the necessary packages using our `requirements.txt` file.

```bash
python3 -m pip install -r requirements.txt
```

I will also install and update the pre-commit hooks. I use pre-commit in every project I work on.

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

I will go through the basics about Ray when I work on the distributed systems part of the workflow.

## Systems

<img width="1080" alt="image" src="https://github.com/aniket-mish/discovery/assets/71699313/a532af67-0fab-477b-91c2-200e27196b20">
CI/CD and automated ML pipeline

## Data

### Data Ingestion

I'm downloading the dataset from huggingface. You can download a raw (~211K rows) or simplified (~54K rows) subset of the dataset.

```python
from datasets import load_dataset

hf_dataset = load_dataset("emotions", "simplified")
```

You can see that the dataset splitting is done for us. If you check out the `hf_dataset`, you'll see the splits and the number of rows each split contains.

```python
DatasetDict({
    train: Dataset({
        features: ['text', 'labels', 'id'],
        num_rows: 43410
    })
    validation: Dataset({
        features: ['text', 'labels', 'id'],
        num_rows: 5426
    })
    test: Dataset({
        features: ['text', 'labels', 'id'],
        num_rows: 5427
    })
})
```

To explore the dataset, you can convert the dataset to a pandas dataframe.

```python
hf_dataset.set_format("pandas")
train_df = hf_dataset["train"][:]
```

#### Data Distribution

### Distributed Ingestion 

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
