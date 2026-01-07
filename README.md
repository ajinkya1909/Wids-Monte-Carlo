Monte Carlo Methods & Reinforcement LearningMy coursework projects on Monte Carlo simulations, probability experiments, and basic reinforcement learning concepts. This repo has code for estimating π and e, simulating gambling scenarios, pricing stock options, and solving some RL theory problems.What's InsideThis repository contains my assignments from weeks 1, 2, and 3:
Week 1 - Gambler's Ruin

Simulating 10,000 gamblers playing a fair coin flip game
Watching wealth distributions evolve over 1,000 rounds
Visualizing how randomness affects individual outcomes

Week 2 - Monte Carlo Integration

Estimating π by throwing random darts at a circle
Two different methods to estimate e (Euler's number)
Calculating areas under curves using random sampling
Seeing how badly things break in high dimensions
Pricing S&P 500 options using the Black-Scholes model
Simulating stock price paths with geometric Brownian motion
Calculating Greeks (Delta) for risk management

Week 3 - Reinforcement Learning Basics

Working through MDP problems and Bellman equations
Calculating returns with discounting
Understanding why some problems are impossible to solve exactly

Week 1: Gambler's RuinThe SetupThis is a classic probability problem. I simulated 10,000 people who each start with $100 and play a simple game:

Flip a fair coin 1,000 times
Heads = win $1
Tails = lose $1
If you hit $0, you're ruined (game over)
What HappenedThe results are pretty interesting:
Mean final wealth: $100.09 (basically unchanged)
Median final wealth: $100.00 (exactly where they started)
Ruin rate: About 18% of people went completely broke
Big winner: One lucky person ended with $220+
Big loser: Multiple people hit $0 and stayed there
Why This MattersEven though this is a perfectly fair game (no house edge, no advantage to either side), individual outcomes vary wildly. Most people end near $100, but plenty go broke or double their money.This demonstrates an important concept: the average outcome doesn't tell you much about what will happen to you personally. The game is fair on average, but that's cold comfort if you're one of the 18% who went broke.The visualization shows 100 random trajectories (grey lines) plus the mean path (red), best performer (green), and worst performer (blue), along with a histogram of final wealth distribution.Week 2: Monte Carlo IntegrationWhat is Monte Carlo Integration?Imagine you need to find the area of a weird shape but don't know the formula. Monte Carlo says: just throw a bunch of random dots at it, count how many land inside, and you've got your answer. More dots = better answer.Estimating πThe idea: Draw a circle inside a square. Throw random darts at the square. The ratio of darts inside the circle to total darts gives you π/4.I generated random (x,y) points in a square from -1 to 1, checked if x² + y² ≤ 1 (meaning it's inside the circle), and multiplied the ratio by 4.Started with 10 points and went up to 2.6 million. At 1 million samples, got π ≈ 3.14205, which is pretty close to the real value (3.14159).Estimating eMethod 1 - Area under curve: The area under the curve y = 1/x from x=1 to x=e equals 1. So I sampled random points under this curve, measured the area, and worked backwards to find e.Method 2 - Random chain: There's a weird property where if you keep adding random numbers between 0 and 1 until the sum exceeds 1, the average number of additions you need equals e. Implemented this and it actually works.Both methods converged to e ≈ 2.718 after enough trials. The chain method was slightly more accurate with fewer samples.Other ShapesMade a general class that can estimate the area of any 2D shape by random sampling:
Circle: Another way to verify π
Parabola: Area under y = x² from 0 to 1 (answer should be 1/3)
Gaussian curve: Area under e^(-x²)
All of them got within 0.02% of the true answer with a million samples.High DimensionsTried calculating the volume of a ball in 2D, 4D, 8D, and 16D. The volume actually decreases as dimensions increase, which is really counterintuitive but mathematically true. This is called the "curse of dimensionality."The cool thing: Monte Carlo doesn't really care about dimensions. Traditional methods would take forever in high dimensions, but Monte Carlo's accuracy only depends on the number of samples, not the dimension.Week 2.5: Stock OptionsWhat's an Option?A call option gives you the right to buy a stock at a fixed price (the strike) by a certain date. If the stock goes up, your option becomes valuable. If it goes down, you just don't use it.Black-Scholes ModelThe Black-Scholes model assumes stock prices move randomly (but with a trend) following something called geometric Brownian motion. Basically, the price does a random walk but with drift.What I DidSimulated S&P 500 option pricing:

Current index: 5,800
Strike price: 6,000 (slightly out of the money)
Time to expiration: 3 months
Volatility: 15%
Ran simulations with different numbers of random price paths:

1,000 paths: Got $126.09 (way off)
10,000 paths: Got $116.47 (getting closer)
1,000,000 paths: Got $114.45 (within 0.1% of exact answer)
The exact Black-Scholes formula gives $114.32, so my Monte Carlo simulation converged nicely.Also calculated Delta (how much the option price changes when the stock moves $0.01). My simulated Delta was 0.353 vs the true value of 0.352.Week 3: Reinforcement LearningThis week was more theory than code. Worked through problems about Markov Decision Processes (MDPs).What's an MDP?Think of a video game character moving through different states (locations) by taking actions. Each action gives a reward (or penalty) and moves you to a new state. The goal is to figure out the best strategy.Key ConceptsDiscounting: Future rewards are worth less than immediate rewards. If γ = 0.9, a reward of $10 next turn is only worth $9 to you now. This makes sense - would you rather have $100 today or $100 in a year?Returns: The total reward you get from a sequence of actions. Calculated one example where an agent took 4 steps with rewards [-1, -1, -1, +10] and γ=0.9. The total discounted return was 4.58.Bellman Equation: A recursive formula that says "the value of being in a state equals the immediate reward plus the discounted value of where you end up next." It's like dynamic programming.Why Some Problems Are ImpossibleBackgammon has about 10²⁰ possible board positions. To solve it exactly using the Bellman equation, you'd need to invert a matrix that size, which takes (10²⁰)³ = 10⁶⁰ operations.Even the fastest supercomputer would take 10³⁴ years. The universe is only 10¹⁰ years old.Solution: Don't try to solve everything exactly. Use sampling methods like Monte Carlo or temporal difference learning that learn from experience.Reward Shaping ProblemHere's a fun one: You design a robot vacuum that gets +1 reward for picking up dust. The robot learns to pick up dust, drop it, pick it up again, and repeat forever. Infinite rewards!This shows why reward design is tricky. You need to reward the goal (clean room), not the action (picking up dust).Model-Free vs Model-BasedIf you know how the environment works (the transition probabilities), you can plan ahead. But in the real world, you usually don't know this.That's why we learn Q-values Q(s,a) instead of just state values V(s). Q-values tell you directly which action is best, without needing to know what happens next.
