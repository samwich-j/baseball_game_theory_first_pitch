# Baseball Game Theory Engine

A reproducible research document that calculates the optimal first pitch swing strategy using Nash Equilibrium, real MLB Statcast data, and the Lahman Baseball Database.

## Overview

This project applies **Game Theory** to baseball strategy, answering two key questions:

> *What percentage of first pitches (0-0 count) should a batter swing at to reach Nash Equilibrium?*

> *How should batters adjust their strategy when facing real MLB pitchers who deviate from equilibrium?*

The analysis uses a **2x2 game matrix** with real **Run Expectancy values** from baseball analytics to:
1. Calculate the **Nash Equilibrium** (theoretical optimal strategy for both players)
2. Solve for **Best Response** to real pitcher behavior using 2024 Statcast data
3. Quantify the strategic advantage of exploiting pitcher tendencies

## Game Theory Concepts

### The Game Matrix

The first pitch confrontation is modeled as a simultaneous-move game:

|                | **Swing**          | **Take**          |
|----------------|--------------------|-------------------|
| **Zone**       | Contact outcome    | Called strike     |
| **Waste**      | Chase/whiff        | Ball              |

### Nash Equilibrium

A **Nash Equilibrium** occurs when both players choose optimal mixed strategies such that:
- The batter's swing percentage makes the pitcher **indifferent** between throwing in the zone or waste
- The pitcher's zone percentage makes the batter **indifferent** between swinging or taking

At equilibrium, neither player can improve their expected value by changing strategy alone.

### Run Expectancy Deltas

The payoffs are based on how count transitions affect expected runs in an inning:

- **0-0 to 0-1** (strike): -0.06 runs
- **0-0 to 1-0** (ball): +0.04 runs
- **Contact outcome**: Varies by batter skill (uses OBP as proxy)

## Data Sources

The analysis supports two data sources, selectable via the `data_source` parameter:

### 1. Statcast Data (2024 MLB Season) ⭐ **Recommended**

