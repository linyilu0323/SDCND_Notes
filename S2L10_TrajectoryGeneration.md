# Lesson 10: Trajectory Generation

## Class Notes

- Motion Planning Problem: find a sequence of movements in configuration space that moves the robot from start to goal without hitting any obstacles.

  - Properties: **completeness** (if solution exists, always find a solution; if not, terminate and report failure); and **optimality** (always return a solution with minimal cost)

- Motion Planning Algorithms:

  - Combinatorial Methods: intuitive but not scalable.
  - Potential Field Methods: create virtual "potential field" around obstacles to prevent the robot from getting close, disadvantage: sometimes push us to local minima which prevents to get to final solution.
  - Optimal Control: combining the motion planning and control input generation in one algorithm (steer, throttle, etc.), hard to make it run fast.
  - Sampling Based Methods: utilize collision detection module that probes the free space (rather than computing the whole environment) - hybrid A* is one of them.

- Hybrid A* Algorithm:

  - Problem with A*: the world is discretized, and the planned path is not always driveable. 

  - Adding a "state transition function", e.g. bycicle model, and record not only the cell which we're expanding, but also the (x, y, theta) state vector. Completeness and optimality is sacrificed, but the solution is guaranteed to be drivable. 

  - Refer to [this paper](https://d17h27t6h515a5.cloudfront.net/topher/2017/July/595fe838_junior-the-stanford-entry-in-the-urban-challenge/junior-the-stanford-entry-in-the-urban-challenge.pdf) for details regarding Stanford "Junior" development for DARPA challenge.

  - Hybrid A* Heuristics:

    - non-holonomic-without-obstacles-heuristic:
    - holonomic-with-obstacles-heuristic:

  - Pseudocode: The pseudocode below outlines an implementation of the A* search algorithm using the bicycle model. The following variables and objects are used in the code but not defined there:

    - `State(x, y, theta, g, f)`: An object which stores `x`, `y` coordinates, direction `theta`, and current `g` and `f` values.
    - `grid`: A 2D array of 0s and 1s indicating the area to be searched. 1s correspond to obstacles, and 0s correspond to free space.
    - `SPEED`: The speed of the vehicle used in the bicycle model.
    - `LENGTH`: The length of the vehicle used in the bicycle model.
    - `NUM_THETA_CELLS`: The number of cells a circle is divided into. This is used in keeping track of which States we have visited already.

    ```pseudocode
    def expand(state, goal):
        next_states = []
        for delta in range(-35, 40, 5): 
            # Create a trajectory with delta as the steering angle using 
            # the bicycle model:
    
            # ---Begin bicycle model---
            delta_rad = deg_to_rad(delta)
            omega = SPEED/LENGTH * tan(delta_rad)
            next_x = state.x + SPEED * cos(theta)
            next_y = state.y + SPEED * sin(theta)
            next_theta = normalize(state.theta + omega)
            # ---End bicycle model-----
    
            next_g = state.g + 1
            next_f = next_g + heuristic(next_x, next_y, goal)
    
            # Create a new State object with all of the "next" values.
            state = State(next_x, next_y, next_theta, next_g, next_f)
            next_states.append(state)
    
        return next_states
    
    def search(grid, start, goal):
        # The opened array keeps track of the stack of States objects we are 
        # searching through.
        opened = []
        # 3D array of zeros with dimensions:
        # (NUM_THETA_CELLS, grid x size, grid y size).
        closed = [[[0 for x in range(grid[0])] for y in range(len(grid))] 
            for cell in range(NUM_THETA_CELLS)]
        # 3D array with same dimensions. Will be filled with State() objects 
        # to keep track of the path through the grid. 
        came_from = [[[0 for x in range(grid[0])] for y in range(len(grid))] 
            for cell in range(NUM_THETA_CELLS)]
    
        # Create new state object to start the search with.
        x = start.x
        y = start.y
        theta = start.theta
        g = 0
        f = heuristic(start.x, start.y, goal)
        state = State(x, y, theta, 0, f)
        opened.append(state)
    
        # The range from 0 to 2pi has been discretized into NUM_THETA_CELLS cells. 
        # Here, theta_to_stack_number returns the cell that theta belongs to. 
        # Smaller thetas (close to 0 when normalized  into the range from 0 to 
        # 2pi) have lower stack numbers, and larger thetas (close to 2pi when 
        # normalized) have larger stack numbers.
        stack_num = theta_to_stack_number(state.theta)
        closed[stack_num][index(state.x)][index(state.y)] = 1
    
        # Store our starting state. For other states, we will store the previous 
        # state in the path, but the starting state has no previous.
        came_from[stack_num][index(state.x)][index(state.y)] = state
    
        # While there are still states to explore:
        while opened:
            # Sort the states by f-value and start search using the state with the 
            # lowest f-value. This is crucial to the A* algorithm; the f-value 
            # improves search efficiency by indicating where to look first.
            opened.sort(key=lambda state:state.f)
            current = opened.pop(0)
    
            # Check if the x and y coordinates are in the same grid cell 
            # as the goal. (Note: The idx function returns the grid index for 
            # a given coordinate.)
            if (idx(current.x) == goal[0]) and (idx(current.y) == goal.y):
                # If so, the trajectory has reached the goal.
                return path
    
            # Otherwise, expand the current state to get a list of possible 
            # next states.
            next_states = expand(current, goal)
            for next_s in next_states:
                # If we have expanded outside the grid, skip this next_s.
                if next_s is not in the grid:
                    continue
                # Otherwise, check that we haven't already visited this cell and
                # that there is not an obstacle in the grid there.
                stack_num = theta_to_stack_number(next_s.theta)
                if closed[stack_num][idx(next_s.x)][idx(next_s.y)] == 0 
                    and grid[idx(next_s.x)][idx(next_s.y)] == 0:
                    # The state can be added to the opened stack.
                    opened.append(next_s)
                    # The stack_number, idx(next_s.x), idx(next_s.y) tuple 
                    # has now been visited, so it can be closed.
                    closed[stack_num][idx(next_s.x)][idx(next_s.y)] = 1
                    # The next_s came from the current state, and is recorded.
                    came_from[stack_num][idx(next_s.x)][idx(next_s.y)] = current
    ```

- The A* works great for **unstructured environment** (e.g. parking lot, maze, etc.), but it is less ideal for **structured environment** (e.g. highway), as it ignores information regarding the environment (traffic rules, speed limit, road itself, etc.)

- Jerk Minimizing Trajectories:

  - In structured environment, given a starting and end point, we want to generate a trajectory that is both feasible for car to drive, and is smooth for passengers.

  - Jerk: derivative of acceraltion, or, 3rd derivative of distance traveled, is the factor which makes human feel uncomfortable when driving.

  - To minimize jerk (absolute value, i.e. square of jerk), if we assume polynomial form of trajectory, it's easy to prove that when polynomial is less or equal than 5th order, then total jerk is minimized, i.e. we use below type of polynomial to fit for 1D trajectory:

    ```
    s = a0 + a1*t + a2*t^2 + a3*t^3 + a4*t^4 + a5*t^5
    ```

    In above equation, we have 6 tunable parameters `(a0~a5)`, and we have 6 boundary conditions `[s_init, v_init, a_init, s_final, v_final, a_final]`.

  - Implement a Quintic Polynomial Solver:

    - Given start and end state [s, v, accel]; and T, the duration of maneuver.
    - Evaluate the state function at t=0, we can get: [a0, a1, a2] = [s_init, v_init, 1/2*accel_init]
    - Evaluate the state function at t=T, the other 3 coefficients can be solved by:

    ![img](./img/codecogseqn-6.gif)

    - Refer to `L10U27_JMT.cpp` for implementation details.

  - Validation for feasibility: need to check both max and min velocity (max: road speed limit and vehicle capability, min: no back up); max and min acceleration (max: rollover, power available, etc.; min: braking capability); and steering angle.

  - Ranking Trajectories:

    - Most of the time the behavioral layer won't provide the exact s and d coordinates, so we will be generating trajectories for all desireable goal positions, then discard the ones that are not feasible, that has collision, and then rank the rest.
    - Cost function: Jerk: minimize, and logitudinal jerk is more uncomfortable. Distance to obstacles; Distance to center line; Time; Others... 

- 