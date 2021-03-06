FROM rocker/shiny:latest

MAINTAINER Tauno Metsalu "taunometsalu@gmail.com"

RUN apt-get update && apt-get install -y \
	udunits-bin libudunits2-dev libnetcdf-dev \
	libxml2-dev libssl-dev/unstable libv8-dev \
	nano less man-db

RUN R -e "source('http://bioconductor.org/biocLite.R'); biocLite(c('GOsummaries', 'pcaMethods'))"
RUN R -e "install.packages(c('shinyBS', 'stringr', 'RColorBrewer', 'gProfileR', 'RNetCDF', 'reshape2', 'plyr', 'Cairo', 'V8', 'DT', 'svglite'), repos = 'http://cloud.r-project.org/')"
RUN R -e "install.packages(c('devtools', 'FactoMineR'), repos = 'http://cloud.r-project.org/')"
RUN R -e "devtools::install_github('taunometsalu/pheatmap@467e79e458b63a5c66fbbb17779d0e1009b44e1d')"

#in shinyjs_0.5.2, PCA plot clicking doesn't work
RUN R -e "devtools::install_version('shinyjs', version = '0.2.3', repos = 'http://cloud.r-project.org/')"

#dependencies for gridSVG (CRAN version is older):
RUN R -e "install.packages(c('RJSONIO', 'XML', 'gtable'), repos = 'http://cloud.r-project.org/')"
RUN R -e "install.packages('gridSVG', repos = 'http://R-Forge.R-project.org')"

#in ggplot2_2.1.0, jitter plot xlab hovering and clicking doesn't work (different XML structure)
#needs older Hmisc since newer Hmisc than 3.17-0 requires ggplot2 >= 2.0
RUN R -e "install.packages(c('Formula', 'latticeExtra', 'acepack', 'proto'), repos = 'http://cloud.r-project.org/')"
RUN R -e "devtools::install_version('ggplot2', version = '1.0.1', repos = 'http://cloud.r-project.org/')"
RUN R -e "install.packages('http://cran.r-project.org/src/contrib/Archive/Hmisc/Hmisc_3.15-0.tar.gz', repos = NULL, type = 'source')"

ENV TERM=xterm
RUN mv /etc/localtime /etc/localtime.orig
RUN ln -sf /usr/share/zoneinfo/Europe/Tallinn /etc/localtime

#create folders
RUN mkdir /srv/settings/
RUN mkdir /srv/settings_large/
RUN chown shiny:shiny /srv/settings/
RUN chown shiny:shiny /srv/settings_large/

#create user and group, actually not needed for custom version
RUN groupadd -g 1067 metsalu
RUN useradd -g 1067 -u 1061 metsalu

