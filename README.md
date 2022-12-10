
<!-- README.md is generated from README.Rmd. Please edit that file -->

# {aniMotum} <a href='https://ianjonsen.github.io/aniMotum/index.html'><img src='man/figures/logo.png' align="right" height="250" /></a>

#### fit latent variable movement models to animal tracking data for location quality control and behavioural inference

<!-- badges: start -->

[![aniMotum status
badge](https://ianjonsen.r-universe.dev/badges/aniMotum)](https://ianjonsen.r-universe.dev)
[![Coverage
status](https://codecov.io/gh/ianjonsen/aniMotum/branch/master/graph/badge.svg)](https://codecov.io/github/ianjonsen/aniMotum?branch=master)
[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.2628481.svg)](https://doi.org/10.5281/zenodo.2628481)
![R-CMD-check](https://github.com/ianjonsen/aniMotum/actions/workflows/check-full.yaml/badge.svg?branch=master)
<!-- badges: end -->

`{aniMotum}` is an R package that fits continuous-time models in
state-space form to filter error-prone animal movement data obtained via
the Argos satellite system, and to estimate changes in movement
behaviour. Template Model Builder {TMB} is used for fast estimation.
Argos data can be either (older) Least Squares-based locations, (newer)
Kalman Filter-based locations with error ellipse information, or a
mixture of the two. The state-space models estimate two sets of location
states: 1) corresponding to each observation, which are usually
irregularly timed (fitted states); and 2) corresponding to (usually)
regular time intervals specified by the user (predicted states).
Locations are returned as both LongLat and on the Mercator projection
(units=km). The models may be applied with appropriate caution to
tracking data obtained from other systems, such as GPS and light-level
geolocations.

## Installation

First, ensure you have R version \>= 4.0.0 installed (preferably R 4.2.0
or higher):

``` r
R.Version()
```

### From R-Universe

As of `v1.1`, `{aniMotum}` is available via R-Universe. This is where
the latest stable version can always be found. Installation is simple:

``` r
# install from my R-universe repository
install.packages("aniMotum", 
                 repos = "https://ianjonsen.r-universe.dev")
```

However, this may not install any of `{aniMotum}`’s Suggested packages,
which add extra functionality such as path re-routing around land. To
ensure all Suggested packages, either from R-Universe or CRAN are also
installed:

``` r
install.packages("aniMotum", 
                 repos = c("https://cloud.r-project.org",
                           "https://ianjonsen.r-universe.dev"),
                 dependencies = "Suggests")
```

If queried, answer `Yes` to install the source version, provided you
have the appropriate compiler tools available (See **From GitHub
(source)**, below).

To avoid typing the above every time you want to re-install
`{aniMotum}`, you can add my R-Universe repo to your local list of
repositories for package download in your `.Rprofile`. This ensures
`install.packages` automatically grabs the latest version of
`{aniMotum}`

``` r
#install.packages("usethis")
usethis::edit_r_profile()

# add the following text or replace existing repos option
options(repos = c(ianjonsen = 'https://ianjonsen.r-universe.dev',
                  CRAN = 'https://cloud.r-project.org'))
```

If you don’t want to install compiler tools or have trouble getting them
to work then you can manually download a binary version of the
`{aniMotum}` package for Windows or Mac from here
<https://ianjonsen.r-universe.dev/ui#package:aniMotum>. In R, use the
following command to install from the file you’ve just downloaded (where
`path_to_file` is wherever you saved the download:

``` r
# for Windows
install.packages("path_to_file\package-windows-release\aniMotum_1.1.zip", 
                 repos=NULL, type="binary")

# for Mac
install.packages("path_to_file/package-macos-release/aniMotum_1.1.tgz", 
                 repos=NULL, type="binary")
```

### From GitHub (source)

To get the very latest but possibly unstable `{aniMotum}` version, you
can install from the staging branch on GitHub.

On PC’s running Windows, ensure you have installed
[Rtools](https://cran.r-project.org/bin/windows/Rtools/)

On Mac’s, ensure you have installed the Command Line Tools for Xcode by
executing `xcode-select --install` in the terminal; or you can download
the latest version from the URL (free developer registration may be
required). A full Xcode install uses up a lot of disk space and is not
required. Also, ensure you have a suitable Gnu Fortran compiler
installed (e.g.,
<https://github.com/fxcoudert/gfortran-for-macOS/releases>).

``` r
remotes::install_github("ianjonsen/aniMotum@staging")
```

Note: there can be issues getting compilers to work properly, especially
on M1 Macs. Often, this is due to missing or incorrect Xcode Command
Line Tools and/or Fortran compiler. If you encounter install and compile
issues, you may find a solution in the excellent documentation here
[glmmTMB](https://github.com/glmmTMB/glmmTMB). Alternatively, if you
don’t care to have the very latest version then installing from
R-Universe is the preferred approach. R-Universe automatically builds
binary package versions for all common platforms, so you won’t have to
worry about compiler software or issues!

## Usage

`{aniMotum}` is intended to be as easy to use as possible. Here’s a
simple example showing how to fit a move persistence state-space model
to Argos tracking data and visualise the result:

``` r
library(aniMotum)

fit <- fit_ssm(sese, 
               vmax= 4, 
               model = "mp", 
               time.step = 24, 
               control = ssm_control(verbose = 0))

plot(fit, type = 3, pages = 1, ncol = 2)
```

<img src="man/figures/README-ex1-1.png" width="100%" />

``` r
map(fit, 
    what = "predicted", 
    crs = "+proj=stere +lon_0=68 +units=km +datum=WGS84")
```

<img src="man/figures/README-explots2-1.png" width="100%" />

Southern elephant seal silhouettes kindly provided by:  
- female southern elephant seal, Sophia Volzke
([@SophiaVolzke](https://twitter.com/SophiaVolzke), University of
Tasmania)  
- male southern elephant seal, Anton Van de Putte
([@AntonArctica](https://twitter.com/Antonarctica), Université Libre de
Bruxelles)

## What to do if you encounter a problem

If you are convinced you have encountered a bug or
unexpected/inconsistent behaviour when using `{aniMotum}`, you can post
an issue [here](https://github.com/ianjonsen/aniMotum/issues). First,
have a read through the posted issues to see if others have encountered
the same problem and whether a solution has been offered. You can reply
to an existing issue if you have the same problem and have more details
to share or you can submit a new issue. To submit an issue, you will
need to *clearly* describe the unexpected behaviour, include a
reproducible example with a small dataset, clearly describe what you
expected to happen (but didn’t), and (ideally) post a few
screenshots/images that nicely illustrate the problem.

## How to Contribute

Contributions from anyone in the Movement Ecology/Bio-Logging
communities are welcome. Consider submitting a feature request
[here](https://github.com/ianjonsen/aniMotum/issues/new/choose) to start
a discussion. Alternatively, if your idea is well-developed then you can
submit a pull request for evaluation
[here](https://github.com/ianjonsen/aniMotum/pulls). Unsure about what
all this means but still want to discuss your idea? then have a look
through the GitHub pages of community-built R packages like
[tidyverse/dplyr](https://github.com/tidyverse/dplyr) for examples.

## Code of Conduct

Please note that the aniMotum project is released with a [Contributor
Code of
Conduct](https://contributor-covenant.org/version/2/0/CODE_OF_CONDUCT.html).
By contributing to this project, you agree to abide by its terms.

## Acknowledgements

Development of this R package was funded by a consortium of partners
including:  
- **Macquarie University**  
- **US Office of Naval Research** (ONR Marine Mammal Biology;grant
N00014-18-1-2405)  
- Australia’s **Integrated Marine Observing System** (IMOS)  
- Canada’s **Ocean Tracking Network** (OTN)  
- **Taronga Conservation Society**  
- **Birds Canada**  
- **Innovasea/Vemco**  
Additional support was provided by France’s Centre de Synthèse et
d’Analyse sur la Biodiversite, part of the Fondation pour la Recherche
sur la Biodiversité.

Example southern elephant seal data included in the package were sourced
from the IMOS Animal Tracking Facility. IMOS is a national collaborative
research infrastructure, supported by the Australian Government and
operated by a consortium of institutions as an unincorporated joint
venture, with the University of Tasmania as Lead Agent. IMOS supported
elephant seal fieldwork on Iles Kerguelen conducted as part of the IPEV
program No 109 (PI H. Weimerskirch) and the SNO-MEMO program (PI C.
Guinet). SMRU SRDL-CTD tags were partly funded by CNES-TOSCA and IMOS.
All tagging procedures were approved and executed under University of
Tasmania Animal Ethics Committee guidelines.

Animal silhouettes used in the `aniMotum` logo were obtained and
modified from sources:  
- southern elephant seal, Anton Van de Putte
([@AntonArctica](https://twitter.com/Antonarctica), Université Libre de
Bruxelles)  
- humpback whale, Chris Huh via [Phylopic.org](http://phylopic.org)
Creative Commons Attribution-ShareAlike 3.0 Unported  
- mallard duck, Maija Karala via [Phylopic.org](http://phylopic.org)
Creative Commons Attribution-ShareAlike 3.0 Unported  
- leatherback turtle, James R. Spotila & Ray Chatterji via
[Phylopic.org](http://phylopic.org) Public Domain Dedication 1.0  
- white shark, Margo Michaud via [Phylopic.org](http://phylopic.org)
Public Domain Dedication 1.0  
- king penguin, Steven Traver via [Phylopic.org](http://phylopic.org)
Public Domain Dedication 1.0
