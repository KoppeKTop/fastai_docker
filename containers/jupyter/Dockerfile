FROM nvidia/cuda:9.2-cudnn7-devel-ubuntu16.04
RUN apt update && apt-get install software-properties-common python-software-properties -y
RUN add-apt-repository ppa:deadsnakes/ppa -y
RUN apt update && apt install -y python3.6 python3.6-dev curl

RUN rm -f /usr/bin/python
RUN ln -s /usr/bin/python3.6 /usr/bin/python
RUN curl https://bootstrap.pypa.io/get-pip.py -o /tmp/get-pip.py && \
python /tmp/get-pip.py && rm -f /tmp/get-pip.py
RUN pip install torch_nightly -f https://download.pytorch.org/whl/nightly/cu92/torch_nightly.html
RUN pip install fastai
RUN apt update
RUN apt install -y libsm6 libxext6 libglib2.0-0 libfontconfig1 libxrender1
RUN pip install jupyter bcolz opencv-python seaborn graphviz sklearn-pandas isoweek pandas-summary torchtext
VOLUME /fastai
WORKDIR /fastai
RUN jupyter nbextension enable --py widgetsnbextension
COPY jupyter_notebook_config.py /opt/jupyter_notebook_config.py
COPY start /usr/bin/start
EXPOSE 8888
