#dummy line to trigger build
FROM ubuntu:bionic
USER root
ENV TIMEZONE America/Mexico_City
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV DEBIAN_FRONTEND noninteractive 
ENV DEB_BUILD_DEPS="sudo nano less git wget curl python3-dev python3-pip python3-setuptools"
ENV DEB_PACKAGES=""
ENV PIP_PACKAGES_COMMON=""
ENV PIP_PACKAGES_KALE="kubeflow-kale==0.6.1"

RUN apt-get update && export $DEBIAN_FRONTEND && \
    echo $TIMEZONE > /etc/timezone && apt-get install -y tzdata

RUN apt-get update && apt-get install -y $DEB_BUILD_DEPS $DEB_PACKAGES && pip3 install --upgrade pip  

RUN curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash - && apt-get install -y nodejs

RUN pip3 install jupyter "jupyterlab>=2.0.0,<3.0.0" --upgrade

#next password is dummy

RUN jupyter notebook --generate-config && \
    sed -i "s/#c.NotebookApp.password = .*/c.NotebookApp.password = u'sha1:09cdc50d3b1f:53488019d5a1670bbce008c699b24cbe50d28717'/" ~/.jupyter/jupyter_notebook_config.py
        

#RUN pip install $PIP_PACKAGES_COMMON --upgrade
RUN pip3 install $PIP_PACKAGES_KALE --upgrade
RUN jupyter labextension install kubeflow-kale-labextension

RUN pip3 install --quiet "git+https://github.com/optimizacion-2-2021-1-gh-classroom/practica-1-segunda-parte-ltejadal.git#egg=ant_colony&subdirectory=src"

VOLUME ["/shared_volume"]

WORKDIR /

ENV NB_PREFIX ant_colony

ENTRYPOINT ["/usr/local/bin/jupyter", "lab", "--ip=0.0.0.0", "--no-browser", "--allow-root", "--LabApp.allow_origin='*'", "--LabApp.base_url=opturl"]
