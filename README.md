# FTEC5660 Homework 1: Receipt Chain

Build a LangChain pipeline that reads every supermarket receipt in a folder
with the vision-capable DeepSeek Flash model and answers these two questions:

1. How much money did I spend in total for these bills?
2. How much would I have had to pay without the discount?

For this homework, **amount spent** means the final payment after the receipt's
rounding line. **Without the discount** means the sum of the original positive
item prices: add back every promotion, coupon, member, app, packaging-damage,
and percentage discount, but do not add back rounding.

## Student task

Only edit the two functions in `hw1.py` that contain `### YOUR CODE HERE`:

- `build_chain()` creates your LangChain chain.
- `answer_queries()` runs the chain on the receipt images and returns one final
  response for each question.

You may use prompt chaining, routing, parallel calls, reflection, or a
combination. Your final responses should each contain one HKD amount. Do not
hard-code filenames or public answers; grading uses unseen receipt folders.

## Setup and public test

```bash
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Put your DeepSeek key after `DEEPSEEK_API_KEY=` in `.env`, then run:

```bash
python3 hw1.py --image-folder public_test
```

The program creates `results.csv` in the current directory. Its columns are
`query`, `model_response`, and `correctness`. The public answers are in
`public_test/ground_truth.json`. The starter intentionally returns the dummy
response `please design your chain to answer these two queries.` so it runs
before you add any API code.

The required model is `deepseek-v4-flash-vision-exp`, the vision-capable
DeepSeek Flash model. JPEG, PNG, GIF, and WebP inputs are accepted by the
homework runner.


## Homework 1 solution: 
> to students: please fill your solution description here.
## Task 1: Receipt OCR & Sum Calculation
### Chain Workflow
1. Input: folder of receipt image paths
2. For each receipt image:
   - Encode local image into base64
   - Pass image and system prompt to deepseek-v4-flash-vision-exp model
   - Model extracts `paid_after_rounding` and `price_without_discount` for single receipt, output JSON
3. Use Python to sum up values across all receipts, instead of asking LLM to compute total
4. Format the total into HK$ string to answer the two queries
5. Export answers into results.csv

### Why this design
Instead of feeding all 7 receipts into the model at once, we process receipts one by one.
Vision model only needs to extract numbers from one receipt, which reduces the chance of missing discount lines.
Arithmetic summation is handled in Python code, guaranteeing accurate total.

## Task 2: Reflection
The rapid development of multimodal AI reshapes my understanding of FinTech.
Vision AI can extract structured data from unstructured documents like invoices and receipts, which automates traditional manual accounting work.
For finance practitioners, AI is not just a replacement but a tool to improve efficiency.
However, we still need to be cautious about hallucination. That is why I offload mathematical calculation to deterministic Python code rather than relying fully on LLM.
In the future, I plan to combine multimodal models with financial rule checks to build more robust financial automation systems.

