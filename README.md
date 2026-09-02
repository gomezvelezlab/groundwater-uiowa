# groundwater-uiowa
Repository for the groundwater course at the University of Iowa

# Step 1: Create a conda environment for the class

Open a terminal wondow and run the following command:

```bash
conda env create -f env_gw_iowa.yml
```

# Step 2: Activate the new conda environment for the class

In the terminal wondow, run the following command:

```bash
conda activate env_gw_iowa
```

# Step 3: Make the environment available as a JupyterLab kernel

In the terminal wondow, run the following command:

```bash
python -m ipykernel install --user --name env_gw_iowa --display-name "Python (groundwater class)"
```

# Step 4: Make USGS bianries executable

In the terminal wondow, run the following command:

```bash
chmod +rx ./usgs_code/*
```