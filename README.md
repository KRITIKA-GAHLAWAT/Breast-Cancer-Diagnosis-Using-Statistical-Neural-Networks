# Breast-Cancer-Diagnosis-Using-Statistical-Neural-Networks
# Rational Fractal Spline

My part of a semester 1 group project on fractal interpolation techniques.
This module implements **rational cubic fractal interpolation functions
(FIFs)**, including a variant with **monotonicity-preserving constraints**.

## What This Is

Standard spline interpolation can introduce unwanted oscillations (overshoot/
undershoot) when fitting data that is monotonic or otherwise shape-constrained.
This project implements a **rational fractal interpolation** scheme — built on
Iterated Function System (IFS) theory — that:

- Constructs a self-referential (fractal) interpolant through a given set of
  data points `(x_i, f_i)`.
- Uses **shape parameters** (`v_i`, `w_i`) and **vertical scaling factors**
  (`α_i`, the IFS "roughness" parameters) to control the shape of the curve
  between knots.
- In the monotonicity-constrained version, chooses these parameters so the
  interpolant **preserves monotonicity** of the original data — i.e. no
  spurious wiggles are introduced where the data is strictly increasing (or
  flat/increasing).

## Files

| File | Description |
|---|---|
| `Rational_Fractal_Spline.ipynb` | Core rational fractal spline implementation — builds the interpolant from data points, derivatives at knots, interval lengths, and shape/scaling parameters; plots the resulting curve. |
| `Monotonicity_Rational_interpolation.ipynb` | Extends the above with monotonicity-preserving constraints — computes appropriate `α_i`, `v_i`, `w_i` so the fractal interpolant stays monotonic where the underlying data is. |
| `Rational Fractal Spline.pdf` | Written report explaining the mathematical formulation (theta parametrization, P_i(θ) blending functions, derivative matching at knots) and the results. |

## Method Overview

1. **Input:** a set of knots `(x_i, f_i)` and the derivative values `d_i` at
   each knot.
2. **Interval mapping:** each interval `[x_i, x_{i+1}]` is mapped via
   `θ(x) = (x - x_i)/(x_{i+1} - x_i)` to a normalized parameter.
3. **Rational blending functions** `P_i(θ)`, parameterized by shape
   parameters `v_i, w_i`, are combined with the fractal (IFS) scaling term
   `α_i` to produce the interpolant on each interval.
4. **Fractal component:** the `α_i` (vertical scaling factors) control how
   much self-similar "roughness" is introduced — setting `α_i = 0` recovers a
   classical (non-fractal) rational spline; larger `wi/vi` values sharpen the
   curve's local shape.
5. **Monotonicity version:** parameters are chosen following sufficient
   conditions (as derived in the report) that guarantee monotonicity is
   preserved on intervals where the data itself is monotonic.

## How to Run

```bash
pip install numpy matplotlib
jupyter notebook "Rational_Fractal_Spline.ipynb"
```
Then run `Monotonicity_Rational_interpolation.ipynb` the same way. Both
notebooks are self-contained — data points, parameters, and plotting are all
defined inline (no external dataset needed).

## Reference
See `Rational Fractal Spline.pdf` in this folder for the full mathematical
derivation and worked examples.
