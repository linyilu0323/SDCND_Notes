# Lesson 7~10: Path Planning

## Class Notes

### 1. Motion Planning

- First Search Program:
  - Expand to all possible adjacent neighbors, g-value increment for each expansion step.
  - See `L7U11_FirstSearch.py` for example.
- A* Search:
  - Heuristic function: pre-define a function to reflect the distance between each cell to the goal, `h(x, y) <= distance to goal`
  - When progressing in search, use the sum of g-value and heuristic function (f value), and only expand to the space with smaller f value.
- Dynamic Programming:
  - 

