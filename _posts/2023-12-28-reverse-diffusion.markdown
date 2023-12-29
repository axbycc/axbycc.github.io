---
layout: post
title:  "A Counterexample in Reverse Diffusion"
author: "Mark Liu"
date:   2023-12-28 17:25:38 -0500
categories: ml-theory
usemathjax: true
---

I will assume familiarity with Brian Anderson's paper ["Reverse-Time Diffusion Equation Models"](https://core.ac.uk/download/pdf/82826666.pdf). The paper's main theorem was used to justify reverse-diffusion sampling in [this](https://yang-song.net/blog/2021/score/) introduction
to diffusion models by Yang Song. (Though this post necessarily contradicts Song's article, Song's great animations and explanations make his article a worthwhile read.)

Song says "Importantly, any SDE has a corresponding reverse SDE"
and provides the equation $(10)\; dx = [f(x,t) - g^2(t) \nabla_x \log p_t(x)]dt + g(t) dw$. I will call this equation "Anderson's reverse SDE". The meaning is that the above process, simulated in reverse time, will have the same distribution of paths as the forward time SDE $dx = f(x, t)dt + g(t) dw$. 

Song neglects the technical condition from Anderson's paper, which is that the Fokker-Planck equation associated to the SDE $dx = f(x, t)dt + g(t) dw$ must have a *unique* solution. Usually practictioners can freely disregard such technical conditions, because they are usually satisfied by the non-pathological objects encountered in practice. Not so here. This precondition was [criticized elsewhere](https://projecteuclid.org/journals/annals-of-probability/volume-14/issue-4/Time-Reversal-of-Diffusions/10.1214/aop/1176992362.full) by Haussman and Pardoux as being "rather unverifiable." (Interestingly, these authors provide their own reverse diffusion formulation which I haven't had the time to work through yet.) 

Indeed we have the following SDE that does not have a corresponding reverse SDE in the sense of Anderson. Take $x_0 \sim N(0, 1)$ and $dx = -x \,dt + \sqrt{2} dw$. This is just an Ornstein Uhlenbeck process initialized in its stationary distribution. In this case, Anderson's reverse Weiner process construction leads to $d \bar w = \sqrt{2} x\, dt + dw$ and initial condition $\bar w = 0$. It was [proven](https://math.stackexchange.com/a/4834356) by math.stackexchange user NN2 that $\bar w_s$ is correlated with $x_t$ for $s < t$, and that $\bar w$ is not a Wiener process. In that case, the reverse time process $x$ is not a diffusion in $\bar w$ and Anderson's construction fails. However, $\bar w$ was [surprisingly close](https://math.stackexchange.com/questions/4834279/a-sde-that-forgets-its-input) to a Wiener process, and *almost* independent from $x$. Perhaps there is some deeper theory of "almost" reverse time SDEs that can justify applications.

This particular issue aside, Song also suggests solving the reverse SDE (sampling) with the Euler-Maruyama method. The quantity $\sqrt{\vert \Delta t\vert}z_t$ is used to approximate the reverse time increment $dw_t$. In the cases where Anderson's formula gives the correct reverse diffusion, this simulation is still problematic because the reverse time Weiner process would be a Brownian Bridge terminating at zero. Using a Wiener process instead would over-estimate the variance at the initial time.

All these facts are papered over, however, with the Predictor-Corrector sampling mentioned later on in Song's article. Even if the Predictor (Anderson's reverse SDE) is faulty, or if Anderson's reverse SDE is simulated incorrectly, enough corrector steps will fix the issue, as long as the score $\nabla p(t, x_t)$ is well estimated.

But at this point, it seems that Anderson's reverse time SDE can only be understood as an empirical heuristic, which should be augmented by corrector steps for proper sampling.




