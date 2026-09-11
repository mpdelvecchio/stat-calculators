# Statistical Distribution Calculators

Five interactive calculators for intro/intermediate statistics, built as one page with a tab-style navigation bar:

- **Z (Normal)** — Z-Crit Lookup (enter alpha) or P-value Lookup (enter a Z-statistic); two-tailed, left, or right.
- **t** — Critical value (enter df and alpha) or p-value (enter df and a t-statistic); two-tailed, left, or right.
- **&chi;&sup2; (Chi-Square)** — enter df and alpha, always right-tailed.
- **F (ANOVA)** — enter numerator df, denominator df, and alpha, always right-tailed.
- **q (Tukey&ndash;Kramer)** — enter number of groups (k), error df, and alpha, always right-tailed.

Each page shades the relevant region on a live plot of the distribution and shows an APA-style summary line.

**Live demo:** https://mpdelvecchio.github.io/stat-calculators/

## Using it in class

Click a tab at the top to switch distributions. Each page works the same way: fill in the inputs, and the result box, plot, and explanation update as you type. The q (Tukey&ndash;Kramer) page needs a short numerical calculation to find its critical value, so it briefly shows "Calculating..." before the result appears.

## Running it locally

No build step, no dependencies. Either:

- Double-click `index.html` to open it in a browser, or
- Serve the folder with any static file server (e.g. `python3 -m http.server`) and open the shown address.

## How the math works

All five distributions reduce to two general-purpose numerical building blocks:

- **Regularized incomplete gamma function** `P(a,x)` (series expansion for small x, continued fraction for large x) drives the **chi-square** CDF.
- **Regularized incomplete beta function** `I_x(a,b)` (continued fraction, Numerical Recipes form) drives the **t** and **F** CDFs.
- Both use a **Lanczos approximation** to `ln Gamma(x)` for the normalizing constants.

Critical values (the inverse direction) have no closed form for t, chi-square, or F, so they're found by **bisection** on the CDF above &mdash; robust and simple, since all three CDFs are monotonic in the statistic.

The **studentized range distribution** (Tukey's q) is the hard case: its CDF is a genuine double integral &mdash; an outer integral over a scaled chi-distributed variable, and an inner integral (for each outer point) over the range distribution of *k* standard normal variables. Both are evaluated with **48-point Gauss&ndash;Legendre quadrature** (nodes/weights generated at load time via the standard Newton-iteration algorithm on Legendre polynomials, rather than a hardcoded table). The critical value is then found the same way as the others &mdash; bisection on that CDF.

Every critical-value routine was checked against published tables (t, chi-square, F, and Tukey-HSD q tables) before being wired into the page.

## License

MIT — see [LICENSE](LICENSE).
