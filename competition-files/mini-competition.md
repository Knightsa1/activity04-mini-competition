Linear Regression Mini-competition
================

``` r
library(googlesheets4)
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6     ✔ purrr   0.3.4
    ## ✔ tibble  3.1.8     ✔ dplyr   1.0.9
    ## ✔ tidyr   1.2.0     ✔ stringr 1.4.1
    ## ✔ readr   2.1.2     ✔ forcats 0.5.2
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(tidymodels)
```

    ## ── Attaching packages ────────────────────────────────────── tidymodels 1.0.0 ──
    ## ✔ broom        1.0.0     ✔ rsample      1.1.0
    ## ✔ dials        1.0.0     ✔ tune         1.0.0
    ## ✔ infer        1.0.4     ✔ workflows    1.0.0
    ## ✔ modeldata    1.0.0     ✔ workflowsets 1.0.0
    ## ✔ parsnip      1.0.1     ✔ yardstick    1.0.0
    ## ✔ recipes      1.0.1     
    ## ── Conflicts ───────────────────────────────────────── tidymodels_conflicts() ──
    ## ✖ scales::discard() masks purrr::discard()
    ## ✖ dplyr::filter()   masks stats::filter()
    ## ✖ recipes::fixed()  masks stringr::fixed()
    ## ✖ dplyr::lag()      masks stats::lag()
    ## ✖ yardstick::spec() masks readr::spec()
    ## ✖ recipes::step()   masks stats::step()
    ## • Dig deeper into tidy modeling with R at https://www.tmwr.org

``` r
data <- read_csv("2022 Fall Data Challenge Dataset.xlsx - curated 2019-required.csv")
```

    ## Rows: 15500 Columns: 75
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl (75): BASMID, ALLGRADEX, EDCPUB, SCCHOICE, SPUBCHOIX, SCONSIDR, SCHLHRSW...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
colnames(data)
```

    ##  [1] "BASMID"       "ALLGRADEX"    "EDCPUB"       "SCCHOICE"     "SPUBCHOIX"   
    ##  [6] "SCONSIDR"     "SCHLHRSWK"    "EINTNET"      "MOSTIMPT"     "INTNUM"      
    ## [11] "SEENJOY"      "SEGRADES"     "SEABSNT"      "SEGRADEQ"     "FSSPORTX"    
    ## [16] "FSVOL"        "FSMTNG"       "FSPTMTNG"     "FSATCNFN"     "FSFUNDRS"    
    ## [21] "FSCOMMTE"     "FSCOUNSLR"    "FSFREQ"       "FSNOTESX"     "FSMEMO"      
    ## [26] "FCSCHOOL"     "FCTEACHR"     "FCSTDS"       "FCORDER"      "FCSUPPRT"    
    ## [31] "FHHOME"       "FHWKHRS"      "FHAMOUNT"     "FHCAMT"       "FHPLACE"     
    ## [36] "FHCHECKX"     "FHHELP"       "FOSTORY2X"    "FOCRAFTS"     "FOGAMES"     
    ## [41] "FOBUILDX"     "FOSPORT"      "FORESPON"     "FOHISTX"      "FODINNERX"   
    ## [46] "FOLIBRAYX"    "FOBOOKSTX"    "HDHEALTH"     "CDOBMM"       "CDOBYY"      
    ## [51] "CSEX"         "CSPEAKX"      "HHTOTALXX"    "RELATION"     "P1REL"       
    ## [56] "P1SEX"        "P1MRSTA"      "P1EMPL"       "P1HRSWK"      "P1MTHSWRK"   
    ## [61] "P1AGE"        "P2GUARD"      "TTLHHINC"     "OWNRNTHB"     "CHLDNT"      
    ## [66] "SEFUTUREX"    "DSBLTY"       "HHPARN19X"    "HHPARN19_BRD" "NUMSIBSX"    
    ## [71] "PARGRADEX"    "RACEETH"      "INTACC"       "CENREG"       "ZIPLOCL"

<https://nces.ed.gov/nhes/pdf/pfi/2016_pfih.pdf>

<https://nces.ed.gov/datalab/onlinecodebook/session/codebook/c1a8ab9d-8685-468b-a23f-c6962331c417>

| Variable  | Description                                |
|-----------|--------------------------------------------|
| FCSCHOOL  | Satisfaction with school                   |
| FCTEACHR  | Satisfaction with teachers                 |
| FCSTDS    | Satisfaction with academic standards       |
| FCORDER   | Satisfaction with discipline               |
| FCSUPPRT  | Satisfaction with staff/parent interaction |
| FHHOME    | Days Spent Doing Homework                  |
| FHWKHRS   | Hours Spent Doing Homework                 |
| FHAMOUNT  | Parents Feelings on Amount of Homework     |
| FHCAMT    | Child’s Feelings on Amount of Homework     |
| FHPLACE   | Place to do Homework                       |
| FHCHECKX  | Check for homework completion              |
| FHHELP    | Days help with Homework                    |
| FOSTORY2X | Had a story in the last week               |
| FOCRAFTS  | Did arts and crafts in the last week       |
| FOGAMES   | Played board games in the last week        |
| FOBUILDX  | Worked on a project in the last week       |
| FOSPORT   | Played a sport in the last week            |
| FORESPON  | Discussed Time Management in the last week |
| FOHISTX   | Discussed Ethnic Heritage in the last week |
| FODINNERX | Ate a dinner together in the last week     |
| FOLIBRAYX | Went to the library in the last month      |
| FOBOOKSTX | Went to the bookstore in the last month    |
| HDHEALTH  | Heath of the child                         |
| CDOBMM    | Birth month of the child                   |
| CDOBYY    | Birth year of the child                    |
