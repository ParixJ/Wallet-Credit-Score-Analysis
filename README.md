# Aave V2 Wallet Credit Scoring System

## Objective
This project computes a credit score (0 to 1000) for each wallet address interacting with the Aave V2 protocol using raw transaction-level data. The goal is to assess trustworthiness and responsible financial behavior on-chain.

---

## Method Chosen

We use a **rule-based scoring algorithm** that assigns weights to engineered features based on financial logic. Each feature is normalized and contributes positively or negatively to the final credit score.

### Features Engineered:
- **repay_ratio** = Total repaid / Total borrowed  
- **redeem_ratio** = Total redeemed / Total deposited  
- **times_deposit** = Number of deposit actions  
- **times_redeemed** = Number of redeem actions  
- **times_repayed** = Number of repay actions  
- **liquidation_count** = Number of liquidations (penalized)

---

## Architecture

user_transactions.json
->
Transaction Parsing & Feature Extraction
->
Feature Normalization (StandardScaler)
->
Weighted Rule-Based Scoring Logic
->
Score Scaling to [0, 1000]
->
Output: credit_scores.json (wallet → score)


---

## Processing Flow

1. **Extract Features**: Parse the JSON input, aggregate transaction metrics.
2. **Normalize**: All features scaled to 0–1.
3. **Scoring Logic**:

score =
0.3 * repay_ratio +
0.2 * redeem_ratio +
0.2 * times_deposit +
0.1 * times_redeemed +
0.1 * times_repayed -
0.2 * liquidation_count

4. **Scale to Credit Score**: Linearly scale final score to 0–1000 range.
5. **Save Scores**: Export as `credit_scores.json`

---

## Files

- `main.ipynb` – Main scoring script
- `user_transactions.json` – Input sample file
- `credit_scores.json` – Output scores
- `analysis.md` – Score insights and distribution
