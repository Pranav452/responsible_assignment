# Lightning Studio Fix

## You're in the wrong directory!

Run these commands in Lightning Studio:

```bash
# 1. Check where you are
pwd

# 2. List what folders you have
ls -la ~

# 3. Find your project
cd ~
find . -name "run_pipeline.py" -type f 2>/dev/null

# 4. Once you find it, cd to that directory
# For example, if it's in ~/final_responisible:
cd ~/final_responisible

# 5. Then run the pipeline
python run_pipeline.py --include_dpo
```

## OR: If you haven't cloned from GitHub yet:

```bash
# Navigate to home
cd ~

# Clone your repo (replace with YOUR actual GitHub URL)
git clone https://github.com/YOUR_USERNAME/YOUR_REPO.git

# Go into the cloned folder
cd YOUR_REPO

# Install dependencies
pip install -r requirements.txt

# Run pipeline
python run_pipeline.py --include_dpo
```

## Quick Check:

Your current working directory should contain:
- `run_pipeline.py`
- `src/` folder
- `configs/` folder
- `requirements.txt`

Run: `ls -la` to verify these files exist in your current directory.

