# Project OmniBus — School Bus Routing RL Competition

**Live site:** [competition.bcamlc.com](https://competition.bcamlc.com) · Hosted by the [BCA Machine Learning Club](https://bcamlc.com) · Sponsored by Jane Street

Project OmniBus is a reinforcement learning competition for high school students. Participants build a dispatcher that routes school buses across the real road network of Hackensack, New Jersey, and submissions are ranked on a public leaderboard.

## The problem

Every morning a district has to pick up students and deliver them to schools with staggered bell times, using a small fleet of capacity-limited buses. That is a variant of the Vehicle Routing Problem, which is NP-hard even when nothing goes wrong. In OmniBus, things go wrong:

- **Bus breakdowns** strand passengers mid-route; other buses have to rescue them and still make their own bell times
- **Road closures** appear mid-route and force rerouting
- **Rush-hour traffic** multiplies travel times on main roads by up to 2.2x
- **No-shows** waste time when a bus drives to a student who isn't there
- **Equity penalty** on the variance of ride times across neighborhoods, so no area gets stuck with 40-minute rides

Static planners like OR-Tools build good plans that break under these disruptions. RL agents learn closed-loop policies that adapt in real time.

## What's in this repo

| Path | Contents |
|---|---|
| `sbrp_rl/` | Gymnasium environment (semi-Markov decision process: each idle bus picks its next stop), DQN baseline in PyTorch, training and visualization scripts |
| `tools/` | Road network generation from OpenStreetMap (OSMnx), scenario generator, leaderboard updater |
| `data/` | Hackensack road network GeoJSON, intersections, default scenario, leaderboard data |
| `index.html`, `simulator.html`, `standings.html` | Competition site with an in-browser route simulator and live standings |
| `.github/workflows/` | GitHub Action that pulls scores from Codabench every 30 minutes and commits the updated leaderboard |

## Formulation (short version)

- **Observation:** clock time, active bus, every bus's position/occupancy/status, every student's location/school/pickup window, live congestion and closures, and an action mask
- **Action:** one discrete choice per idle bus — pick up student *i*, deliver to one of three schools, wait 5 minutes, or return to depot
- **Reward:** points for every student delivered (more if on time), minus penalties for lateness, missed students, miles driven, buses used, rides over 40 minutes, and the variance of ride times across neighborhoods

Full rules and scoring are in [`sbrp_rl/README.md`](sbrp_rl/README.md) and on the competition site.

## Running locally

```bash
cd sbrp_rl
pip install -r ../tools/requirements.txt gymnasium torch matplotlib
python train.py        # train the DQN baseline
python visualize.py    # watch a policy run on the map
```

## My role

I led the project: built the simulator site and automated Codabench leaderboard, worked on the environment, scoring, and competition infrastructure, wrote the competition rules, and handled sponsorship outreach (including the Jane Street sponsorship) and promotion.

**Contributors:** Yash Korgaonkar, Pinak Unawane, Kunj Parikh
