# CVRP discrete PSO - results

## Problem
- 45 customers + 1 depot (`Indomarco`), ORS `driving-car` road distances (asymmetric matrix)
- Synthetic demands U{10..49} (seed 42), capacity 100, total demand 1364, lower bound 14 vehicles
- Objective: total travelled distance; encoding: customer permutation + greedy capacity split decoder

## Experimental protocol
- Budget per run: **6000 fitness evaluations** for every metaheuristic
- Tuning seeds 100..104, confirmation seeds 200..209, final comparison seeds 2000..2029 (30 runs/algorithm, disjoint from tuning)
- Default PSO: {'n_particles': 30, 'w': 0.7, 'c1': 1.5, 'c2': 1.5}
- Tuned PSO: {'n_particles': 50, 'w': 0.9, 'c1': 1.5, 'c2': 0.5}

## Stage 3 baseline (100 random permutations)
- mean 870.59 km, best 773.77 km, mean vehicles 16.8

## Stage 5 diagnostics (initial PSO runs)
|   run |   best_distance_km |   final_diversity |   first_iter_diversity<5% |   unique_positions_final |   last_gbest_improvement_iter |   stagnant_iters_% |   particles_moved_last25% |   pbest_improv_last25% |   mean_velocity_len |   iters_with_capped_particles_% |
|------:|-------------------:|------------------:|--------------------------:|-------------------------:|------------------------------:|-------------------:|--------------------------:|-----------------------:|--------------------:|--------------------------------:|
|     1 |            687.339 |                 0 |                         9 |                        1 |                            14 |               93   |                         0 |                      0 |               0.879 |                               0 |
|     2 |            718.342 |                 0 |                        10 |                        1 |                             8 |               96   |                         0 |                      0 |               0.772 |                               0 |
|     3 |            724.317 |                 0 |                         6 |                        1 |                             7 |               96.5 |                         0 |                      0 |               0.576 |                               0 |
|     4 |            728.772 |                 0 |                        11 |                        1 |                            10 |               95   |                         0 |                      0 |               0.899 |                               0 |
|     5 |            737.351 |                 0 |                        10 |                        1 |                            12 |               94   |                         0 |                      0 |               0.948 |                               0 |

## Stage 6 tuning
Top 10 grid configurations (km):
|   w |   c1 |   c2 |    mean |    std |    best |
|----:|-----:|-----:|--------:|-------:|--------:|
| 0.9 |  1   | 0.5  | 614.792 | 20.267 | 582.523 |
| 0.9 |  1.5 | 0.5  | 616.379 | 15.088 | 599.012 |
| 0.7 |  1   | 0.5  | 616.839 | 16.599 | 597.592 |
| 0.5 |  1   | 0.25 | 616.881 | 13.877 | 595.264 |
| 0.3 |  1.5 | 0.5  | 618.057 |  8.632 | 606.845 |
| 0.5 |  1.5 | 0.5  | 618.865 | 27.096 | 587.554 |
| 0.7 |  1.5 | 0.5  | 618.938 | 27.442 | 592.024 |
| 0.5 |  1   | 0.5  | 622.07  |  4.4   | 616.16  |
| 0.7 |  0.5 | 0.25 | 622.301 | 24.466 | 584.029 |
| 0.3 |  1   | 0.5  | 624.259 | 17.293 | 610.67  |

Top-3 confirmation (km):
|   w |   c1 |   c2 |    mean |    std |   n |
|----:|-----:|-----:|--------:|-------:|----:|
| 0.9 |  1.5 |  0.5 | 615.205 | 14.069 |  15 |
| 0.9 |  1   |  0.5 | 616.312 | 16.38  |  15 |
| 0.7 |  1   |  0.5 | 622.736 | 16.576 |  15 |

Swarm size at fixed budget (km):
|   n_particles |   n_iter |    mean |    std |    best |
|--------------:|---------:|--------:|-------:|--------:|
|            10 |      599 | 653.103 | 21.901 | 627.186 |
|            20 |      299 | 630.675 | 13.217 | 613.428 |
|            30 |      199 | 615.205 | 14.069 | 591.745 |
|            50 |      119 | 600.13  | 16.272 | 574.333 |
|           100 |       59 | 606.297 | 23.685 | 564.418 |

