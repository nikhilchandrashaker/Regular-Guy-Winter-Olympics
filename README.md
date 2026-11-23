# One Regular Guy at the Winter Olympics

A Monte Carlo simulation and interactive HTML dashboard that puts one average adult man into 11 Winter Olympic events and measures how far he is from gold. The winter companion to the summer "regular guy" project.

## Files

| File | What it is |
|---|---|
| `regular-guy-winter-olympics.html` | Single-file dashboard (no build step). Open in any browser. |
| `regular_guy_winter_events.csv` | The 11 events with gold-level benchmarks, baseline values and noise assumptions. |
| `regular_guy_winter_simulations.csv` | 66,000 simulated performances: 2,000 per event for each of 3 fitness levels. |

## Events

Ice: speed skating 500 m and 5000 m, short track 1000 m, figure skating (total score). Snow: alpine downhill, cross-country 15 km, biathlon 10 km, ski jumping, snowboard halfpipe. Sliding: skeleton, luge.

## Dashboard

- Pick a fitness level and the number of Winter Olympics to simulate, then run it.
- Each row shows his median result against gold, plus his best single fluke.
- Press **Play** on any event for a stick-figure animation (gold in yellow, you in red): skaters stride in a crouch, skiers glide, the biathlete stops to shoot, the downhiller tucks down the slope, the ski jumper flies off the hill, the snowboarder works the halfpipe, the figure skater attempts a jump, and the sliders race feet-first or head-first.

## Dataset columns

`events.csv`: discipline, event, direction (lower or higher is better), unit, gold_benchmark, regular_baseline, distance_m (races only, approximate course length), noise_sd.

`simulations.csv`: sim_id, fitness_level (couch, average, weekend_athlete), event, discipline, unit, result, gold_benchmark, pct_of_gold, gap_to_gold. Times are in seconds, ski jumping in metres, figure skating and halfpipe in points.

## Model

1. His baseline is divided (timed events) or multiplied (distance and points) by a fitness multiplier: couch 0.88, average 1.00, weekend athlete 1.12.
2. Each simulated performance applies lognormal noise: `result = baseline * exp(sd * Z)`, with Z standard normal and sd from 0.05 (short sprints) to 0.25 (halfpipe).
3. `pct_of_gold` is gold divided by his time (or his mark divided by gold's mark), capped at 100.
4. In races, the distance behind when gold finishes is `D * (1 - pct_of_gold)`.

## Caveats

- Gold benchmarks and course lengths are rounded approximations, not official records.
- His baselines are guesses for an untrained adult who somehow gets onto the ice, slope or track. In real life, he would likely crash, fall or fail to finish several of these. The model does not simulate falls or disqualifications.
- Scores for figure skating and halfpipe are judged, so the gap to gold is only a rough comparison.
- The CSVs were generated in Python (seed 2026), so the numbers differ slightly from a fresh, unseeded run in the browser.
