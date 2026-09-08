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


# Compiling USGS code (optional)

## MODFLOW 6

### 1. Clone the repository
```bash 
git clone --branch master https://github.com/MODFLOW-USGS/modflow6.git
```

### 2.Install Build Tools:Prerequisite.Install Meson and Ninja in your active environment using Python's package manager:

```bash 
pip install meson ninja
```

### 3.Configure the Build:Static Linking.Initialize the build directory and pass the static linking flags to ensure the resulting binary can be shared with your students' sessions without runtime errors:

```bash 
meson setup builddir -Doptimization=2 -Dc_link_args="-static-libgcc" -Dfortran_link_args="-static-libgfortran"
```

### 4. Compile the Binary

```bash 
ninja -C builddir
```

### 5. Verify the executable was created successfully and is not relying on dynamic Fortran libraries:

```bash 
ldd builddir/src/mf6
```

### 6. Test the binary to ensure it outputs the version banner correctly:

```bash 
./builddir/src/mf6 -v
```

### 7. Apply the proper permissions so your students can execute it in their own IDAS sessions:

```bash 
chmod 755 builddir/src/mf6
```

### Share with students

```bash 
mkdir /home/jgomezvelez/classdata/usgs_binaries
```

Copy the binary and grant permissions:

```bash 
chmod +x /home/jgomezvelez/classdata/usgs_binaries/mf6
```
