# Emoai is an AI system that detects emotions from the text

I'm building an ML service that detects emotions. The motivation behind this project is to learn about distributed ways of training and serving ML models, Ray framework, MLOps best practices, and LLMs 😜. I'm using Gradio as a frontend framework, FastAPI for building APIs, MLFlow for experiment tracking, etc. 

Understanding distributed systems is one of the most important skills that a developer needs. I have previously worked on a project [Distributed ML System](https://github.com/aniket-mish/distributed-ml-system) in which I used Kubernetes to train, tune and serve the AI workloads. 

## Setup

To do distributed ML, I need to have a cluster/group of machines to scale the workloads effortlessly. I'm using Ray here. I will have to create a cluster. In this cluster, one is a head node that manages the cluster and will be connected to worker nodes that will execute workloads. We can then implement auto-scaling based on our application's computing needs.

I'm going to create our cluster by defining a computing configuration and an environment.

I'm using a personal machine (Mac 💻) for this project but you can use any cloud platform. I'm using `pyenv` to create the virtual environments and switch between Python versions easily. To create a cluster on the cloud you'll need a yaml with all the configurations with a base image, env variables, etc.

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

I can define the compute configuration in the `cluster_compute.yaml`, which will specify the hardware dependencies I'll need to execute workloads.

If you're using resources from the cloud computing platforms like AWS, you can define those configurations here such as region, instance_type, min_workers, max_workers, etc.

I'm doing this on my laptop and so my laptop will act as a cluster where one CPU will be the head node and some of the remaining CPU will be the worker nodes.

I'm using VS Code. You can also develop your app in Jupyter Lab and then copy your code in the VS Code. I know Karpathy does this!

I have created a repository on GitHub. I clone it.

```bash
export GITHUB_USERNAME="aniket-mish"
git clone https://github.com/aniket-mish/e2e-mlops.git . 
git remote set-url origin https://github.com/$GITHUB_USERNAME/e2e-mlops.git 
git checkout -b dev 
export PYTHONPATH=$PYTHONPATH:$PWD
```

Next, I install the necessary packages using our `requirements.txt` file. I am just starting to develop this project so there are just the necessary packages (pandas, numpy, torch, transformers) required as dependencies.

```bash
python3 -m pip install -r requirements.txt
```

I am a huge fan of `pre-commit` and I always install it when I am setting up a project. This is my go-to way of ensuring I do not commit bad things.

```bash
pre-commit install
pre-commit autoupdate
```

Let's launch a jupyter notebook to start experimenting.

```bash
jupyter lab notebooks/emotions.ipynb
```

**_Ray_**

I'm using Ray to build the scalable ML application. Ray is a popular distributed computing framework. It can help with the scaling of very complex AI workloads like tuning, inference, model training, etc.

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

I'm downloading the dataset from huggingface.

```python
from datasets import load_dataset

hf_dataset = load_dataset("dair-ai/emotion")
```

### Splitting

We already have train, validation, and test splits. If you check out the `hf_dataset`, you'll see the splits and the number of rows each split contains.

```python
DatasetDict({
    train: Dataset({
        features: ['text', 'label'],
        num_rows: 16000
    })
    validation: Dataset({
        features: ['text', 'label'],
        num_rows: 2000
    })
    test: Dataset({
        features: ['text', 'label'],
        num_rows: 2000
    })
})
```

### Exploration

To explore the dataset, you can convert the dataset to a pandas dataframe.

```python
hf_dataset.set_format("pandas")
train_df = hf_dataset["train"][:]
```

**_Data Distribution_**

```python
# Most common emotions
all_labels = Counter(train_df.label)
all_labels.most_common()


[('joy', 5362),
 ('sadness', 4666),
 ('anger', 2159),
 ('fear', 1937),
 ('love', 1304),
 ('surprise', 572)]
```

**_Preprocessing_**

I am encoding our text labels into indices and vice versa. I am also using SentenceTransformers model to tokenize our text.

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
