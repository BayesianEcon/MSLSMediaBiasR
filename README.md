```text
-------------------------------------------------------------------------------------------------------------------------------

        %%%%%%%%%%%%%%  R Package MSLSMediaBiasR   %%%%%%%%%%%%%%

-------------------------------------------------------------------------------------------------------------------------------

The following package contains the MCMC function used in the paper 
"Media Bias and Polarization through the Lens of a Markov Switching Latent Space Network Model".
The MCMC function runs a Metropolis-withing-Gibbs Algorithm to draw the Markov Chains 
of the parameters of a Markov Switching Latent Space Network Model.

To install the package in R:

library(devtools)
install_github("BayesianEcon/MSLSMediaBiasR")

INPUT

The MCMC function expects the use of the columns of 3 data frames as main input: "EL_x", "EL_princ", "DBplane".

* "EL_x" is an edge list of the off-diagonal elements of a network through time with columns:
  -"i": node i
  -"j": node j
  -"t": time t
  -"w": countable weight 
  "one": value 1

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
~~ example with N = 3 and T = 2 ~~
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


----------------------------
 "i", "j", "t", " w", "one"
----------------------------
 1  2   1  w121  1
 1  3   1  w131  1
 2  1   1  w211  1
 2  3   1  w231  1
 3  1   1  w311  1
 3  2   1  w321  1
 1  2   2  w122  1
 1  3   2  w132  1
 2  1   2  w212  1
 2  3   2  w232  1
 3  1   2  w312  1
 3  2   2  w322  1
----------------------------
  
* "EL_princ" is an edge list of the lower-triangle elements (i > j) of a network through time with columns:
  -"i": node i
  -"j": node j
  -"t": time t
  -"w": countable weight 
  "one": value 1

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
~~ example with N = 3 and T = 2 ~~
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


----------------------------
 "i", "j", "t", " w", "one"
----------------------------
 1  2   1  w121  1
 1  3   1  w131  1
 2  3   1  w231  1
 1  2   2  w122  1
 1  3   2  w132  1
 2  3   2  w232  1
----------------------------


* "DBplane" is a dataset of nodes' features through time with columns:
  -"i": node i
  -"t": time t
  -"leaning": leaning index 

~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
~~ example with N = 3 and T = 2 ~~
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


---------------------
 "i", "t", "leaning",
---------------------
 1  1   l11
 2  1   l21
 3  1   l31
 1  2   l12
 2  2   l22
 3  2   l23
---------------------


FUNCTION SIGNATURE
Here below is a description of all the arguments of the function MCMC

result = MCMC(princ_w = EL_princ$w, ..................#edge weights from EL_princ
       princ_ones = EL_princ$ones, ............#vector of ones from from EL_princ
       x_w = EL_x$w, ..........................#edge weights from EL_x
       x_ones = EL_x$ones, ...................#vector of ones from EL_x
       leaning = DBplane$leaning, .............#text analysis leaning from DBplane
       lam_ad_beta = lam_ad_beta, .............#adaptive RW-MH lambda parameter - individual effects
       mu_mat_beta = mu_mat_beta, .............#adaptive RW-MH mu - individual effects
       Sigma_ad_beta = Sigma_ad_beta, .........#adaptive RW-MH sigma - individual effects
       lam_ad_za = lam_ad_za, .................#adaptive RW-MH lambda - latent coordinate - state a (vector of length N)
       mu_mat_za = mu_mat_za, .................#adaptive RW-MH mu - latent coordinate - state a (matrix of dimension Nx2)
       Sigma_ad_za = Sigma_ad_za, .............#adaptive RW-MH sigma - latent coordinate - state a (list of N 2x2 matrices)
       lam_ad_zb = lam_ad_zb, .................#adaptive RW-MH lambda - latent coordinate - state b (see above)
       mu_mat_zb = mu_mat_zb, .................#adaptive RW-MH mu - latent coordinate - state b (see above)
       Sigma_ad_zb = Sigma_ad_zb, .............#adaptive RW-MH sigma - latent coordinate - state b (see above)
       beta = rnorm(Npages, 0, 0.01), ........#starting value - individual effect (vector of length N)
       xi_state1 = xi[,1], ....................#starting value - state 1 (boolean vector of length Times)
       zi_a_1 = rep(0, Npages), ...............#starting value - latent coordinate - state a (vector of length N)
       zi_b_1 = rep(0, Npages), ...............#starting value - latent coordinate - state b (vector of length N)
       mu_beta =0, ............................#prior mean - individual effect
       sigma_beta = 15, .......................#prior sd - individual effect
       mu_a = 0, .............................#prior mean - latent coordinate - state a
       sigma_a = 10, ..........................#starting sd - latent coordinate - state a
       mu_b = 0, .............................#prior mean - latent coordinate - state b
       sigma_b = 10, ..........................#starting sd - latent coordinate - state b
       s_a = 0.1,..............................#prior shape parameter of sigma (gamma(s_a, s_b))
       s_b = 0.1, .............................#prior scale parameter of sigma (gamma(s_a, s_b))
       phi = 50, ..............................#starting value phi
       gamma_0 = 0, ...........................#starting value - gamma_0
       gamma_1 = 0, ...........................#starting value - gamma_1
       a_phi = a_phi, .........................#prior shape parameter of phi (gamma(a_phi, b_phi))
       b_phi = b_phi, .........................#prior scale parameter of phi (gamma(a_phi, b_phi))
       a_gamma_0 = 0, .........................#prior mean parameter of gamma_0
       b_gamma_0 = 15, ........................#prior sd parameter of gamma_0
       a_gamma_1 = 0, .........................#prior mean parameter of gamma_1
       b_gamma_1 = 15, ........................#prior sd parameter of gamma_1
       omega_lower_a = 2, .....................#starting value - 1st parameter dirichlet
       omega_lower_b = 2, .....................#starting value - 2nd parameter dirichlet
       P = P, .................................#2x2 transition Matrix P
       N = Npages, ............................# Number of nodes
       Time = 100, ............................#Number of times
       x_i = EL_x$i, ..........................#index vector of nodes in EL_x
       DBplane_i = DBplane$i, ................#index vector of nodes in DBplane
       prop_sd_gamma = 0.01, ..................#proposal RWMH for the gamma parameters
       prop_sd_phi = 35, ......................#proposal RWMH for the phi parameter
       acc_beta = 0.25, .......................#target acceptance Adaptive RW-MH - individual effects
       acc_zeta_a = 0.25, .....................#target acceptance Adaptive RW-MH - latent coordinates - state a
       acc_zeta_b = 0.25, .................... #target acceptance Adaptive RW-MH - latent coordinates - state b
       pivot = 3, .............................#pivot node (news outlet) to solve identification
       sign= -1, ..............................#sign of the news outlet -1 left or 1 right
       interp_eq = interp_eq, .................#0-1 option for the use of the interpretation equation
       ms_eq = ms_eq, .........................#0-1 option for the use of the markov switching
       rg_eq = rg_eq,..........................#0-1-2 option random graph
       Iterations = Iterations.................#number of iterations
       )

NOTE on the use of options:

- interp_eq = 0 implies that the MS-LS model is computed disregarding equation 3 (see the manuscript)
- ms_eq = 0 implies that the LS model is computed diregarding the Markov-Switching component
- rg_eq = 1 implies that a simple random graph model y_{ijt} ~ Pois(exp(alpha)) is run
- rg_eq = 2 implies that a random graph model y_{ijt} ~ Pois(exp(alpha_i + alpha_j )) is run
- the combination interp_eq = 1, ms_eq = 1, rg_eq = 0 returns the MS-LS model as in the manuscript

OUTPUT

The main output is a list object called "result" containing the following elements:

- result[[1]] the matrix beta_it containing the iteration draws for the individual effects
- result[[2]] the matrix phi_gamma_it containing the iteration draws for the parameters gamma_0, gamma_1, phi
- result[[3]] the matrix zi_a_it containing the iteration draws for the parameters zi_a, the latent coordinates in state a
- result[[4]] the matrix zi_b_it containing the iteration draws for the parameters zi_b, the latent coordinates in state b
- result[[5]] the matrix P_ite containing the iteration draws for the parameters p1 and p2
- result[[6]] the matrix x_it containing the iteration draws for the latent states 
- result[[7]] the matrix HETA containing the likelihood in state a, in state b and given the current x_it
- result[[8]] the matrix Sigma_z_ite containing the iteration draws for the variances of the latent states 

-------------------------------------------------------------------------------------------------------------------------------