Uses real pitch-by-pitch data from [Baseball Savant](https://baseballsavant.mlb.com/) including:
- **First-pitch strike rates** by pitcher (actual behavior, not theoretical)
- **Swing rates** and plate discipline metrics by batter
- **Contact quality** (hard-hit %, whiff rates, wOBA)

**Key Feature:** Enables **Best Response Analysis** - calculates optimal batter strategy given the *observed* first-pitch strike rate, not just theoretical equilibrium.

**Files Required:**
- `batter_stats_2024.csv`
- `pitcher_stats_2024.csv`

### 2. Lahman Baseball Database (Historical)

The [Lahman Baseball Database](http://www.seanlahman.com/baseball-archive/statistics/) contains complete batting and pitching statistics from 1871 to present.

**Data Modes:**
1. **League Average** (default): Calculates aggregate statistics from the most recent season
2. **Specific Player**: Fetches individual player stats by name (format: `"Last, First"`)
3. **Manual Entry**: Allows custom statistics for hypothetical scenarios

### Required R Packages

- `tidyverse`: Data manipulation and visualization
- `Lahman`: Baseball statistics database
- `scales`: Number formatting for plots

## Usage

### Installation

```r
# Install required packages
install.packages(c("tidyverse", "Lahman", "scales"))
```

### Running the Analysis

1. **Open the Quarto document**: `game_theory_analysis.qmd`

2. **Configure the analysis** in the "User Configuration" section:

```r
# SELECT DATA SOURCE
data_source <- "statcast"  # Options: "statcast" or "lahman"

# EXAMPLE 1: Statcast league average (uses real 2024 MLB data)
target_batter <- NULL
target_pitcher <- NULL

# EXAMPLE 2: Specific player matchup (Statcast)
data_source <- "statcast"
target_batter <- "Ward, Taylor"
target_pitcher <- "Rodón, Carlos"

# EXAMPLE 3: Historical player (Lahman Database)
data_source <- "lahman"
target_batter <- "Trout, Mike"
target_pitcher <- "Kershaw, Clayton"

# EXAMPLE 4: Custom manual stats
manual_batter_stats <- c(
  avg = 0.300,
  obp = 0.400,
  slg = 0.550,
  pa = 600
)
```

3. **Render the document**:

```bash
quarto render game_theory_analysis.qmd
```

The output HTML file will contain:
- Interactive data source confirmation
- 2x2 payoff matrix
- Nash equilibrium solution (theoretical optimal)
- **Best response analysis** (optimal strategy vs. real pitcher behavior) ⭐ *Statcast only*
- Expected value visualization
- Comparison plot (Nash vs. Best Response) ⭐ *Statcast only*
- Strategic interpretation with actionable insights

## Key Features

✅ **Dual data sources**: Switch between Statcast (2024 real data) and Lahman (historical)
✅ **Best Response Analysis**: Exploit real pitcher tendencies that deviate from equilibrium ⭐ *NEW*
✅ **Mixed strategy Nash equilibrium solver**
✅ **Interactive visualizations** showing EV curves and strategy comparisons
✅ **Actionable insights**: Quantifies expected value gains from optimal adjustments
✅ **Automatic interpretation** of strategic implications
✅ **Reproducible research** format using Quarto
✅ **Custom baseball-themed plots** with professional styling

## File Structure

```
baseball_game_theory_first_pitch/
├── game_theory_analysis.qmd    # Main analysis document (Quarto)
├── batter_stats_2024.csv        # 2024 Statcast batter data
├── pitcher_stats_2024.csv       # 2024 Statcast pitcher data
├── README.md                    # This file
└── .gitignore                   # Git ignore rules
```

## Example Output

When rendered, the analysis produces:

### 1. **Payoff Matrix**
Shows the run expectancy for each strategy combination

### 2. **Nash Equilibrium Solution**
```
⚖️  NASH EQUILIBRIUM SOLUTION
🏏 BATTER'S OPTIMAL STRATEGY:
   Swing on first pitch: 28.8%
   Take on first pitch:  71.2%
```

### 3. **Best Response to Real Data** ⭐ *Statcast only*
```
🎯 BEST RESPONSE TO REAL PITCHER DATA
⚾ OBSERVED PITCHER BEHAVIOR:
   First-pitch strike rate: 62.1%

🏏 BATTER'S OPTIMAL RESPONSE:
   Swing on first pitch: 45.3%

💡 COMPARISON:
   → Pitcher throws MORE strikes than equilibrium
   → Batter should be MORE aggressive
   → Expected value gain: +0.0089 runs per first pitch
```

### 4. **Visualizations**
- Nash equilibrium EV curves showing intersection point
- Comparison plot highlighting Nash vs. Best Response strategies

### 5. **Strategic Analysis**
Actionable interpretation with recommendations for aggressive/patient approach

## Mathematical Foundation

The Nash equilibrium is calculated by solving the indifference equations:

**Batter's optimal swing probability (p):**
```
p = (waste_take - zone_take) / (zone_swing - waste_swing + waste_take - zone_take)
```

**Pitcher's optimal zone probability (q):**
```
q = (waste_take - waste_swing) / (zone_swing - zone_take + waste_take - waste_swing)
```

## References

### Data Sources
- **Baseball Savant / Statcast**: https://baseballsavant.mlb.com/ (2024 MLB pitch-level data)
- **Lahman Baseball Database**: http://www.seanlahman.com/ (Historical statistics 1871-present)

### Game Theory & Baseball Analytics
- Nash, J. F. (1951). "Non-Cooperative Games". *Annals of Mathematics*, 54(2), 286-295
- Tango, T., Lichtman, M., & Dolphin, A. (2007). *The Book: Playing the Percentages in Baseball*

## My Next Step
- I am looking to create a python script that users can play around with and try different situations.
- I am also working on updating the quarto to have more notes and readability, as well as updating the user's ability to manualy input specific data for different theories. 

## License

This project uses the Lahman Baseball Database (Creative Commons Attribution-ShareAlike 3.0 Unported License) and publicly available Statcast data from MLB.

## Contributing

Contributions are welcome! Areas for enhancement:

**Data & Analysis:**
- Multi-count analysis (1-0, 0-1, 2-0, etc.)
- Pitch type decisions (fastball vs. breaking ball)
- Individual pitcher/batter matchup validation
- Comparison to actual observed swing rates

**Situational Factors:**
- Pitcher/batter handedness effects
- Runners on base (especially RISP)
- Score differential and inning
- Historical era comparisons (Deadball vs. Steroid vs. Modern)

## Author

Sam Johnston

Created as a reproducible research document combining R, Game Theory, and Baseball Analytics.
