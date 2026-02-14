# Betting_Football

**Betting_Football** is an end-to-end data pipeline designed to find a competitive edge against bookmakers. By scraping historical performance data and live market odds, the system calculates independent "true" probabilities and compares them against Unibet's implied odds to identify value opportunities.

---

## 📈 Project Workflow

The project is structured into four main phases:

### 1. Data Acquisition (Web Scraping)
* **Historical Results:** Scrapes comprehensive match history (scores, dates, home/away status) from ESPN for the "Big Five" European leagues and their second divisions (e.g., Premier League, Championship, LaLiga, Ligue 1).
* **Live Odds:** Automates the extraction of real-time betting markets from the Unibet website, specifically targeting Match Result (1X2) and Double Chance markets.
* **Team Mapping:** Utilizes a robust mapping system to synchronize team names between ESPN (statistical source) and Unibet (betting source), handling variations in spelling and sponsorships (e.g., "Ligue 1 McDonald's®").

### 2. Statistical Computation & Ratios
* **Recent Form Analysis:** Computes team-specific statistics based on a rolling window of the last games to capture current momentum.
* **Home/Away Weighting:** Calculates performance ratios specifically for Home and Away contexts to account for "Home Field Advantage".
* **Custom Weighting:** Features a dynamic results dictionary (e.g., `results_dict["0.5 0.5"]`) that allows for adjustable weights between home and away performance metrics.

### 3. Probability Modeling
* **True Probability (`MaProba`):** Generates an independent probability for a win, loss, or draw based on historical scoring trends and recent defensive/offensive ratios.
* **Difference Analysis (`DiffProba`):** Calculates the delta between the model's calculated probability and the bookmaker's implied probability.
* **Threshold Filtering:** Filters matches using specific probability limits and a `minDiffProba` threshold to isolate "Value Bets" where the model believes the outcome is more likely than the odds suggest.

### 4. Dashboard & Filtering
* **Multi-Day Forecasting:** A flexible `day_range` filter (e.g., `[0]` for today, `[0, 1]` for today and tomorrow) allows for focused daily analysis or weekend planning.
* **Visual Analytics:** Uses Pandas styling and background gradients to highlight high-probability value bets for quick decision-making.

---

## 🛠️ Technical Stack
* **Python:** Core logic and data processing.
* **Requests & JSON:** For high-speed data extraction via internal API endpoints.
* **BeautifulSoup & Selenium:** For parsing complex HTML structures and handling dynamic web content.
* **Pandas:** For data manipulation, merging statistics, and creating the final analytical tables.
* **Tqdm:** Progress tracking for large-scale historical scrapes.

---

## 🚀 How to Use
1.  **Update Teams:** Run the Team Mapping script to ensure all ESPN IDs for current league members are up to date.
2.  **Scrape History:** Execute the historical scraper to build the statistical database for the current season.
3.  **Run Analysis:** Define your `day_range` and probability thresholds in the main notebook.
4.  **Identify Value:** Review the styled DataFrame to find matches where `DiffProba` exceeds your risk tolerance.

---

## 📂 Repository Structure
* `Team_Mapping.ipynb`: Logic for capturing ESPN Team IDs and creating the `LinkTeam` database.
* `History_Scraper.py`: The engine that crawls ESPN for match results.
* `Unibet_Parser.py`: Script to clean and standardize live odds data.
* `Value_Finder.ipynb`: The final analytical layer where probabilities are compared and bets are flagged.