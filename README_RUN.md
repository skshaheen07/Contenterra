# Run
No dependencies (Python 3.10+, stdlib only); requirements.txt is intentionally empty.

Offline: `python main.py --payments data/challenge/payments.csv --shipments data/challenge/shipments.jsonl --complaints data/challenge/complaints.jsonl --extractions data/challenge/extractions.json --output result_offline.json`

Live: `export GEMINI_API_KEY=...` (or `GROQ_API_KEY`), then omit `--extractions`. Optional: `--provider gemini|groq`, `--model NAME`.

python main.py --payments data/smoke/payments.csv --shipments data/smoke/shipments.jsonl --complaints data/smoke/complaints.jsonl --extractions data/smoke/extractions.json --output smoke_result.json

python main.py --payments data/challenge/payments.csv --shipments data/challenge/shipments.jsonl --complaints data/challenge/complaints.jsonl --extractions data/challenge/extractions.json --output result_offline.json

python main.py --provider groq --model openai/gpt-oss-20b --payments data/sample/payments.csv --shipments data/sample/shipments.jsonl --complaints data/sample/complaints.jsonl --output result_live.json

python -c "import json; r=json.load(open('result_live.json')); print('llm_errors:', r['stats']['llm_errors']); [print(c['ticket_id'], c['order_id'], c['issue'], c['verdict'], c['evidence_ids']) for c in r['cases']]"

