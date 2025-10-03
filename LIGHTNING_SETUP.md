# Lightning Studio Quick Setup

## Step 1: Push to GitHub (Do this NOW on your Mac)

```bash
cd /Users/pranavnair/final_responisible

git init
git add .
git commit -m "Complete alignment pipeline"

# Create repo on GitHub, then:
git remote add origin https://github.com/YOUR_USERNAME/responsible-ai-alignment.git
git branch -M main
git push -u origin main
```

## Step 2: Clone in Lightning Studio (A100 GPU)

```bash
# In Lightning Studio terminal:
git clone https://github.com/YOUR_USERNAME/responsible-ai-alignment.git
cd responsible-ai-alignment

# Install dependencies (5-10 min)
pip install -r requirements.txt
```

## Step 3: BOOM - Run Everything

```bash
# Single command runs entire pipeline (3-4 hours)
./run_complete.sh
```

That's it! The script will:
1. ✅ Prepare data
2. ✅ Evaluate baseline (Detoxify + GPT-4o-mini judge)
3. ✅ Train QLoRA
4. ✅ Evaluate QLoRA
5. ✅ Train DPO
6. ✅ Evaluate DPO
7. ✅ Generate comparison charts (3 models)

## Monitor Progress

**Terminal 2:**
```bash
watch -n 1 nvidia-smi
```

**Browser:**
https://wandb.ai/pranavnairop090-misogi/Responsible-AI-Alignment-A100-please

## If Something Fails

The pipeline auto-resumes. Just run the same command again:
```bash
./run_complete.sh
```

It will skip completed steps automatically.

## Results

After 3-4 hours, check:
```bash
ls results/
# comparison_metrics.csv - Your main results table
# model_comparison.png - Charts comparing all 3 models

cat results/comparison_metrics.csv
```

Done! 🚀

