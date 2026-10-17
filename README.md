# Lid-Driven Cavity Flow Solver (with a Machine Learning Model)

I wrote a Python solver for the 2D lid-driven cavity flow. I checked it against the benchmark data of Ghia et al. (1982). Then I used the solver to make a dataset and trained a small machine learning model on it.

## What is the problem?

A square box is filled with fluid. The top wall (the lid) moves to the right and the other three walls are fixed. The moving lid makes the fluid rotate inside the box. This is a very common test problem in computational fluid dynamics (CFD).

## What I did

1. **Solver:** I used the stream function–vorticity method with finite differences. The Poisson equation is solved with the Jacobi method first, and later with a faster sparse solver (SciPy `splu`).
2. **Validation:** I compared my results with Ghia, Ghia and Shin (1982) for Re = 100, 400 and 1000.
3. **Dataset:** I ran the solver for 37 Reynolds numbers (Re = 100 to 1000, step 25) and saved the results in `cavity_dataset.csv`.
4. **Machine learning:** I trained a polynomial fit and a Random Forest to predict flow features from Re only.

## Results

**Comparison with Ghia et al. (1982), centerline velocities:**

| Re | Grid | Max error in u | Max error in v |
|----|------|----------------|----------------|
| 100 | 41×41 | 0.0065 | 0.0047 |
| 400 | 129×129 | 0.0054 | 0.0030 |
| 1000 | 129×129 | 0.0137 | 0.0080 |

**Machine learning model (test data):**

- The polynomial fit has R² above 0.96 for all six features. The best is v_max with R² = 0.9996.
- The Random Forest has R² above 0.95 for all features.
- One simulation takes about 14.7 seconds, and one prediction takes about 0.14 milliseconds.

## Files

- `lid_driven_cavity_cfd.ipynb`: the full notebook (explanations, code, and results)
- `cavity_dataset.csv`: the dataset made from 37 simulations

## How to run

Open the notebook in Google Colab and run all cells from top to bottom. It uses NumPy, Matplotlib, SciPy, pandas, and scikit-learn.

## Limitations

- I typed the Ghia et al. benchmark values from the paper's tables. For Re = 400, three of the v points are left out because I could not confirm their values.
- The dataset uses an 81×81 grid, which is a bit coarse at high Re. The values near Re = 1000 are slightly less accurate than the 129×129 results.
- The vortex centre (x_c, y_c) is taken from the nearest grid point, so it changes in small steps.
- The machine learning model has only 37 data points and one input (Re). It should not be used outside Re = 100 to 1000.

## Reference

Ghia, U., Ghia, K. N., and Shin, C. T. (1982). High-Re solutions for incompressible flow using the Navier-Stokes equations and a multigrid method. Journal of Computational Physics, 48, 387–411.

## Author

**Khadim Hussain**

- BS in Mathematics, International Islamic University Islamabad
- Email: khadimnetworks.com
- GitHub: https://github.com/Khadim-Hussain10
