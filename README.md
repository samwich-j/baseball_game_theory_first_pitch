# Baseball Game Theory Engine

A reproducible research document that calculates the optimal first pitch swing strategy using Nash Equilibrium and the Lahman Baseball Database.

## Overview

This project applies **Game Theory** to baseball strategy, specifically answering the question:

> *What percentage of first pitches (0-0 count) should a batter swing at to reach Nash Equilibrium?*

The analysis uses a **2x2 game matrix** with real **Run Expectancy values** from baseball analytics to find the mixed strategy equilibrium where neither the batter nor pitcher can improve their expected outcome unilaterally.

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

### Lahman Baseball Database

The primary data source is the [Lahman Baseball Database](http://www.seanlahman.com/baseball-archive/statistics/), which contains complete batting and pitching statistics from 1871 to the present.

The analysis supports three data modes:

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

2. **Configure the matchup** in the "User Configuration" section:

```r
# Example: League average vs. league average (default)
target_batter <- NULL
target_pitcher <- NULL

# Example: Specific player matchup
target_batter <- "Trout, Mike"
target_pitcher <- "Kershaw, Clayton"

# Example: Custom manual stats
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
- Nash equilibrium solution
- Expected value visualization
- Strategic interpretation

## Key Features

✅ **Automatic data source selection** with fallback logic
✅ **Mixed strategy Nash equilibrium solver**
✅ **Interactive visualizations** showing EV curves
✅ **Automatic interpretation** of strategic implications
✅ **Reproducible research** format using Quarto
✅ **Custom baseball-themed plots** with professional styling

## File Structure

```
baseball_game_theory/
├── game_theory_analysis.qmd    # Main analysis document
├── README.md                    # This file
└── .gitignore                   # Git ignore rules
```

## Example Output

When rendered, the analysis produces:

1. **Payoff Matrix**: Shows the run expectancy for each strategy combination
2. **Nash Equilibrium Solution**: Optimal swing % and zone % for both players
3. **Visualization**: Plot showing where EV curves intersect at equilibrium
4. **Strategic Analysis**: Interpretation of whether to be aggressive or patient

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

- Lahman, S. (2023). *Lahman Baseball Database*. http://www.seanlahman.com/
- Tango, T., Lichtman, M., & Dolphin, A. (2007). *The Book: Playing the Percentages in Baseball*
- Nash, J. F. (1951). "Non-Cooperative Games". *Annals of Mathematics*, 54(2), 286-295

## License

This project uses the Lahman Baseball Database, which is made available under the Creative Commons Attribution-ShareAlike 3.0 Unported License.

## Contributing

Contributions are welcome! Areas for enhancement:
- Additional pitch count scenarios (1-0, 0-1, etc.)
- Pitcher handedness effects
- Situational factors (runners on base, inning, score)
- Historical era comparisons

## Author

Created as a reproducible research document combining R, Game Theory, and Baseball Analytics.
