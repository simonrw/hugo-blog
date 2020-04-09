---
title: Emcee in Rust
Authors: Simon Walker
date: 2017-06-07 19:59:23
category: tech
tags:
- rust
---

I've been re-implementing the [Python `emcee` library][emcee] in Rust.

I thought it would be a good project to tackle for a few reasons:

* I actively use it in my own work, and am reasonably familiar with the API and how it works
* it has few external dependencies and is mostly pure Python
* it performs cpu-limited computations which suit a compiled high performance language
* the Python version has parallelism to increase speed, which should be easily achievable with rust

(We'll see that for the time being the last point has been put on hold.)

After getting the project to feature parity, or at least for the major features are completed, I thought I'd reflect a little on the process, and differences in design between the Rust version and the Python version.

## Background

`emcee` is an Ensemble sampler for Markov-Chain Monte-Carlo (MCMC) processes. It uses a series of walkers to explore N-dimensional parameter space efficiently and without getting hung up on sharp local likelihood spikes.

The API is very simple to use - you supply your "objective" function, a function to be maximised, and an array of starting points for each walker, and the sampler samples the parameter space. After the given number of iterations have been performed, the location in parameter space of each of the walkers at each iteration can be analysed to extract posterior distributions, find correlations between parameters, and visualise the process.

The objective function encodes prior probabilities and likelihood values, as per [Bayes' rule][bayes], which is applicable for problems in science as Bayesian statistics are my preferred way to describe probability. For more information on this topic, see Jake VanderPlas' [excellent series][jakevdp].

## The Rust version

When starting to write the sampler, I realised that due to Python's dynamic nature, the objective function and required arguments are quite dynamic. In Python the objective function is wrapped in a wrapper which stores the function arguments and keyword arguments so the function can be pickled and sent to other processes:

```python
class _function_wrapper(object):
    """
    This is a hack to make the likelihood function pickleable when ``args``
    or ``kwargs`` are also included.
    """
    def __init__(self, f, args, kwargs):
        self.f = f
        self.args = args
        self.kwargs = kwargs

    def __call__(self, x):
        try:
            return self.f(x, *self.args, **self.kwargs)
        except:
            import traceback
            print("emcee: Exception while calling your likelihood function:")
            print("  params:", x)
            print("  args:", self.args)
            print("  kwargs:", self.kwargs)
            print("  exception:")
            traceback.print_exc()
            raise
```
[(link)][function-wrapper]

This dynamic wrapping of a function is not really supported in Rust, where everything must have a known type at compile time.

Instead I used Rust's trait system to allow the user to supply their own objective function and for arguments to be stored with the objective `Struct`:

```rust
pub trait Prob { 
    fn lnlike(&self, params: &Guess) -> f64;
    fn lnprior(&self, params: &Guess) -> f64;

    fn lnprob(&self, params: &Guess) -> f64 {
        let lnp = self.lnprior(params);
        if lnp.is_finite() {
            lnp + self.lnlike(params)
        } else {
            -::std::f64::INFINITY
        }
    }
}
```

Note the default implementation of Bayes' rule, which the user does not have to supply. I find this separation between prior probability and objective function helps understanding for new users, who hear about applying priors to MCMC analysis and are then asked to write a single function.

Where Python defines external data when constructing the sampler, the Rust version allows the user to store any data on whatever struct implements `Prob`, for example for a simple linear model:

```rust
    struct LinearModel<'a> {
        x: &'a [f64],
        y: &'a [f64],
    }

    impl<'a> LinearModel<'a> {
        fn new(x: &'a [f64], y: &'a [f64]) -> Self {
            LinearModel { x, y }
        }
    }

    impl<'a> Prob for LinearModel<'a> {
        fn lnprior(&self, _params: &Guess) -> f64 {
            // no prior information
            0.0f64
        }

        fn lnlike(&self, params: &Guess) -> f64 {
            let m = params[0];
            let c = params[1];
            let sum = self.x
                .iter()
                .zip(self.y)
                .fold(0.0f64, |acc, (x, y)| {
                    let model_value = m * x + c;
                    let residual = y - model_value;
                    acc + residual.powf(2.0)
                });
            -sum
        }
    }
```

## Problems encountered

Random floating point numbers are hard to work with! Despite leaning on the [`assert_approx_eq`][assert-approx-eq] crate which compares floating point numbers for _approximate_ equality, sometimes extra wide tolerances had to be used to get the numbers to compare.

As alluded to earlier, I have not got parallelism working yet in the Rust implementation. To be fair, and as expected the sampler is much faster than it's python counterpart (once it's run in `release` mode that is), but that is not very forward thinking.

The Python implementation parallelises the application of the user's objective function to the vector of trial walker positions. The (usually correct) assumption is that the user's objective function is much slower than a single iteration of the sampler, and by it's nature the MCMC sampling algorithm is difficult to parallelise in general.

I did struggle with parallelising in the same way, with both `threadpool` and `rayon`, due to borrowing and `Sync` problems, but the performance is good for the time being. Should the library become popular (i.e. more than just me using it) and the need arise, I'll look into it again.

On that note, the latest release has been tagged `1.0.0-alpha.1` indicating that it's got the features I wish to add for a version 1 library, but needs more user testing.

So if you're interested, please check out the library at [`https://crates.io/crates/emcee`](https://crates.io/crates/emcee) and read the [documentation](https://docs.rs/emcee/1.0.0-alpha.1/emcee/). I appreciate any feedback.


[emcee]: https://dan.iel.fm/emcee/current/
[bayes]: https://en.wikipedia.org/wiki/Bayes%27_theorem
[jakevdp]: https://jakevdp.github.io/blog/2014/03/11/frequentism-and-bayesianism-a-practical-intro/
[function-wrapper]: https://github.com/dfm/emcee/blob/7984142541bd907fc1ef74fb2c49b663bb9086f3/emcee/ensemble.py#L507
[assert-approx-eq]: https://crates.io/crates/assert_approx_eq
