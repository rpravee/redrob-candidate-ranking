# Redrob AI — Intelligent Candidate Ranking System
### India Runs Hackathon 2026 | Track 1 — Data & AI Challenge
 
## Problem Statement
Recruiters go through hundreds of profiles and still miss the right person — not because the talent isn't there, but because keyword filters can't see what actually matters. This system ranks candidates the way a great recruiter would: by understanding context, career history, and behavioral signals, not just matching keywords.
 
## My Approach
 
### Architecture
```
Job Description (job_description.docx)
        ↓
Honeypot Detection — removes fake/impossible profiles
        ↓
Weighted Text Profile Building
   (title ×3, headline ×2, expert skills ×2, recent job ×3)
        ↓
BAAI/bge-base-en-v1.5 (Sentence Transformer)
        ↓
Cosine Similarity → Semantic Score (50% weight)
        ↓
Career Scoring — experience fit, services-firm penalty,
location, GitHub activity (30% weight)
        ↓
Behavioral Scoring — exponential recency decay, response
rate, interview completion, notice period (20% weight)
        ↓
Hybrid Final Score → Ranked Top 100 → submission.csv
```
 
### Scoring Formula
```
final_score = (0.50 × semantic_score)
            + (0.30 × career_score)
            + (0.20 × behavioral_score)
```
 
## Key Features
- **Honeypot detection** — flags and removes candidates with impossible career timelines (e.g. tenure longer than the company has existed) or unrealistic "expert at everything" skill profiles, before they can pollute the ranking.
- **Weighted text profiles** — current title, headline, and most recent job are repeated in the embedding text so the model weights them more heavily than older or less relevant history.
- **Stronger embedding model** — `BAAI/bge-base-en-v1.5` instead of a lighter MiniLM model, for more accurate semantic matching between job description and candidate profile.
- **Career intelligence** — rewards 5–9 years experience (the JD's target range), penalizes a pure services-firm background (TCS, Infosys, Wipro, etc.), and rewards product-company experience, India location, and GitHub activity.
- **Behavioral signals with recency decay** — uses Redrob's signals (response rate, last active date, interview completion, notice period, open-to-work flag) with an exponential decay function so recently active candidates are valued far more than stale profiles.
- **Validated output** — every submission is checked for exactly 100 rows, no duplicate candidate IDs, correct rank ordering, and non-increasing scores before being finalized.
## Tech Stack
- Python 3
- `sentence-transformers` (BAAI/bge-base-en-v1.5)
- `scikit-learn` (cosine similarity)
- `pandas` / `numpy`
- `python-docx` (reading the job description file)
- Google Colab (T4 GPU)
## Files
| File | Description |
|------|-------------|
| `redrob_ai.ipynb` | Complete, executed notebook (13 cells) |
| `submission.csv` | Final ranked top 100 candidates |
| `README.md` | This file |
 
## How to Run
1. Open `redrob_ai.ipynb` in Google Colab.
2. Mount Google Drive and place the official dataset files in `/MyDrive/redrob_hackathon/`.
3. In the dataset-loading cell, set `MODE = 'full'` to run on all candidates (or `'sample'` to test quickly first).
4. Run all cells in order. The final ranked CSV is saved to `/MyDrive/redrob_hackathon/submission.csv`.
## Results
- **Dataset size:** 100,000 candidates processed
- **Honeypots removed:** 0 detected in this run (filter active and verified on known trap patterns)
- **Final ranked output:** Top 100 candidates, columns `candidate_id, rank, score, reasoning`
- **Score range (top 100):** 0.7888 to 0.8628
- **Validation:** All checks passed — 100 rows, no duplicate IDs, ranks 1–100, scores non-increasing
**Top 5 ranked candidates:**
 
| Rank | Title | Experience | Score |
|------|-------|------------|-------|
| 1 | Senior AI Engineer | 5.9 yrs | 0.8628 |
| 2 | Senior AI Engineer | 7.9 yrs | 0.8487 |
| 3 | Senior Machine Learning Engineer | 6.1 yrs | 0.8438 |
| 4 | Senior NLP Engineer | 7.8 yrs | 0.8406 |
| 5 | Senior Machine Learning Engineer | 7.2 yrs | 0.8340 |
 
Every candidate in the top 10 is a directly relevant AI/ML title, with India-based location and 5–9 years of experience matching the job description's target profile.
 
## About Me
- **Name:** Praveena Ravichandran
- **Education:** M.Sc. Data Analytics, Bharathiar University
- **Certification:** AWS Certified AI Practitioner (AIF-C01)
- **LinkedIn:** https://www.linkedin.com/in/praveena-ravichandran-21b702283/
- **GitHub:** https://github.com/rpravee
