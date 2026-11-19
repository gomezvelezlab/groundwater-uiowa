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

## Compiling MODPATH 7

### 1. Clone the repository and enter the directory:

```bash
git clone https://github.com/MODFLOW-ORG/modpath-v7.git
```
Enter the new directory
```bash
cd modpath-v7
```

### 2. Compile with static linking
```bash
mfpymake source mp7 -mc -fc gfortran -ff="-O2 -static-libgfortran" -cf="-O2 -static-libgcc"
```

### 3. Set execution permissions and verify:
```bash
chmod 755 mp7
ldd mp7
```

```bash
./mp7
```

### Share with students

```bash 
mkdir /home/jgomezvelez/classdata/usgs_binaries
```

Copy the binary and grant permissions:

```bash 
chmod +x /home/jgomezvelez/classdata/usgs_binaries/mp7
```

## Compiling MT3D-USGS

### 1. Clone the repository and enter the directory:

```bash
git clone https://github.com/MODFLOW-ORG/mt3d-usgs.git
```
Enter the new directory
```bash
cd mt3d-usgs
```

### 2. Compile with static linking
```bash
mfpymake src mt3dusgs -mc -fc gfortran -ff="-O2 -static-libgfortran" -cf="-O2 -static-libgcc"
```

### 3. Set execution permissions and verify:
```bash
chmod 755 mt3dusgs
ldd mt3dusgs
```

```bash
./mt3dusgs
```

### Share with students

```bash 
mkdir /home/jgomezvelez/classdata/usgs_binaries
```

Copy the binary and grant permissions:

```bash 
chmod +x /home/jgomezvelez/classdata/usgs_binaries/mt3dusgs
```

## MODFLOW-NWT

### 1. Download and extract the source code
```bash
mkdir -p modflow-nwt && cd modflow-nwt

wget [https://water.usgs.gov/water-resources/software/MODFLOW-NWT/MODFLOW-NWT_1.3.0.zip](https://water.usgs.gov/water-resources/software/MODFLOW-NWT/MODFLOW-NWT_1.3.0.zip)
unzip MODFLOW-NWT_1.3.0.zip

cd MODFLOW-NWT
```

### 2. Install Build Tools: Prerequisite. Install Meson and Ninja in your active environment using Python's package manager:

```bash
conda activate env_gw_iowa
```

Check if Meson and Ninja are installed in the environment.

### 3. Configure the Build: Static Linking. Create a meson.build file and initialize the build directory with static linking flags so the binary runs cleanly in student sessions:


```bash
python3 -c '
import glob, os
files = sorted(glob.glob("src/*.[fF]*"))
exclude = {"src/Irestart.f", "src/gmg7_NWT.f", "src/gmg7.f", "src/r_vector.c", "src/solvers.c", "src/ccfd.c", "src/mf2k的原则.c"}
files = [f for f in files if f not in exclude and not os.path.basename(f).startswith("gmg")]
with open("meson.build", "w") as out:
    out.write("""project(\x27mfnwt\x27, \x27fortran\x27, \x27c\x27,
  version: \x271.3.0\x27,
  default_options: [\x27warning_level=0\x27]
)

fc = meson.get_compiler(\x27fortran\x27)
add_project_arguments(
  fc.get_supported_arguments([\x27-std=legacy\x27, \x27-fallow-argument-mismatch\x27, \x27-Wno-argument-mismatch\x27]),
  language: \x27fortran\x27
)

sources = files(\n""")
    for f in files:
        out.write(f"  \x27{f}\x27,\n")
    out.write(")\n\nexecutable(\x27mfnwt\x27, sources)\n")
'
meson setup builddir --reconfigure -Doptimization=2 -Dc_link_args="-static-libgcc" -Dfortran_link_args="-static-libgfortran"
ninja -C builddir
```


### 4. Verify Static Linking
Confirm the executable is not relying on dynamic Fortran or GCC libraries:

```bash
ldd builddir/mfnwt
```

### 5. Test the Binary
Run the executable to confirm the USGS MODFLOW-NWT version banner prints cleanly:

```bash
./builddir/mfnwt
```

### 6. Set Permissions and Share with Students on IDAS
Apply execution permissions and copy the binary into your shared usgs_binaries folder:

```bash
chmod 755 builddir/mfnwt
mkdir -p /home/jgomezvelez/classdata/usgs_binaries
cp builddir/mfnwt /home/jgomezvelez/classdata/usgs_binaries/
chmod +x /home/jgomezvelez/classdata/usgs_binaries/mfnwt

```