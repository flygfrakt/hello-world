# hello-world
There can be only one

The only one readme



%%bash
set -e

ROOT="$HOME/ai-cac-clean"

mkdir -p "$ROOT/conda_pkgs" "$ROOT/tmp" "$ROOT/models"

export CONDA_PKGS_DIRS="$ROOT/conda_pkgs"
export TMPDIR="$ROOT/tmp"

# Klona bara om repot inte redan finns
if [ ! -d "$ROOT/repo/.git" ]; then
    git clone https://github.com/Raffi-Hagopian/AI-CAC.git "$ROOT/repo"
fi

source "$(conda info --base)/etc/profile.d/conda.sh"

# Skapa miljön bara om den inte redan finns
if [ ! -d "$ROOT/env" ]; then
    conda create -p "$ROOT/env" -c pytorch -y \
        python=3.9 pip numpy=1.26.4 \
        pytorch=1.12.1 torchvision=0.13.1 \
        cudatoolkit=11.3 "mkl<2024.1"
fi

conda activate "$ROOT/env"
cd "$ROOT/repo"

sed -E '/^(torch|torchvision)==/d; /^--extra-index-url/d' \
    requirements.txt > requirements.home.txt

python -m pip install --no-cache-dir \
    -r requirements.home.txt \
    "torch==1.12.1" \
    "torchvision==0.13.1" \
    "numpy==1.26.4" \
    "ipywidgets==8.1.5" \
    "ipykernel==6.29.5"

python -m ipykernel install --user \
    --name ai-cac \
    --display-name "Python (AI-CAC)"
