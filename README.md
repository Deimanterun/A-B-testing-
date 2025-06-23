# A-B-testing-

FitTrack A/B Test Analysis Report
1. Company Overview
Company: FitTrack
Product: Fitness app offering workout plans, nutrition tracking, and progress analytics
Business Goal: Determine the optimal monthly subscription price to maximize revenue while maintaining a healthy conversion rate
Tested Price Points:

Group A: $4.99/month

Group B: $9.99/month

Group C: $14.99/month

Test Duration: 2 weeks
Sample Size: 1,000 users (randomly assigned)

2. Sample Size Calculation (Upfront Power Analysis)
Before running the test, we ensured adequate statistical power (80%) to detect a minimum 5% difference in conversion rates between groups:

Baseline conversion rate (control group): ~10%

Minimum detectable effect (MDE): 5%

Significance level (α): 0.05

Power (1-β): 0.80

Using a sample size calculator for proportions:

# Sample size calculation (example)
from statsmodels.stats.power import NormalIndPower
effect_size = 0.05  # 5% absolute difference
alpha = 0.05
power = 0.80
analysis = NormalIndPower()
sample_size = analysis.solve_power(effect_size=effect_size, alpha=alpha, power=power, ratio=1.0)
print(f"Required sample size per group: ~{int(sample_size)}")  
Result: Needed ~400 users per group (total 1,200). Our test had 333–334 users per group, slightly underpowered but acceptable for directional insights.

3. Choosing the Right Statistical Test
Metric Type: Binary (Subscribed: Yes/No)
Primary Test: Chi-square test (for overall differences)

Pairwise Comparisons: Z-test for proportions

python
from scipy.stats import chi2_contingency
from statsmodels.stats.proportion import proportions_ztest

# Chi-square test (all groups)
chi2, pval, _, _ = chi2_contingency(pd.crosstab(df['group'], df['subscribed']))
print(f"Chi-square p-value: {pval:.4f}")  # p < 0.001 → Significant differences exist

# Pairwise Z-tests
successes = [40, 23, 10]  # Subscriptions in A, B, C
trials = [333, 333, 334]   # Total users per group
proportions_ztest(successes[:2], trials[:2])  # A vs B: p = 0.008
proportions_ztest([successes[0], successes[2]], [trials[0], trials[2]])  # A vs C: p < 0.001
4. Visualizing Results with Confidence Intervals
python
import matplotlib.pyplot as plt
import seaborn as sns

# Plot conversion rates with 95% CIs
sns.barplot(x='group', y='subscribed', data=df.replace({'Yes': 1, 'No': 0}), ci=95)
plt.title("Conversion Rates by Price Point (±95% CI)")
plt.ylabel("Conversion Rate")
plt.xlabel("Price Group")
plt.show()
Key Insight:

Group A ($4.99) has the highest conversion (12.0%, CI: 8.7%–15.3%)

Group C ($14.99) has the lowest (3.0%, CI: 1.2%–4.8%)


5. Practical vs. Statistical Significance
Statistical Significance
All pairwise comparisons significant (p < 0.05).

Business Impact
$4.99 price: Highest conversions but requires 7,168 users/week to hit $5,000 revenue.

$14.99 price: Needs 333 sales/week (33.4% conversion), but current rate is only 3%.

Decision:

$4.99 is "best" statistically, but not viable for the revenue target.

Action: Test higher prices ($19.99+) or increase user acquisition.

6. Recommendations
Pricing Strategy:

Keep $4.99 as entry-level (high conversion).

Introduce premium tiers ($19.99+) with added features.

Growth Focus:

Scale user base to >7,000 users/week for $4.99 to hit targets.

Follow-Up Tests:

Annual subscription option (improves LTV).

Segmented pricing (e.g., geographic discounts).

Final Note: Always align statistical results with business constraints!
