# 🧬 Setting up Conda + Mamba on a Bare Ubuntu Machine

This guide sets up a **lean, fast Conda environment** using **Miniforge** (open-source only) and the **Mamba solver** for high-speed package management.  

### 🎯 End Goals
- **Base environment:** extremely minimal — only core Conda/Mamba tooling  
- **`py-notebook` environment:** for Python notebook work (data viz, lightweight projects)  
- **`r-notebook` environment:** for R notebooks and exploratory work  
- For heavy projects (AI/ML pipelines, etc.), create new dedicated environments separately.

---

## 🪜 Step 1 — Install Miniforge (lightweight Conda + Mamba)

```bash
# Download and install Miniforge (open-source equivalent of Miniconda)
wget https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh -O ~/miniforge.sh
bash ~/miniforge.sh -b -p ~/miniconda3

# Add conda initialization to your shell
echo ". ~/miniconda3/etc/profile.d/conda.sh" >> ~/.bashrc

# Initialize Conda in bash and reload
~/miniconda3/bin/conda init bash
source ~/.bashrc
```

---

##⚙️ Step 2 — Set Mamba as the Default Solver

```bash
# Configure Conda to always use the faster libmamba solver
conda config --set solver libmamba

# Verify configuration
conda config --show solver
```

---

## 🐍 Step 3 — Create a Python Notebook Environment

> For Python-based Jupyter notebook work, lightweight data visualization, and analysis.  
> For larger ML or pipeline projects, create new dedicated environments separately.

```bash
mamba create -n py-notebook -c conda-forge python=3.12 jupyterlab ipykernel numpy pandas matplotlib
conda activate py-notebook
```

---

## 📊 Step 4 — Create an R Notebook Environment

> For R-based data exploration and Jupyter or VS Code integration.

```bash
mamba create -n r-notebook -c conda-forge r-base=4.5.1 jupyterlab r-irkernel radian
conda activate r-notebook
```

**Note** Radian is an awesome drop-in terminal replacement for R.

It can be installed separately in any active env using

```bash
pip install radian
```

### Optional: Install useful R libraries
Once inside R:

```r
# Recommended: install pak for easy R/CRAN/Bioconductor package management
install.packages("pak")

# Examples:
pak::pkg_install("ggplot2")
pak::pkg_install("bioc::GSVA")   # Bioconductor
pak::pkg_install("metap")        # CRAN
```

### If R kernel doesn't appear in Jupyter/VS Code
Run this inside an R session:

```r
install.packages("IRkernel")
IRkernel::installspec(user = TRUE)
```

## Step 5 — Open VSCode

Open VSCode from command line which will automatically install the VSCode server

```bash
code .
```

After launch you will need to install the Jupyter plugin for kernel support, then restart code.

Now activate desired environment, create a ipynb file, and start coding in either Python or R!

---

✅ **Done!**
You now have:
- A clean base system with open-source Conda/Mamba
- Two reproducible, isolated environments for Python and R work
- A clear pattern for creating future specialized environments (AI, pipelines, etc.)
