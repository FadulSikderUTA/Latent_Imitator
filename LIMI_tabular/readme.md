# LIMI for tabular data
The scripts are stored in the folder **expsh**. 

Getting Started as follows:

1. Create and activate a Python 3.7 virtual environment (e.g., using `conda` or `pyenv` with `virtualenv`).
2. Install dependencies:
   `pip install -r requirements.txt`

   **Important Notes on Dependencies:**
    *   **PyTorch (torch, torchvision, torchaudio):** The `requirements.txt` file lists the specific versions used. However, due to the need for CUDA-specific (or CPU-only) builds, these are commented out. You will need to install them manually *after* the command above, using a command appropriate for your system. For example, for CUDA 10.1:
        `pip install torch==1.8.1+cu101 torchvision==0.9.1+cu101 torchaudio==0.8.1 -f https://download.pytorch.org/whl/cu101/torch_stable.html`
        Refer to the comments in `requirements.txt` and the official PyTorch website for the correct command for your setup.
    *   **Tempeh:** The `requirements.txt` file attempts to install `tempeh` directly from GitHub. If this fails, you may need to install it manually using:
        `pip install git+https://github.com/microsoft/tempeh.git@v0.1.12#egg=tempeh`
    *   For a detailed explanation of changes made to the original requirements, please see `requirements_changes.md`.

2. train the tested models (the models are stored in `./exp/train_dnn/train/`)

    `bash train_dnn.sh`

3. train the gan model (the gans are stored in `./exp/gans/`)
    `bash train_gan.sh`

4. generate $\mathbf{Z}_{init}$ latent vector and corresponding instances (stored in `./exp/table/`)

    `bash generate.sh`

5. use the tested models to predict instances in 4. (stored in the folder of each model by default )

   `bash predict.sh`

6. train the surrogate boundary to imitate the real decision boundary (stored in `./exp/train_boundaries/`)

   `bash train_boundary.sh`

7. conduct the fairness testing to generate natural individual discriminatory instances. (stored in `./exp/main_fair/ours[_description]`)

   `bash main_fair.sh`

   And the instances are stored in  `global_samples.npy`