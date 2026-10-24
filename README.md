# Correlation & Causality: Confounders, Colliders and the Limits of Controls

## Objective

I wanted to show that correlation alone cannot tell causation apart from confounding or selection, and that adding control variables can either remove bias or create it, depending on the causal structure.

## Methodology

- I simulated three worlds with different causal structures: one with true causation (true effect 2.0), one with pure confounding (true effect 0.0) and one with pure selection (true effect 0.0). I set them up so their correlations all come out near 0.65.
- I drew three basic causal structures as directed graphs with networkx: a confounder, a collider and a mediator.
- I compared regressions with and without controls. Controlling for a confounder recovered the true effect, while controlling for a collider produced a large, highly significant association between two variables that are independent by construction.
- I built an M-bias structure, in which a control variable measured before treatment still biases the estimate because it is a collider between two hidden causes.
- I wrote a small function, `coef_on_d()`, that returns the coefficient on the treatment for any list of controls. I used it to show that also controlling for one of the hidden causes (u1) closes the biasing path again.
- I reproduced the LaLonde (1986) contrast. I compared the experimental estimate from the randomised trial with an estimate that uses a comparison group drawn from the CPS. I then measured how much of the gap covariate adjustment closes.

## Key Findings

- **Correlation does not identify the effect.** The three simulated worlds gave correlations of 0.654, 0.664 and 0.632, even though the true effects were 2.0, 0.0 and 0.0.
- **Controlling for a collider creates bias.** With the collider as a control, the estimated coefficient was -0.592, where the truth is 0.
- **Pre-treatment controls are not automatically safe.** In the M-bias setup, controlling for the pre-treatment collider gave an estimate of -0.191. Once I also controlled for u1, the estimate fell to 0.006, close to the true value of 0.
- **Observational comparisons can be badly off, and controls only partly fix them.** In the LaLonde data, the experimental estimate was $1794 and the naive CPS comparison gave $-8498. With covariate controls, the CPS estimate was $699, which closes 89% of the gap but still does not match the experimental benchmark.

The overall lesson for me was that whether a control helps or hurts depends on where it sits in the causal graph. I cannot decide what to control for by looking at the data alone.
