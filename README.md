# Responsible AI Alignment Pipeline

Complete implementation of LLM fine-tuning for safety and alignment using QLoRA and DPO on Llama-3-8B.

**Evaluation Metrics:**
- ✅ Toxicity Detection (Detoxify BERT model)
- ✅ Helpfulness & Harmlessness (GPT-4o-mini LLM Judge)
- ✅ 3-Model Comparison (Baseline → QLoRA → DPO) with charts

## Project Structure

```
final_responisible/
├── .env                          # Environment variables (API keys)
├── requirements.txt              # Python dependencies
├── run_pipeline.py              # Main orchestration script
├── configs/
│   ├── training_config.yaml     # QLoRA configuration
│   └── dpo_config.yaml          # DPO configuration
├── src/
│   ├── data/
│   │   └── prepare_data.py      # Evaluation dataset preparation
│   ├── training/
│   │   ├── train_qlora.py       # QLoRA training script
│   │   └── train_dpo.py         # DPO training script
│   ├── evaluation/
│   │   ├── evaluate_model.py    # Model evaluation script
│   │   └── compare_results.py   # Results comparison
│   └── utils/
│       └── chat_templates.py    # Chat format templates
├── data/                        # Evaluation datasets
├── models/                      # Saved model adapters
└── results/                     # Evaluation results & visualizations
```

## Lightning Studio Deployment (A100 GPU)

### Method 1: GitHub (Recommended)

```bash
# 1. Push this code to GitHub (from your local machine)
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/yourusername/responsible-ai-alignment.git
git push -u origin main

# 2. In Lightning Studio with A100:
git clone https://github.com/yourusername/responsible-ai-alignment.git
cd responsible-ai-alignment

# 3. Install dependencies (5-10 minutes)
pip install -r requirements.txt

# 4. Your .env file is already configured with API keys
# Just verify: cat .env

# 5. Run complete pipeline
python run_pipeline.py --include_dpo
```

### Method 2: Direct Upload (Alternative)

1. Upload entire `final_responisible` folder to Lightning Studio
2. Open terminal in Lightning Studio
3. Navigate to folder: `cd final_responisible`
4. Install: `pip install -r requirements.txt`
5. Run: `python run_pipeline.py --include_dpo`

## Running the Pipeline

### Quick Run (Single Command)

```bash
# Easiest way - runs everything automatically
./run_complete.sh

# OR using Python
python run_pipeline.py --include_dpo
```

**Expected Timeline on A100:**
- Data prep: 1 min
- Baseline eval: 30-40 min
- QLoRA training: 20-30 min  
- QLoRA eval: 30-40 min
- DPO training: 40-50 min
- DPO eval: 30-40 min
- Comparison: 1 min
- **Total: 3-4 hours**

## Monitor Progress

```bash
# Terminal 1: Run pipeline
./run_complete.sh

# Terminal 2: Monitor GPU
watch -n 1 nvidia-smi

# Browser: W&B Dashboard
# https://wandb.ai/pranavnairop090-misogi/Responsible-AI-Alignment-A100-please
```

## Configuration (Optimized for A100 40GB)

### Memory-Safe Settings
- **Max sequence length**: 1024 (prevents OOM)
- **Batch size**: QLoRA=2, DPO=1
- **Gradient accumulation**: QLoRA=8, DPO=16
- **Gradient clipping**: 1.0 (prevents NaN loss)
- **Precision**: BF16 (better than FP16)

### Training Details
- **QLoRA**: 10k samples, 2 epochs, ~25 min
- **DPO**: 5k pairs, 1 epoch, ~45 min
- **LoRA rank**: 16 (balanced performance/memory)

## Results Location

After completion, find results in:

```
results/
├── evaluation_baseline_100samples.csv    # Baseline scores
├── evaluation_qlora_100samples.csv       # QLoRA scores  
├── evaluation_dpo_100samples.csv         # DPO scores
├── comparison_metrics.csv                # Summary table
└── model_comparison.png                  # Comparison charts

models/
├── llama3-qlora-adapter/                 # QLoRA weights
└── llama3-dpo-adapter/                   # DPO weights
```

**Evaluation Metrics (from your old setup):**
- ✅ **Toxicity** (Detoxify BERT): 0-1 scale, lower = better
- ✅ **Helpfulness** (GPT-4o-mini judge): 1-5 scale, higher = better  
- ✅ **Harmlessness** (GPT-4o-mini judge): 1-5 scale, higher = better

## Resource Requirements

### Minimum (QLoRA only)
- GPU: 1x A100 40GB or equivalent
- RAM: 32GB
- Storage: 50GB
- Time: ~2-3 hours

### Full Pipeline (QLoRA + DPO)
- GPU: 1x A100 40GB or equivalent
- RAM: 32GB
- Storage: 80GB
- Time: ~4-5 hours

### Estimated Costs
- Compute (Lightning AI/Modal/Runpod): $5-10
- OpenAI API (GPT-4o-mini judge): $2-5
- **Total**: ~$10-15

## Troubleshooting

### Out of Memory (OOM)
- Reduce `per_device_train_batch_size` in configs
- Increase `gradient_accumulation_steps` to maintain effective batch size

### Slow Evaluation
- Reduce `num_samples` in evaluation scripts
- Use fewer evaluation examples (50 instead of 100)

### API Rate Limits
- Add delays between judge API calls
- Use cheaper judge model (gpt-3.5-turbo)

### W&B Sync Issues
- Check `WANDB_API_KEY` in `.env`
- Run `wandb login` manually
- Use `--skip_wandb` flag if needed

## Assignment Deliverables

This pipeline provides all required deliverables:

- ✅ Training scripts with hyperparameter configs
- ✅ Side-by-side comparison of outputs (qualitative)
- ✅ Quantitative metrics showing improvement
- ✅ Analysis of safety-utility trade-offs
- ✅ Resource utilization report (W&B logs)

### Bonus Points Implemented
- ✅ DPO (Direct Preference Optimization)
- ✅ Compare multiple PEFT methods (QLoRA vs DPO)
- ✅ Inference optimization (4-bit quantization, adapter merging)

## References

- [Unsloth](https://github.com/unslothai/unsloth) - Efficient LLM fine-tuning
- [Anthropic HH-RLHF](https://huggingface.co/datasets/Anthropic/hh-rlhf) - Alignment dataset
- [DPO Paper](https://arxiv.org/abs/2305.18290) - Direct Preference Optimization
- [LoRA Paper](https://arxiv.org/abs/2106.09685) - Low-Rank Adaptation

## License

MIT License - See LICENSE file for details

