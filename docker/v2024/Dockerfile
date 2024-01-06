# Laboratorio de datos: servicio Shiny Server que usa Postgres
FROM r-base:4.3.2

ENV DEBIAN_FRONTEND=noninteractive

RUN echo "es_MX.UTF-8 UTF-8" >> /etc/locale.gen \
 && locale-gen es_MX.UTF-8 \
 && /usr/sbin/update-locale LANG=es_MX.UTF-8 \
 && dpkg-reconfigure -f noninteractive locales \
 && ln -fs /usr/share/zoneinfo/America/Mexico_City /etc/localtime \
 && dpkg-reconfigure -f noninteractive tzdata

ENV LANG=es_MX.UTF-8  
ENV LANGUAGE=es_MX:es
ENV LC_ALL=es_MX.UTF-8
ENV TZ=America/Mexico_City

# Actualizar la lista de paquetes e instalar paquetes necesarios
RUN apt-get update -qq && apt-get upgrade -y \
 && apt-get install --no-install-recommends -y \
    software-properties-common \
    dirmngr gnupg2 gdebi-core wget locales tzdata \
    gcc g++ gfortran make  \
    libharfbuzz-dev libfribidi-dev libudunits2-dev \
    libfreetype6-dev libpng-dev libtiff5-dev libjpeg-dev \
    r-cran-dbi odbc-postgresql unixodbc-dev \
    libgdal-dev libproj-dev \
    r-cran-tidyverse \
    r-cran-dt \
    r-cran-stringdist \
    r-cran-openxlsx \
    r-cran-doparallel \
    r-cran-sf \
    r-cran-ggrepel \
    r-cran-plyr \
    r-cran-svglite \
 && apt-get autoremove && apt-get autoclean \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

# Instala los paquetes de R para aplicación Shiny
RUN R -e "update.packages(ask = FALSE)" \
 && R -e "install.packages('shiny', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('shinyWidgets', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('shinycssloaders', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('shinyBS', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('shinythemes', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('odbc', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('readODS', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('leaflet', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('leaflet.extras', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('feather', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('ggspatial', repos='https://cran.rstudio.com/')" \
 && rm -rf /tmp/* \
 && rm -rf /var/lib/apt/lists/*

# Instala Shiny Server
# https://posit.co/download/shiny-server/
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN wget --no-verbose "https://download3.rstudio.org/ubuntu-18.04/x86_64/shiny-server-1.5.21.1012-amd64.deb" -O /ss-latest.deb
#COPY shiny-server-1.5.21.1012-amd64.deb /ss-latest.deb
RUN apt-get update -qq && apt-get upgrade -y \
 && gdebi -n /ss-latest.deb \
 && rm -f /ss-latest.deb \
 && rm -rf /tmp/* \
 && apt-get autoremove && apt-get autoclean \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*
 
RUN usermod -u 2000 docker && groupmod -g 2000 docker \
 && usermod -u 1000 shiny && groupmod -g 1000 shiny \
 && rm -rf /srv/shiny-server/* \
 && chown 1000:1000 /srv/shiny-server/ \
 && chown 1000:1000 /var/lib/shiny-server/ \
 && userdel -r docker

EXPOSE 3838
USER shiny
ENTRYPOINT ["shiny-server"]

#$ [1] "/usr/local/lib/R/site-library"
#$ abind	 ggspatial  leaflet.extras     raster	shinyBS		 sp
#$ anytime  gridExtra  leaflet.providers  readODS	shinycssloaders  terra
#$ BH	 jpeg	    odbc	       rosm	shinythemes	 viridis
#$ feather  leaflet    png		       shiny	shinyWidgets
#$ 
#$ [2] "/usr/lib/R/site-library"
#$ askpass     crosstalk	 foreach	iterators  pkgKitten	 rlang	      tidyr
#$ backports   curl	 fs		jquerylib  plyr		 rmarkdown    tidyselect
#$ base64enc   data.table	 gargle		jsonlite   prettyunits	 rstudioapi   tidyverse
#$ bit	    DBI		 generics	knitr	   processx	 rvest	      timechange
#$ bit64	    dbplyr	 ggplot2	labeling   progress	 s2	      tinytex
#$ blob	    digest	 ggrepel	later	   promises	 sass	      tzdb
#$ broom	    docopt	 glue		lazyeval   proxy	 scales       units
#$ bslib	    doParallel	 googledrive	lifecycle  ps		 selectr      utf8
#$ cachem	    dplyr	 googlesheets4	littler    purrr	 sf	      uuid
#$ callr	    DT		 gtable		lubridate  R6		 shiny	      vctrs
#$ cellranger  dtplyr	 haven		magrittr   ragg		 sourcetools  viridisLite
#$ classInt    e1071	 highr		memoise    rappdirs	 stringdist   vroom
#$ cli	    ellipsis	 hms		mime	   RColorBrewer  stringi      withr
#$ clipr	    evaluate	 htmltools	modelr	   Rcpp		 stringr      wk
#$ colorspace  fansi	 htmlwidgets	munsell    readr	 svglite      xfun
#$ commonmark  farver	 httpuv		openssl    readxl	 sys	      xml2
#$ conflicted  fastmap	 httr		openxlsx   rematch	 systemfonts  xtable
#$ cpp11	    fontawesome  ids		pillar	   rematch2	 textshaping  yaml
#$ crayon	    forcats	 isoband	pkgconfig  reprex	 tibble       zip
#$
#$ [3] "/usr/lib/R/library"
#$ base	 codetools  graphics	lattice  mgcv	   rpart    stats4    translations
#$ boot	 compiler   grDevices	MASS	 nlme	   spatial  survival  utils
#$ class	 datasets   grid	Matrix	 nnet	   splines  tcltk
#$ cluster  foreign    KernSmooth	methods  parallel  stats    tools

