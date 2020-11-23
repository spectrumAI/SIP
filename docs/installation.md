# Installation guide:

**Step 1: Install anaconda.** Install conda for python 3.7: https://docs.anaconda.com/anaconda/install/linux/. For Winows, please go to https://www.anaconda.com/distribution/. 

**Step 2: Create python 3.6.10 environment.** Create a conda environment 'py36' that runs on python version 3.6.10. 
```
conda create -n py36 python=3.6
```

**Step 3: Activate 'py36':**

```
conda activate py36
```

**Step 4: Install packages.** 

1. Install pytorch: Go to https://pytorch.org/, and generate the command based on your OS and CUDA version, copy the command to run it. 

2. Install the the other packages:

```
conda install pandas
conda install -c anaconda pyqt
conda install -c conda-forge/label/TEST gdal=3.1.3
conda install -c anaconda scikit-image
conda install -c anaconda scikit-learn
conda install pyyaml

conda install git
```

**Step 5: go to your home folder, and run**

```
cd you_home_folder
git clone https://github.com/spectrumAI/SIP.git
```

cd to the folder 'SIP' to run the app:
```
cd SIP
python SIP.py
```

**Step 6: Get a license to run.** Please email spectrumaitech@gmail.com with your name and organization to get a free license. Put the license.lic under the pytransform folder and replace the old one. 


