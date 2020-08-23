# MLflow Projects&mdash;Making Code Reproducible
Having found a way to track code versions and parameters, we now want to go one step further and enable others to use our code on different machines or even run the code at scale on another platform, thus, making our results reproducible. This usually means that the whole environment (e.g., library dependencies) needs to be captured.

MLflow Projects are a standard format for packaging reusable data science code. Each project is simply a directory with code or a Git repository, and uses a descriptor file or simply convention to specify its dependencies and how to run the code.

A broad documentation on how to set up MLflow Projects can be found on [MLflow's own documentation](https://www.mlflow.org/docs/latest/projects.html#). Here, again, a shortened [overview](#overview) is given. Additionally, a [template folder](200_mlflow_project_template) provides basic files to create an MLflow project. Hands-on tasks will be given [afterwards](#tasks).

## Overview
### How to package your code
At the core, MLflow Projects are just a convention for organizing and describing your code to let other data scientists (or automated tools) run it. Each project is simply a directory of files, or a Git repository, containing your code. MLflow can run projects based on a convention for placing files in this directory (e.g., a `conda.yaml` file is treated as a Conda environment), but you can describe your project in more detail by adding a `MLproject` file, which is a YAML formatted text file. Each project can specify several properties:
* Name:  A human-readable name for the project.
* Entry Points: Commands that can be run within the project, and information about their parameters. Most projects contain at least one entry point that you want other users to call. Some projects can also contain more than one entry point: e.g., you might have a single Git repository containing multiple featurization algorithms. You can also call any `.py` or `.sh` file in the project as an entry point. If you list your entry points in an `MLproject` file, however, you can also specify parameters for them, including data types and default values.
* Environment: The software environment that should be used to execute project entry points. This includes all library dependencies required by the project code.

Having used `conda` to create a virtual environment makes creating the `conda.yaml` file quite easy. Just execute these commands:
```bash
conda activate <YOUR_CONDA_ENVIRONMENT>
conda env export -f conda.yaml
```

See the files in the [template folder](./200_mlflow_project_template) for more details or consult MLflow's documentation.

### Commands
You can run any project from a Git URI or from a local directory using the `mlflow run` CLI tool, or the `mlflow.projects.run()` Python API. For more information on how to call the CLI command, consult
```bash
mlflow run --help
```

## Tasks
1. Take a look at the ["Hello World" Project folder](./210_hello_world) and its specific implementation. Run the project with the local folder as well as with the remote GitHub folder.
    ```bash
    set MLFLOW_TRACKING_URI=http://localhost:5000
    conda activate mlflow_sklearn
    mlflow run 20_projects\210_hello_world -P alpha=.01 -P run_origin=LocalRun -P log_artifact=True
    mlflow run https://github.com/MynherVanKoek/AMLD_2020_MLflow.git#20_projects/210_hello_world -P alpha=.01 -P run_origin=GitRun -P log_artifact=True
    ```
    Linux or Mac users use this command at the top:
    ```bash
    export MLFLOW_TRACKING_URI=http://localhost:5000
    ...
    ```
    Users who do not use `conda` but `venv` to create virtual environments need to run the respective commands as shown at the start of the workshop. 

2. Complete the [Logistic Regression](./221_sklearn_logreg) and [Wine Classification](./231_sklearn_elasticnet_wine) folders and run them as well. You can also use their respective solution folders to them as remote GitHub projects.