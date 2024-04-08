# Python Environment Management

#### By Michele Collevati

---

This solution integrates Conda and Poetry tools for environment management, dependency resolution, and package distribution. It creates a unified and reproducible environment for Python projects.  

**Conda** excels in creating isolated environments and managing packages.  
**Poetry** simplifies package management and dependency resolution.


## Benefits of using Conda and Poetry together

1. **Isolation and reproducibility:**  
   - Conda allows you to create isolated environments that encapsulate dependencies, ensuring consistent and reproducible results.  
   - Poetry ensures accurate package versions, minimizing version conflicts.

2. **Dependency resolution:** Poetry's dependency resolution algorithm guarantees conflict-free packages, preventing the notorious *dependency hell* problem.

3. **Packaging and distribution:** both Conda and Poetry simplify packaging and distribution, making it easier to share projects with others.


## Setting up the environment (instructions for Linux)

### 1. Install Conda

These quick command line instructions will get you set up quickly with the latest **Miniconda** installer. These four commands quickly and quietly install the latest 64-bit version of the installer and then clean up after themselves. To install a different version or architecture of Miniconda for Linux, change the name of the `.sh` installer in the `wget` command.

```
$ mkdir -p ~/miniconda3
$ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
$ bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
$ rm -rf ~/miniconda3/miniconda.sh
```

After installing, initialize your newly-installed Miniconda. The following commands initialize for ```bash``` and ```zsh``` shells:

```
$ ~/miniconda3/bin/conda init bash
$ ~/miniconda3/bin/conda init zsh
```

See also [https://docs.conda.io/projects/miniconda/en/latest/](https://docs.conda.io/projects/miniconda/en/latest/) for more details.

### 2. Create the Conda virtual environment (locally in the project directory)

You can control where a Conda environment lives by providing a path to a target directory when creating the environment. For example, the following command will create a new environment, called `.envs`, with a specific version of Python in a subdirectory of the current working directory:

```
$ conda create --prefix ./.envs python=3.11
```

To activate the environment:

```
$ conda activate ./.envs
```

Specifying a path to a subdirectory of your project directory when creating an environment has the following benefits:  
- It makes it easy to tell if your project uses an isolated environment by including the environment as a subdirectory.
- It makes your project more self-contained as everything, including the required software, is contained in a single project directory.

An additional benefit of creating your project’s environment inside a subdirectory is that you can then use the same name for all your environments.  

See also [https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#specifying-a-location-for-an-environment](https://conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#specifying-a-location-for-an-environment) for more details.

### 3. Install Poetry in the Conda environment

```
$ conda install poetry
```

### 4. Initialize a pre-existing project using Poetry

Instead of creating a new project, Poetry can be used to *initialize* a pre-populated directory. To interactively create a `pyproject.toml` file in directory `pre-existing-project`:

```
$ cd pre-existing-project
$ poetry init
```

This command will help you create a `pyproject.toml` file interactively by prompting you to provide basic information about your package. It will interactively ask you to fill in the fields, while using some smart defaults.  

See also [https://python-poetry.org/docs/basic-usage/#initialising-a-pre-existing-project](https://python-poetry.org/docs/basic-usage/#initialising-a-pre-existing-project) for more details.

Fill in the Python field as follows:

```
Compatible Python versions [^3.11]:  ~3.11
```

See also [https://python-poetry.org/docs/dependency-specification/#tilde-requirements](https://python-poetry.org/docs/dependency-specification/#tilde-requirements) for more details.

### 5. Add dependencies

The `add` command adds required packages to your `pyproject.toml` and installs them. If you do not specify a version constraint, poetry will choose a suitable one based on the available package versions.

```
$ poetry add urllib3="^1.26.16" numpy pillow tensorflow
```

See also [https://python-poetry.org/docs/cli/#add](https://python-poetry.org/docs/cli/#add) for more details.

### 6. Lock dependencies

This command locks (without installing) the dependencies specified in `pyproject.toml` to ensure reproducibility:

```
$ poetry lock --no-update
```

See also [https://python-poetry.org/docs/cli/#lock](https://python-poetry.org/docs/cli/#lock) for more details.

### 7. Export Conda environment

Export the Conda environment to the `.yml` file:

```
$ conda env export | grep -E -v "(^prefix: |^name: )" > environment.yml
```


## Initialize the environment after cloning the project repo

```
$ conda env create -p ./.envs -f environment.yml
$ conda activate ./.envs
$ poetry install
```


## Additional notes

1. There is an error with Poetry's dependency on `urllib3` that shows the following error message:

    ```
    HTTPResponse.__init__() got an unexpected keyword argument 'strict'
    ```

    The dependency error can be resolved by downgrading the `urllib3` package:

    ```
    $ poetry add urllib3="^1.26.16"
    ```

    See also [https://stackoverflow.com/questions/76295497/poetry-add-pkg-got-httpresponse-init-got-an-unexpected-keyword-argument](https://stackoverflow.com/questions/76295497/poetry-add-pkg-got-httpresponse-init-got-an-unexpected-keyword-argument) for more details.


## References

1. [Conda and Poetry: A Harmonious Fusion](https://medium.com/@silvinohenriqueteixeiramalta/conda-and-poetry-a-harmonious-fusion-8116895b6380) (viewed online on Monday, 09/10/2023)
2. [Conda](https://docs.conda.io/en/latest/) (viewed online on Monday, 09/10/2023)
3. [Poetry](https://python-poetry.org/docs/) (viewed online on Monday, 09/10/2023)
