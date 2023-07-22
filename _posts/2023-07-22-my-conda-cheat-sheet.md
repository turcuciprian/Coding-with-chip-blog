---
title: "my-conda-cheat-sheet"
date: 2023-07-22
---

For those of you starting off with using Conda as your main python package manager, especially for ML, and for myself to not forget things or to have them easily available, I am creating this page with a bunch of useful comands that can be considered my personal conda cheet sheet


### Create an environment:

When you want to create a brand new conda enviroment from scratch, that you want to use and install packages in to use them in your code implementations.

```
conda create --name <environment_name>
```

### Activate an environment

When you want one of your environmants to become the active environment you are working in, with it's installed packages for you to use.

```
conda activate <environment_name>
```

### Install dependency / ML library

When you want to install a specific package with anaconda

```
conda install <libary_name>
```

### Install channel dependency / ML library multiple packages

Allthough you can install one or more packages with the simple install comand, some packages are under a specific anaconda channel and must be installed witha  channel argument.

```
conda install -c <channel_name> <libary_name>
```

### Generate yml file

If you want to generate from all your installed packages a "install" file that you can use to install the exact packages, later

```
conda env export > environment.yml
```

### Create environment from yml file

Installing the packages set up in the "install" file called environment.yml.

```
conda env create -f environment.yml
```

### List Environments

List all the environments available for you to activate in conda

```
conda env list
```

### Using the env with jupyter notebook (by creating a jupyter kernel)

When you want to use the packages you just installed in your environment with jupyter notebook, you need to use ipykernel (that needs to be installed )
You can create a new jupyter notebook kernel like this:

```
python -m ipykernel install --user --name=your_kernel_name_here
```