## Stage 7 equal-budget comparison (km)
| algorithm              |   mean_km |   std_km |   min_km |   median_km |   max_km |   mean_vehicles |   runs |   gap_to_best_known_% |   improvement_vs_random_% |
|:-----------------------|----------:|---------:|---------:|------------:|---------:|----------------:|-------:|----------------------:|--------------------------:|
| Simulated annealing    |   532.715 |   11.685 |  512.319 |     532.546 |  556.023 |          15.133 |     30 |                 3.981 |                    26.328 |
| Genetic algorithm      |   574.883 |   18.558 |  532.524 |     572.64  |  613.427 |          15.633 |     30 |                12.212 |                    20.497 |
| Discrete PSO (tuned)   |   601.903 |   17.109 |  564.196 |     602.554 |  637.991 |          15.767 |     30 |                17.486 |                    16.76  |
| Discrete PSO (default) |   707.519 |   25.232 |  658.3   |     704.995 |  782.763 |          16.267 |     30 |                38.101 |                     2.154 |
| Random search          |   723.094 |   10.435 |  694.316 |     724.343 |  739.569 |          16.433 |     30 |                41.141 |                     0     |

Clarke-Wright reference: 501.238 km, 15 vehicles (best tuned-PSO run is +12.6% vs CW).

Mann-Whitney U, tuned PSO vs others (Holm-corrected):
| vs                     |   median_diff_km (PSO - other) |   p_raw |   p_holm | significant@0.05   |
|:-----------------------|-------------------------------:|--------:|---------:|:-------------------|
| Random search          |                      -121.789  |       0 |        0 | True               |
| Simulated annealing    |                        70.0083 |       0 |        0 | True               |
| Genetic algorithm      |                        29.914  |       0 |        0 | True               |
| Discrete PSO (default) |                      -102.441  |       0 |        0 | True               |

## Stage 8 best PSO solution
|   vehicle |   stops |   load |   matrix_km |   ors_directions_km | geometry       |   diff_% |
|----------:|--------:|-------:|------------:|--------------------:|:---------------|---------:|
|         1 |       3 |     76 |      33.842 |              33.842 | ORS directions |        0 |
|         2 |       2 |     89 |      33.653 |              33.654 | ORS directions |        0 |
|         3 |       3 |    100 |      35.632 |              35.632 | ORS directions |       -0 |
|         4 |       4 |     98 |      41.93  |              41.93  | ORS directions |       -0 |
|         5 |       3 |    100 |      31.368 |              31.368 | ORS directions |       -0 |
|         6 |       3 |     84 |      59.279 |              59.279 | ORS directions |        0 |
|         7 |       3 |     95 |      25.909 |              25.909 | ORS directions |        0 |
|         8 |       4 |     94 |      57.059 |              57.059 | ORS directions |       -0 |
|         9 |       3 |     82 |      51.676 |              51.676 | ORS directions |        0 |
|        10 |       2 |     57 |      16.173 |              16.173 | ORS directions |        0 |
|        11 |       2 |     71 |      48.686 |              48.686 | ORS directions |        0 |
|        12 |       3 |     98 |      43.115 |              43.115 | ORS directions |        0 |
|        13 |       3 |     93 |      19.625 |              19.625 | ORS directions |       -0 |
|        14 |       3 |     99 |      32.661 |              32.661 | ORS directions |        0 |
|        15 |       3 |     97 |      24.584 |              24.584 | ORS directions |       -0 |
|        16 |       1 |     31 |       9.004 |               9.004 | ORS directions |        0 |

Maps: `map_pso_best.html`, `map_random_best.html`.

## Figures
- `fig_final_comparison.png`
- `fig_pso_diagnostics_all_runs.png`
- `fig_pso_diagnostics_run1.png`
- `fig_tuning_heatmaps.png`
- `fig_tuning_main_effects.png`
- `fig_tuning_swarm_size.png`