LDATS rerender for defense
================

``` r
rodents <- readRDS(here::here("defense_talk", "ldats_rerender", "rodents.RDS"))

#rodents_LDA <- cvlt::LDA_set_user_seeds(rodents$document_term_table, 4, 206)
#odents_cpt <- LDATS::TS_on_LDA(rodents_LDA[[1]], rodents$document_covariate_table, formulas = ~ sin_year + cos_year, timename = "newmoon", nchangepoints = 4, weights = NULL, control = LDATS::TS_control(nit = 1000))

#save(rodents_LDA, file = here::here("defense_talk", "ldats_rerender", "rodents_LDA.RData"))
#save(rodents_cpt, file = here::here("defense_talk", "ldats_rerender", "rodents_cpt.RData"))

load(here::here("defense_talk", "ldats_rerender", "rodents_LDA.RData"))
load(here::here("defense_talk", "ldats_rerender", "rodents_cpt.RData"))
```

``` r
plot(rodents_LDA)
```

![](ldats_rerender_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

``` r
betas <- rodents_LDA$`k: 4, seed: 206`@beta 
colnames(betas) <- colnames(rodents$document_term_table)
betas <- betas %>%
  exp() %>%
  as.data.frame() %>%
  mutate(`Community type` = paste0("Type ", row_number())) %>%
  tidyr::pivot_longer(-`Community type`, names_to = "Species", values_to = "Proportion")

gammas <- rodents_LDA$`k: 4, seed: 206`@gamma %>%
  as.data.frame() %>%
  mutate(Date = as.Date(rodents$document_covariate_table$date)) %>%
  tidyr::pivot_longer(-Date, names_to = "Community type", values_to = "Proportion")%>%
  filter(Date > as.Date("1988-01-01")) %>%
  mutate(`Community type` = paste0("Type ", substr(`Community type`, 2,2)))

rhos <- rodents_cpt$`, gamma ~ sin_year + cos_year, 4 changepoints`$rho_summary %>%
  as.data.frame() %>%
  select(Mode, `Lower_95%`, `Upper_95%`) %>%
  mutate(Changepoint = row.names(.)) %>%
       #  Mean = round(Mean)) %>%
  tidyr::pivot_longer(-Changepoint, names_to = "Value", values_to = "newmoon") %>%
  left_join(rodents$document_covariate_table) %>%
  mutate(Date =as.Date(date)) %>%
  filter(Date > as.Date("1988-01-01"))
```

    ## Joining, by = "newmoon"

``` r
lda_plot <- ggplot(betas, aes(Species, Proportion, fill = `Community type`)) +
  geom_col() +
  facet_wrap(vars(`Community type`), scales = "free") +
  theme_minimal() +
  theme(legend.position = "bottom", axis.text.x = element_text(size = 5.5))

ts_plot <- ggplot(gammas, aes(Date, Proportion, color = `Community type`)) + 
  geom_line() +
  geom_col(data = filter(rhos, Value == "Mode"), aes(Date,  1), width = 100, inherit.aes = F, position = "dodge") +
  scale_fill_viridis_d() +
  theme(legend.position = 'none')
```

``` r
multi_panel_figure(columns = 1, rows = 2) %>%
  fill_panel(lda_plot) %>%
  fill_panel(ts_plot)
```

    ## Setting row to 1

    ## Setting column to 1

    ## Setting row to 2

    ## Setting column to 1

![](ldats_rerender_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

``` r
lda_plot
```

![](ldats_rerender_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

``` r
ts_plot
```

![](ldats_rerender_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->
