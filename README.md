☕️ **If this tool helps your team, consider supporting my work!** 

[Buy Me a Coffee](buymeacoffee.com/kendoce)

---

This Story Point Calculator is a lightweight, interactive HTML-based tool to help agile teams estimate story points based on effort, complexity, risk, and other key factors. This calculator is designed to guide meaningful conversations and produce consistent, transparent estimations.

---

## 🚀 Overview

The Story Point Calculator helps teams evaluate work items across six categories:

1. **Requirements**
2. **Dependencies**
3. **Familiarity**
4. **Volume of Work**
5. **Complexity**
6. **Risk**

Each category includes multiple options with associated scores. The calculator applies weighted logic to compute a total score and maps it to a recommended story point value using a Fibonacci scale.

---

## ⚙️ How to use the calculator

This story point calculator can be used by an individual or by a team. The goal is to let the estimation be driven by facts rather than gut feeling.

1. During refinement, someone will discuss the context of a work item that needs to be estimated.

2. Team will decide which options to select per category. (note: If score between dev and qa are different, always take the higher score.)

3. Populate the text box for each category for any necessary grooming notes.

4. Click `Calculate Story Point` button.

5. Discuss among each other whether the story point is reasonable.

6. Go back to the Jira issue and set the story point based on the team agreement.
 
7. Copy the generated summary and paste it in your Jira Issue. (Note: it's better to paste it in the comments for easier access to history in case the work item has been re-estimated).

---

## 🧠 How It Works

### 🔘 Estimation Scope

At the top of the calculator, users can select the estimation scope using radio buttons:

- **Epic**: For high-level estimation of an entire Epic, including coordination, integration, and cross-squad efforts.
- **Children**: For detailed estimation of individual stories or tasks within the Epic.

The selected scope determines both the scoring formula and the descriptions shown for each category.

---

### 🔢 Scoring Categories and Weights

Each category contributes to the total score based on the selected option and its weight:

| Category       | Weight for Children | Weight for Epics |
|----------------|---------------------|------------------|
| Requirements   | 1.0x                | 1.5x             |
| Dependencies   | 1.2x                | 2.0x             |
| Familiarity    | 1.5x                | 3.0x             |
| Volume of Work | 1.5x                | 2.5x             |
| Complexity     | 2.0x                | 3.0x             |
| Risk           | 1.0x                | 1.5x             |

Details on why these weight values were selected can be found in the comments in the [codebase](https://github.com/kendoce/story-calculator/blob/61e41650614a12e86e3f661efb648cc02f49da12/index.html#L578C1-L596C15).
The total score is calculated using a formulas that accepts the weight for the selected scope:

#### Scope Formula

`Total Score = (Requirements * RequirementsWeight) + (Dependencies * DependenciesWeight) + (Familiarity * FamiliarityWeight) + (Volume * VolumeWeight) + (Complexity * ComplexityWeight) + (Risk * RiskWeight)`

---

### 📈 Story Point Mapping

#### Children Scope

| Total Score Range | Story Points |
|-------------------|--------------|
| <= 10             | 1            |
| <= 14             | 2            |
| <= 20             | 3            |
| <= 30             | 5            |
| <= 46             | 8            |
| <= 72             | 13           |
| > 72              | 20+ (Split)  |

#### Epic Scope

| Total Score Range | Story Points |
|-------------------|--------------|
| <= 72             | 13           |
| <= 114            | 21           |
| <= 164            | 34           |
| <= 233            | 55           |
| <= 329            | 89           |
| > 329             | 144+ (Split) |

---

This mapping utilizes the fibonnaci sequence. The ceiling for the efforts will be determined by `<previous ceiling> + (<effort multiplier> * <next fibonnaci sequence>)`. The lowest score that can be produced by the tool is 8.2. As initial baseline, 10points is set as the ceiling for 1 SP. This can change based on team's intuition.

This makes the "effort multiplier" = 2. Therefore, the proceeding story points were calculated as `10 baseline + (2 points * 2)` which will result to `14` and so on and so forth.

For Epics, the lowest effort was copied from the largest Children Scope. This is because it's usually recommended to split as early as 13 SP. It is important to note that as epics get bigger, the more unpredictable the effort becomes. To account for unpredictability, a variance multiplier is added to the formula. Therefore, the formula for epics is now `(<previous ceiling> + (<effort multiplier> * <next fibonnaci sequence>)) * Variance multiplier`. Variance multiplier is set as `[1,0.9,0.85,0.8,...]`.

---

## ❓ Common Questions

**Why isn't 0.5 story points an option?**

Story points are meant to reflect relative effort — not precise time estimates. Using 0.5 points introduces a level of precision that contradicts the abstract nature of story pointing. It can also lead to over-refinement, noisy velocity tracking, and unnecessary complexity in planning. Most agile teams use 1 as the smallest unit of estimation and batch smaller tasks together when needed. In this calculator, the lowest possible score already maps to 1 point, reinforcing the idea that if a task is worth estimating, it’s worth at least 1 point

**Why do different categories have different weights?**

The weights reflect how much each category typically contributes to overall effort and uncertainty:

- **Complexity**: Represents the technical difficulty of the work. Complex stories often require more time, deeper thinking, and more coordination — making this one of the strongest drivers of effort.
- **Risk**: Captures the potential for failure, rework, or side effects. High-risk stories often require extra validation, contingency planning, or stakeholder involvement.
- **Familiarity**: Reflects how much prior experience the team has with the task. Unfamiliar work slows teams down due to learning curves and uncertainty.
- **Volume of Work**: Represents the scale of the task — how much there is to do. While not always complex, large volumes of work still consume time and capacity.
- **Knowledge**: Measures how well the requirements are understood. While important, gaps in knowledge are often resolved early and don’t always scale with effort.
- **Dependencies**: Captures external reliance. While dependencies can cause delays, they don’t always increase the actual work required — hence the baseline weight.

These weights were chosen to balance technical challenge, uncertainty, and execution effort, while keeping the scoring model simple and intuitive.


**Why use non-linear scores like 1, 5, 8, 13 for children or 5,13,21,34 for epics?**

These scores reflect the non-linear nature of effort. The jump from 'Medium' to 'High' effort is often more than just one unit. This scale helps avoid false precision and aligns with agile estimation practices.

**How is the score mapped to story points?**

The calculator uses a Fibonacci-based scale (1, 2, 3, 5, 8, 13, 21+) to convert the total score into story points. This mapping is designed to reflect relative effort — not exact time or hours.

The ranges are intentionally non-linear to:
- Reflect the increasing uncertainty and coordination required as effort grows
- Encourage teams to split overly large stories
- Avoid false precision in estimation

This mapping is not tied to your team’s actual velocity (e.g., how many points you complete per sprint), but rather to the relative size and complexity of work items. It helps teams consistently categorize stories as small, medium, or large based on effort, not duration.


---

## 📌 Final Notes

- Use this tool as a conversation starter, not a replacement for team judgment.
- Revisit and recalibrate the mapping as your team gains experience.
- Consider batching very small tasks or splitting very large ones.
---
<br>

## Happy estimating! 🎯

### Author

Created and maintained by [kendoce](https://github.com/kendoce).


