
##  Chapt 4

What are other important measures of community function, what else could you have chosen here to complement energy/biomass, anything?

_Ecosystem function can be defined pretty broadly, in terms of - say - nutrient cycling, trophic efficiency, primary productivity. Biomass and energy use are the currencies we're generally most comfortable using for the Portal rodents, because we have a pretty good handle on how to measure/calculate those._

In both this chapter and elsewhere, it's always nice to see some mention (along with an analysis) of model validation (as in, do residuals indicate that the model structure was reasonable for the data)?

_I've gone back through and checked the residuals for the GLS_ **Include figs**

_residuals look reasonably normally distributed, with some outliers. it's slightly weird because there are really only 3 fitted values (time period mean) vs. the many draws from each time period. also, to my recollection,_

The shift in microhabitats is such an interesting part of the story that it would be good to know more specifically if any shifts were similar in both control and exclosure plots? Have there been periodic measurements of veg in the plots?

_I've been curious about this too! The overall shift towards shrub-versus-grassland has occurred site (and probably region-) wide, apparently facilitated by the mid-1990s La Nina cycle. Specific to the site, we have some shrub cover data. I have not found clear differences in shrub cover between control and exclosure plots (although there is considerable within-treatment variation in shrub cover). The shrub data was collected intermittently over time, so can't be used to make a cross-time-period comparison of changes. We generally find subtle and seasonally inconsistent differences in the species composition of annual plants between the control and exclosure plots (e.g. Christensen et. al 2019: https://royalsocietypublishing.org/doi/10.1098/rspb.2019.2269)._


## Chapt 5

Because of the way that the individual body size is simulated, the entire effect you're seeing is (philosophically) driven by changes in relative and total abundance - I know you know this, just something to keep clear in the text.

_Definitely, and thank you for the reminder to keep this clear for the reader!_


Is it possible in this framework for biomass and energy to show different patterns?

_Yes. For about 1/4 of routes, biomass and energy use fall into different broad "syndromes" of change (no trend, decoupled trend, coupled trend). Changes in energy use do not perfectly mirror changes in biomass because of the 3/4 scaling exponent. In this dataset, the common scenario of detectable change is one of community-wide mean biomass increasing and driving a more positive slope for total biomass dynamics than for individuals-driven dynamics. For these communities, mean energy use also increases, but to a much lesser degree, because the scaling exponent between biomass and energy use is <1. This dampens the effects of increases in body size on total energy use (relative to effects on total biomass) and brings the slope for total energy use back closer to the slope for abundance-driven dynamics. This isn't the only scenario we see, or the only way we arrive at different broad categorizations of dynamics for total energy use and total biomass, but it is how I understand the dataset-wide patterns._

Adding a lot of noise with normal distribution - what difference is there from n individs per species all with identical mass?

_This is a good thing to check - in the limit it'd be expected to converge, but ecological data isn't always "in the limit". I re-ran the whole analysis using species-level means and zero intraspecific variation for all individuals. For all but 9 routes (99.3% of routes), the dynamics come out with the results, in terms of syndromes-of-change, under either scenario._


Again, why simulate species ID in individuals-driven using a multinomial distribution rather than just using average abundance per species?

_I didn't remember this initially, but when I went to re-run the pipeline using a deterministic null model, I was reminded! In a null model in which a species' abundance in a given timestep is its average abundance times the total number of individuals observed in that timestep, there are non-integer numbers of individuals. Simple rounding schemes don't reproduce the original **total** number of individuals - which we also care about, because it defines the "individual-abundance-driven" dynamics. Rather than come up with a more involved rounding algorithm, I opted for drawing the appropriate number of individuals from the weighted multinomial distribution._

_That said, I did re-run the pipeline with the deterministic null model, using `round` to take care of non-integer abundances. That analysis deviated more from the original analysis than did the species-mean one, but still came out the same for 92% of routes._


L176: written as four separate models, I presume it was really a single full model and then reduced models - any reason for using AIC here rather than just P-values on covariates? (There aren't that many and you have lots of data.)

_These are fit as four separate models at the route level. I opted for this, rather than using p-values to do stepwise variable selection from a full model. Using p-values leads to a very large number of comparisons (especially when repeated 700 times), and there is some conversation about moving away from stepwise selection. Of course, AIC isn't perfect either, but it is a way to navigate the trade-off between complexity and fit within a bounded set of models without relying on the .05 cutoff._


With noise is introduced by randomness, I would expect to see some number contrary to “true” pattern (without stochastic simulation) - can you estimate this?

_I'm not completely sure that I'm understanding this question properly. Do you mean some number of routes that have results that are different from the most-common outcomes?_



## Chapt 6

About 10 years ago I sent Ken a way of calculating distribution/conf intervals for SN combos without sampling and can also find mean for each rank - not sure if you saw this or if it would be useful (might be slower than what you did), but just mentioning.

_Interesting! I am looking around at other ways of working with the feasible set (and related approaches with different counting rules). I might also be working on a python module for interacting with feasible sets, hopefully a little more efficiently than in the 2020-2021 implementation! As that work spins up, it could be interesting to chat a bit!_
