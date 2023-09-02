# Discovery

## Design
I'm building an ML service that discovers biomedical content and categorizes it. The motivation behind this side project is to learn about distributed ML patterns, MLOps, and LLMs.

1. Setup

**Clusters**

Now to do distributed ml, I need to have a cluster/group of machines so that I can effortlessly scale the workloads. I'm using Ray here. I'll create a cluster using Ray. In this cluster, one of them is a head node that manages the cluster and it will be connected to worker nodes that will execute workloads. We can then implement auto-scaling based on our application's computing needs.

I'm going to create our cluster by defining a computing configuration and an environment.

**Environment**

I'm using a mac and python 3.10 here. I'm using `pyenv` to easily switch between python versions.

```
pyenv install 3.10.11 # install 
pyenv global 3.10.11 # set default
```

Once I have python version, I can create virtual environment to install the dependencies.

```
mkdir classification 
cd classification 
python3 -m venv venv # create virtual environment 
source venv/bin/activate
python3 -m pip install --upgrade pip setuptools wheel
```

**Compute**

I can define the compute configuration in the `cluster_compute.yaml`, which will specify the hardware dependencies that I'll need to execute workloads.

If you're using resources from the cloud computing platforms like AWS, you can define those configurations here such as region, instance_type, min_workers, max_workers, etc.

I'm doing this on my personal laptop and so my laptop will act as a cluster where one CPU will be the head node and some of the remaining CPU will be the worker nodes.

**Workspace**

I'm using VS Code

**Git**

I have created a repository in my GitHub account and cloned it

```
export GITHUB_USERNAME="aniket-mish"
git clone https://github.com/aniket-mish/emotions.git . 
git remote set-url origin https://github.com/$GITHUB_USERNAME/emotions.git 
git checkout -b dev 
export PYTHONPATH=$PYTHONPATH:$PWD
```

Now I'm going to install python packages that are required and add it to our `requirements.txt` file. You can find the requirements.txt here.

```
python3 -m pip install -r requirements.txt
```

Now I can launch the jupyter notebook to develop our ML application

```
jupyter lab notebooks/emotions.ipynb
```

**Ray**

I'm using Ray to scale and productionize our ML application

```
import ray

# Initialize Ray
if ray.is_initialized():
	ray.shutdown()
ray.init()
```

I can also view cluster resources

```
ray.cluster_resources()
```

2. Product

**Product Design**

3. Systems

## Data

1. Preparation

2. Exploration

3. Preprocessing

4. Distributed

## Model

## Developing

## Utilities

## Testing

## Reproducibility

## Production

## References
