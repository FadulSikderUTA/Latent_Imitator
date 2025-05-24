# Documentation of Changes to `requirements.txt` for LIMI_tabular

This document outlines the modifications made to the original `_requirements_original.txt` to produce the finalized `LIMI_tabular/requirements.txt`. These changes were necessary to ensure a reproducible and smoother environment setup for the `LIMI_tabular` project.

## Summary of Key Changes:

1.  **`matplotlib` Version:**
    *   Original: `matplotlib==3.3.4`
    *   Finalized: `matplotlib==3.4.0`
    *   Reason: The original version conflicted with `copulas==0.7.0`, which requires `matplotlib>=3.4.0`. Version `3.4.0` satisfies this and remains compatible with other dependencies.

2.  **`mkl-*` Packages (Intel Math Kernel Library):**
    *   Original:
        *   `mkl-fft==1.3.0`
        *   `mkl-random==1.1.1`
        *   `mkl-service==2.3.0`
    *   Finalized:
        *   `mkl==2025.1.0` (This single package manages its components like fft, random, service)
        *   (Individual `mkl-fft`, `mkl-random`, `mkl-service` lines from `pip freeze` were commented out as they are covered by the main `mkl` package).
    *   Reason: The original `mkl-random==1.1.1` had a strict dependency on `numpy==1.19.5`, which conflicted with other packages like `copulas`, `ctgan`, and `deepecho` that required `numpy>=1.20.0`. Using the umbrella `mkl` package allows for better dependency resolution. The `pip freeze` output showed `mkl==2025.1.0`, `mkl-fft==1.3.1`, `mkl-random==1.2.2`, and `mkl-service==2.4.0` were installed, which are compatible with `numpy==1.21.6`.

3.  **`tempeh` Installation:**
    *   Original: `tempeh==0.1.12`
    *   Finalized: `git+https://github.com/microsoft/tempeh.git@v0.1.12#egg=tempeh` (which installs as `tempeh==0.1.11`)
    *   Reason: The specified version `0.1.12` was not found on PyPI for Python 3.7. Installation directly from the GitHub repository using the `v0.1.12` tag was successful. The `pip freeze` output confirmed `tempeh==0.1.11` was installed via this method.

4.  **PyTorch Packages (`torch`, `torchaudio`, `torchvision`):**
    *   Original:
        *   `torch==1.8.1+cu101`
        *   `torchaudio==0.8.1`
        *   `torchvision==0.9.1+cu101`
    *   Finalized: These lines are commented out in the `requirements.txt`, and a detailed comment explains the manual installation procedure using the `-f https://download.pytorch.org/whl/cu101/torch_stable.html` flag.
    *   Reason: `pip install -r requirements.txt` cannot handle the `+cu101` (or other CUDA/CPU specific builds) and the find-links flag (`-f`) directly from a requirements file line. These packages must be installed in a separate step with the correct flags to ensure the appropriate build is downloaded. The `pip freeze` output confirmed these versions were active.

5.  **`seaborn` Added:**
    *   Original: Not present.
    *   Finalized: `seaborn==0.12.2`
    *   Reason: The `train_dnn.py` script requires `seaborn` for plotting, but it was missing from the original requirements. It was installed manually during the setup process.

6.  **Other Intel and NVIDIA Libraries:**
    *   Several other packages related to Intel oneAPI (`dpcpp-cpp-rt`, `intel-cmplr-*`, `intel-opencl-rt`, `intel-openmp`, `intel-sycl-rt`, `tbb`, `tcmlib`, `umf`) and NVIDIA CUDA ( `nvidia-cublas-cu11`, `nvidia-cuda-nvrtc-cu11`, `nvidia-cuda-runtime-cu11`, `nvidia-cudnn-cu11`) appeared in the `pip freeze` output.
    *   These are generally dependencies of the main `mkl` and `torch` (with CUDA) packages, respectively.
    *   They have been commented out in the finalized `requirements.txt` to avoid potential redundancy or conflicts, as they should be managed by the installation of their parent packages (`mkl` and `torch`).

By addressing these points, the finalized `LIMI_tabular/requirements.txt` aims to provide a more reliable and user-friendly setup experience. The manual steps for `tempeh` (if not using the git URL directly in `requirements.txt`) and PyTorch are clearly documented within the file itself.