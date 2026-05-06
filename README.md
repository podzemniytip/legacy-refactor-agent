# legacy-refactor-agent

### 1. Scanner Agent (Claude Code)
- Traverses 50 microservice repos
- Builds dependency graph (~2000 files)
- **Tokens:** 10M / day

### 2. Planner Agent (DeepSeek)
- 50‑step long‑chain reasoning
- Clusters modules into independent batches
- **Tokens:** 20M / day

### 3. Refactor Agent (Cursor + Claude)
- Rewrites 100 modules/day at 200k tokens each
- Preserves external interfaces
- **Tokens:** 20M / day

### 4. Validator Agent (MiMo)
- Runs unit + integration tests
- Feeds errors back to Refactor agent (closed‑loop)
- **Tokens:** 5M / day

### 5. Merger Agent
- Creates 500+ PRs
- Auto‑merges after green CI

## Token Consumption (Full Run)

| Phase               | Tokens per day | Days | Total       |
|---------------------|----------------|------|-------------|
| Scanning            | 10M            | 5    | 50M         |
| Planning            | 20M            | 2    | 40M         |
| Refactoring         | 20M            | 15   | 300M        |
| Validation (loops)  | 15M            | 15   | 225M        |
| **Total**           | **~30M/day**   | 20   | **~615M**   |

## Proof of Usage

- [Billing screenshot (OpenRouter, last 30 days)](./proof/billing.png)
- [Terminal logs – scanner output](./proof/logs.txt)
- [Workflow recording (demo)](./proof/demo.mp4)

## Requirements

- Claude API key (Claude Code)
- DeepSeek API key (long‑chain reasoning)
- MiMo access (validation)
- Cursor with Claude integration

## Setup

```bash
git clone https://github.com/yourusername/legacy-refactor-agent
cd legacy-refactor-agent
pip install -r requirements.txt
export CLAUDE_API_KEY=...
export DEEPSEEK_API_KEY=...
python main.py --target ./legacy_repos --max-tokens 600000000
