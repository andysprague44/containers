# Docker image to run rstudio in databricks. 
# Starts from rstudio, adds databricks

FROM rocker/rstudio:3.6.0

# Install minimum for databricks to run
RUN apt-get update \
  && apt-get install --yes \
    openjdk-8-jdk \
    iproute2 \
    bash \
    sudo \
    coreutils \
    procps \
  && /var/lib/dpkg/info/ca-certificates-java.postinst configure \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# hwriterPlus is used by Databricks to display output in notebook cells
# Rserve allows Spark to communicate with a local R process to run R code
RUN R -e "install.packages('hwriterPlus', repos='https://mran.revolutionanalytics.com/snapshot/2017-02-26')" \
 && R -e "install.packages('Rserve', repos='http://rforge.net/')"

# Run the following in a cell once the cluster is up to start rstudio.
# TODO ideally would be able to start the server with an init script, or even better a startup.sh file from the docker image
# ``` 
# %scala 
# import sys.process._
# "/usr/lib/rstudio-server/bin/rstudio-server restart"!! // %scala since %py doesn't work without python installed
# ```