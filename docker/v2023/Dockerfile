# Laboratorio de datos: servicio Shiny Server que usa Postgres
FROM rocker/shiny:4.3.2

# Actualiza el sistema   
RUN apt-get update -qq && apt-get upgrade -y \
    && apt-get install --no-install-recommends -y software-properties-common dirmngr gnupg2 gdebi-core wget \
    && apt-get autoremove && apt-get autoclean \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

SHELL ["/bin/bash", "-o", "pipefail", "-c"]
RUN wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc && \
    add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/" && \
    add-apt-repository "ppa:c2d4u.team/c2d4u4.0+"

# Actualizar la lista de paquetes e instalar paquetes necesarios
RUN apt-get update -qq && apt-get upgrade -y \
    && apt-get --no-install-recommends -y install \
    r-cran-rodbc odbc-postgresql \
    r-cran-dbi r-cran-odbc \
    libgdal-dev libproj-dev \
    r-cran-tidyverse \
    r-cran-dt \
    r-cran-crayon \
    r-cran-jsonlite \
    r-cran-stringr \
    r-cran-stringdist \
    r-cran-openxlsx \
    r-cran-readxl \
    r-cran-readods \
    r-cran-leaflet \
    r-cran-leaflet.extras \
    r-cran-doparallel \
    r-cran-ggplot2 \
    r-cran-r6 \
    r-cran-rgdal \
    r-cran-sf \
    r-cran-ggspatial \
    r-cran-rvest \
    r-cran-ggrepel \
    r-cran-feather \
    r-cran-tibble \
    r-cran-pkgconfig \
    && apt-get autoremove && apt-get autoclean \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN echo ".libPaths(c(.libPaths(), '/usr/lib/R/site-library'))" >> /usr/local/lib/R/etc/Rprofile.site

# Instala los paquetes de R para aplicación
RUN R -e "update.packages(ask = FALSE)" \
 && R -e "install.packages('shiny', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('shinyWidgets', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('shinycssloaders', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('shinyBS', repos='https://cran.rstudio.com/')" \
 && R -e "install.packages('shinythemes', repos='https://cran.rstudio.com/')"\
 && rm -rf /tmp/* \
 && rm -rf /var/lib/apt/lists/*

EXPOSE 3838

RUN usermod -u 1000 shiny && groupmod -g 1000 shiny
RUN chown 1000:1000 /srv/shiny-server/
