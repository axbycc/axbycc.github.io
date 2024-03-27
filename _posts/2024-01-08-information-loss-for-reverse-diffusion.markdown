---
layout: post
title:  "Information Loss for Reverse Diffusion"
author: "Mark Liu"
date:   2024-01-24 13:17:38 -0500
categories: ml-theory
usemathjax: true
---

Generative diffusion models attempt to reverse a stochastic diffusion
process and thereby generate samples from the initial distribution.

Consider the process $x_t$ whose initial distribution is an unknown
data distribution $x_0 \sim p_D$. Let the process evolve according 
to the stochastic differential equation

$$ dx_t = - x_t \,dt + \sqrt{2} dw_t.$$

This is an Ornstein Uhlenbeck process whose stationary distribution
is $N(0,1)$. Following Anderson, we consider the auxiliary process $\bar w_t$
whose intial stat is $0$, and follows the stochastic differential 
equation

$$ d\bar w_t = \sqrt{2} u_\theta(x_t, t) \, dt + dw_t$$

where $u_\theta$ is a machine learned model with parameters $\theta$.
Any particular choice of $\theta$ induces a joint distribution on the
processes $x$ and $\bar w$. 

A priori, the increment $d\bar w_t$ is correlated with $x_t$.
This prevents running the process $x$ in reverse, since we have
no way of sampling $d\bar w_t$ at time $t + dt$ without knowing
what $x_t$ is.

But we can attempt to find $\theta$ so that $d\bar w_t$ is independent from 
from $x_{t+dt}$. We also want $d\bar w_t$ to be efficiently sample-able,
so we also want $d\bar w_t \sim N(0, \sqrt{dt})$.

To this end, consider the loss 
$$L_\theta = I[d\bar w_{t-dt} ; x_t] + D_{KL}[ p^\theta(d\bar w_{t-dt}) \vert\vert \phi(d\bar w_{t - dt})]. $$

This reduces to $E_{x_t, d\bar w_{t-dt}}[ \log p^\theta(d \bar w_{t - dt} | x_t) ] - E_{d\bar w_{t - dt}}[\phi(d \bar w_{t - dt})].$

