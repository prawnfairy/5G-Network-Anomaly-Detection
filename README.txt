================================================================================
 EXECUTION INSTRUCTIONS
 Deep Learning Model Analysis for Network Anomaly Detection (FYP)
================================================================================

This file lists the tools and libraries (with versions) required to run the
source code in this repository, along with links to download them.


--------------------------------------------------------------------------------
1. TOOLS / SOFTWARE REQUIRED
--------------------------------------------------------------------------------

 Tool                Version used        Download link
 -------------------------------------------------------------------------------
 Python               3.12.12            https://www.python.org/downloads/
 pip                  (bundled with      https://pip.pypa.io/en/stable/installation/
                       Python)
 Jupyter Notebook /   Latest             https://jupyter.org/install
 JupyterLab
 Git                  Latest             https://git-scm.com/downloads
 NVIDIA CUDA Toolkit  12.1 or newer      https://developer.nvidia.com/cuda-downloads
 (optional, for GPU   (12.8 used in
 acceleration)        original runs)
 NVIDIA GPU driver    Latest compatible  https://www.nvidia.com/Download/index.aspx
 (optional, for GPU   with installed
 acceleration)        CUDA version

 Notes:
 - The original notebooks were developed and executed on Kaggle Notebooks
   (https://www.kaggle.com/code) using an NVIDIA Tesla T4 GPU accelerator.
 - The notebooks also carry Google Colab (https://colab.research.google.com/)
   configuration metadata, so they can alternatively be run there without any
   local installation.
 - A GPU is strongly recommended. Training involves ~3.5 million sequences;
   CPU-only training will be significantly slower.
 - If no local GPU/CUDA setup is available, use Kaggle Notebooks or Google
   Colab (both provide free GPU access) instead of installing CUDA locally.


--------------------------------------------------------------------------------
2. PYTHON LIBRARIES REQUIRED
--------------------------------------------------------------------------------

 Library         Version used     Install command / Download link
 -------------------------------------------------------------------------------
 torch           2.10.0+cu128     https://pytorch.org/get-started/locally/
 torchvision     (matching torch) pip install torch torchvision torchaudio ^
 torchaudio      (matching torch)     --index-url https://download.pytorch.org/whl/cu121
                                   (GPU build, CUDA 12.1; adjust index-url per
                                    your CUDA version on the PyTorch site above)

                                   CPU-only alternative:
                                   pip install torch torchvision torchaudio

 torchinfo       Latest           https://pypi.org/project/torchinfo/
                                   pip install torchinfo

 pandas          Latest           https://pypi.org/project/pandas/
                                   pip install pandas

 numpy           Latest           https://pypi.org/project/numpy/
                                   pip install numpy

 matplotlib      Latest           https://pypi.org/project/matplotlib/
                                   pip install matplotlib

 seaborn         Latest           https://pypi.org/project/seaborn/
                                   pip install seaborn

 scikit-learn    Latest           https://pypi.org/project/scikit-learn/
                                   pip install scikit-learn

 Combined install (mirrors the install cells inside the notebooks):

     pip install pandas numpy matplotlib seaborn scikit-learn torchinfo
     pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121

 To pin the exact versions used in the original runs after installing:

     pip freeze > requirements.txt


--------------------------------------------------------------------------------
3. DATASET
--------------------------------------------------------------------------------

 Dataset:      5G-NIDD (5G Network Intrusion Detection Dataset)
 Source:       https://www.kaggle.com/datasets/humera11/5g-nidd-dataset/data
 File used:    5G_NIDD_Full.csv
 Placement:    Dataset/5G_NIDD_Full.csv (folder is included in the repo;
               the CSV itself must be downloaded separately — see README.md
               for step-by-step download instructions).


--------------------------------------------------------------------------------
4. STEPS TO EXECUTE THE SOURCE CODE
--------------------------------------------------------------------------------

 1. Install Python 3.12.x from the link above (if not already installed).

 2. Clone this repository:
        git clone https://github.com/prawnfairy/5G-Network-Anomaly-Detection.git
        cd 5G-Network-Anomaly-Detection

 3. (Recommended) Create and activate a virtual environment:
        python -m venv venv
        source venv/bin/activate        (On Windows: venv\Scripts\activate)

 4. Install the required libraries (see Section 2 above).

 5. Download the 5G-NIDD dataset and place it at Dataset/5G_NIDD_Full.csv
    (see README.md, section "Download the dataset", for detailed steps).

 6. Launch Jupyter:
        jupyter notebook

 7. Open and run all cells, top to bottom, in either notebook:
    - "Lightweight GRU (MGU + Additive Attention + CL) TimeSeries_Binary
       Model.ipynb"      -> binary classification (benign vs. attack)
    - "Lightweight GRU (MGU + Additive Attention + CL) TimeSeries_Multiclass
       Model.ipynb"      -> multiclass classification (attack type)

    Alternative: upload the notebook to Kaggle Notebooks or Google Colab,
    attach a GPU runtime, upload/attach the dataset, and run all cells there
    instead of installing everything locally.


--------------------------------------------------------------------------------
DISCLAIMER
--------------------------------------------------------------------------------
This project was developed solely for academic purposes as a Final Year
Project (FYP) submitted in partial fulfilment of the Bachelor of Computer
Science (Hons) Artificial Intelligence at the Faculty of Information Science
& Technology, Multimedia University. It is not intended for production or
commercial use.
