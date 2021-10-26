---
title: "Foldover in $2^{K - p}$ Designs"
description: |
  Strategies for dealiasing effects in follow-up experiments.
author:
  - name: Kris Sankaran
    url: {}
date: 11-25-2021
output:
  distill::distill_article:
    self_contained: false
---



_Readings [8.6, 8.7](https://www.wiley.com/en-us/Design+and+Analysis+of+Experiments%2C+10th+Edition-p-9781119492443#content-section), [Rmarkdown](https://github.com/krisrs1128/stat424_f21/blob/main/_posts/2021-08-17-week11-3/week11-3.Rmd)_

<div class="layout-chunk" data-layout="l-body">


</div>


<div class="layout-chunk" data-layout="l-body">


</div>


1. The main drawback of fractional designs is that we can end up with aliased
effects. However, there are specific ways to follow-up initial experiments in a
way that dealiases these effects.
  * Best case: The hierarchical and hereditary principles allow strong effects to
  be isolated using only a $\frac{1}{2^p}$ fraction of the samples needed for the
  full design.
  * Worst case: Strong effects remain aliased, and a follow-up dealiasing
  experiment is required.
  
2. We’ll discuss two dealiasing strategies, full-foldover and single-factor
foldover. They are both particularly relevant in resolution III designs, where
main effects can be confounded with order-2 interactions.

## Full Foldover

<div class="layout-chunk" data-layout="l-body">


</div>


3.Imagine that we want to delias *main effects* for *all factors* under study.
	* Assume that there aren’t any interaction effects with order > 2
  * Idea: For a second fractional factorial run, reverse the signs of all factors

<div class="layout-chunk" data-layout="l-body">
<div class="figure">
<img src="week11-3_files/figure-html5/unnamed-chunk-3-1.png" alt="Setup for the $2^{7 - 4}$ design with foldover used in the eye focus experiment. The second panel is the full foldover of the first." width="288" />
<p class="caption">(\#fig:unnamed-chunk-3)Setup for the $2^{7 - 4}$ design with foldover used in the eye focus experiment. The second panel is the full foldover of the first.</p>
</div>

</div>



4. What does this do? Let’s consider an the eye focus time example from the
textbook (Example 8.7). The design is a $2^{7 - 3}_{III}$ fractional factorial
experiment (`Seq == 1`) that has then been folded over (`Seq == 2`) by reversing
the signs of factors.

5. Let’s consider the alias structure for the original fractional factorial,
ignoring all interactions of order 3 and higher.

<div class="layout-chunk" data-layout="l-body">
<div class="figure">
<img src="week11-3_files/figure-html5/unnamed-chunk-4-1.png" alt="Alias pattern of the original $2^{7 - 3}$ design." width="624" />
<p class="caption">(\#fig:unnamed-chunk-4)Alias pattern of the original $2^{7 - 3}$ design.</p>
</div>

</div>

Parsing this matrix, the effects derived from alias groups are
\begin{align*}
[A] = A + BD + CE + FG \\
[B] = B + AD + CF + EG \\
[C] = C + AE + BF + DG \\
[D] = D + AB + CG + EF \\
[E] = E + AC + BG + DF \\
[F] = F + BC + BG + DE \\
[G] = G + CD + BE + AF
\end{align*}

<div class="layout-chunk" data-layout="l-body">
<div class="figure">
<img src="week11-3_files/figure-html5/unnamed-chunk-5-1.png" alt="Alias pattern after a full foldover. Note that the signs between main effects and their aliased interactions have switched." width="624" />
<p class="caption">(\#fig:unnamed-chunk-5)Alias pattern after a full foldover. Note that the signs between main effects and their aliased interactions have switched.</p>
</div>

</div>


6. Now, suppose we reversed the signs of all the factors. What happens to the
alias groups? The signs for the second order interactions flip! The resulting
effect estimates are
\begin{align*}
[A]^{fold} = A - BD - CE - FG \\
[B]^{fold} = B - AD - CF - EG \\
[C]^{fold} = C - AE - BF - DG \\
[D]^{fold} = D - AB - CG - EF \\
[E]^{fold} = E - AC - BG - DF \\
[F]^{fold} = F - BC - BG - DE \\
[G]^{fold} = G - CD - BE - AF
\end{align*}

The punchline is that we can now estimate the main effects without any aliasing,
\begin{align*}
A = \frac{1}{2}\left(\left[A\right] + \left[A\right]^{fold}\right) \\
B = \frac{1}{2}\left(\left[B\right] + \left[B\right]^{fold}\right) \\
C = \frac{1}{2}\left(\left[C\right] + \left[C\right]^{fold}\right) \\
D = \frac{1}{2}\left(\left[D\right] + \left[D\right]^{fold}\right) \\
E = \frac{1}{2}\left(\left[E\right] + \left[E\right]^{fold}\right) \\
F = \frac{1}{2}\left(\left[F\right] + \left[F\right]^{fold}\right) \\
G = \frac{1}{2}\left(\left[G\right] + \left[G\right]^{fold}\right) \\
\end{align*}

7. This is in fact a general principle for dealising main effects from second
order interactions. When you flip the signs of all factors in an original
fractional factorial design, you will get a cancellation of second-order terms
when you add pairs of effect estimates.

## Data Example

8. Let’s use these ideas to study effects in the eye data experiment. Let's
first make a Daniel plot of the effects we'd find when running the fractional
factorial before foldover.

<div class="layout-chunk" data-layout="l-body">


</div>


<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>eye</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://readr.tidyverse.org/reference/read_table.html'>read_table2</a></span><span class='op'>(</span><span class='st'>"https://uwmadison.box.com/shared/static/zh7majh2s6gesnu6f27fl17ncqfuwzev.txt"</span><span class='op'>)</span> <span class='op'>%&gt;%</span>
  <span class='fu'><a href='https://dplyr.tidyverse.org/reference/mutate_all.html'>mutate_at</a></span><span class='op'>(</span><span class='fu'><a href='https://ggplot2.tidyverse.org/reference/vars.html'>vars</a></span><span class='op'>(</span><span class='va'>A</span><span class='op'>:</span><span class='va'>G</span><span class='op'>)</span>, <span class='va'>code</span><span class='op'>)</span>
<span class='va'>eye1</span> <span class='op'>&lt;-</span> <span class='va'>eye</span><span class='op'>[</span><span class='va'>eye</span><span class='op'>$</span><span class='va'>Seq</span> <span class='op'>==</span> <span class='fl'>1</span>, <span class='op'>]</span>
<span class='va'>fit</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/stats/lm.html'>lm</a></span><span class='op'>(</span><span class='va'>y</span> <span class='op'>~</span> <span class='va'>A</span> <span class='op'>*</span> <span class='va'>B</span> <span class='op'>*</span> <span class='va'>C</span> <span class='op'>*</span> <span class='va'>D</span> <span class='op'>*</span> <span class='va'>E</span> <span class='op'>*</span> <span class='cn'>F</span>, data <span class='op'>=</span> <span class='va'>eye1</span><span class='op'>)</span>
<span class='va'>effects</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/stats/coef.html'>coef</a></span><span class='op'>(</span><span class='va'>fit</span><span class='op'>)</span><span class='op'>[</span><span class='op'>-</span><span class='fl'>1</span><span class='op'>]</span>
<span class='fu'>daniel_plot</span><span class='op'>(</span><span class='va'>effects</span>, probs <span class='op'>=</span> <span class='fu'><a href='https://rdrr.io/r/base/c.html'>c</a></span><span class='op'>(</span><span class='fl'>0.1</span>, <span class='fl'>0.5</span><span class='op'>)</span><span class='op'>)</span>
</code></pre></div>
<div class="figure">
<img src="week11-3_files/figure-html5/unnamed-chunk-7-1.png" alt="From the original fraction, it seems that the effects for [A], [B], and [D] are important." width="624" />
<p class="caption">(\#fig:unnamed-chunk-7)From the original fraction, it seems that the effects for [A], [B], and [D] are important.</p>
</div>

</div>


It seems like $\left[B\right], \left[D\right]$, and / or $\left[A\right]$ are
important. Inspecting the corresponding alias groups, and using the heredity
principle, some plausible situations are
  * $A, B, D$ are important
  * $A, B, AB$ are important
  * $A, D, AD$ are important
  * $B, D, BD$ are important

9. But without more information, we can't draw further conclusions. To that end,
suppose we've run the full foldover experiment. Let's estimate effects in this
run and then add them to effects from before -- this is how we can estimate the
main effects.

<div class="layout-chunk" data-layout="l-body">
<div class="sourceCode"><pre class="sourceCode r"><code class="sourceCode r"><span class='va'>fit</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/stats/lm.html'>lm</a></span><span class='op'>(</span><span class='va'>y</span> <span class='op'>~</span> <span class='va'>A</span> <span class='op'>*</span> <span class='va'>B</span> <span class='op'>*</span> <span class='va'>C</span> <span class='op'>*</span> <span class='va'>D</span> <span class='op'>*</span> <span class='va'>E</span> <span class='op'>*</span> <span class='cn'>F</span>, data <span class='op'>=</span> <span class='va'>eye</span><span class='op'>[</span><span class='va'>eye</span><span class='op'>$</span><span class='va'>Seq</span> <span class='op'>==</span> <span class='fl'>2</span>, <span class='op'>]</span><span class='op'>)</span>
<span class='va'>effects_fold</span> <span class='op'>&lt;-</span> <span class='fu'><a href='https://rdrr.io/r/stats/coef.html'>coef</a></span><span class='op'>(</span><span class='va'>fit</span><span class='op'>)</span><span class='op'>[</span><span class='op'>-</span><span class='fl'>1</span><span class='op'>]</span>
<span class='fl'>0.5</span> <span class='op'>*</span> <span class='op'>(</span><span class='va'>effects</span> <span class='op'>+</span> <span class='va'>effects_fold</span><span class='op'>)</span><span class='op'>[</span><span class='fl'>1</span><span class='op'>:</span><span class='fl'>6</span><span class='op'>]</span>
</code></pre></div>

```
      A       B       C       D       E       F 
 0.7375 19.0250 -0.9000 14.6875  0.0625  0.2500 
```

</div>


It's now clear that the main effect for $A$ is in fact not important. The only
plausible situation is that $B, D$ and the $BD$ interaction are strong.

## Single-Factor Foldover

10. Imagine that we want to dealias *main and interaction effects* associated
with a *single factor* in the study,
* Idea: For a second fractional factorial run, reverse the signs of just the
factor of interest.

11. The mechanics at work here are similar to those in the full foldover. As
before, let’s focus attention on the $2^{7 - 3}_{III}$.  Remember that the
effect estimates were,
\begin{align*}
[A] = A + BD + CE + FG \\
[B] = B + AD + CF + EG \\
[C] = C + AE + BF + DG \\
[D] = D + AB + CG + EF \\
[E] = E + AC + BG + DF \\
[F] = F + BC + BG + DE \\
[G] = G + CD + BE + AF
\end{align*}

12. Suppose we flip the sign of factor $D$ in the follow-up run. The new effect
estimates would be,
\begin{align*}
[A]^{fold} &= A - BD + CE + FG \\
[B]^{fold} &= B - AD + CF + EG \\
[C]^{fold} &= C + AE + BF + DG \\
[D]^{fold} &= -D + AB + CG + EF \\
[E]^{fold} &= E + AC + BG - DF \\
[F]^{fold} &= F + BC + BG - DE \\
[G]^{fold} &= G - CD + BE + AF
\end{align*}

13. In particular, notice that we can estimate the main effect of $D$ using
\begin{align*}
D = \frac{1}{2}\left(\left[D\right] - \left[D\right]^{fold}\right)
\end{align*}
and interactions involving $D$ using, for example,
\begin{align*}
AD = \frac{1}{2}\left(\left[B\right] - \left[B\right]^{fold}\right).
\end{align*}
```{.r .distill-force-highlighting-css}
```
