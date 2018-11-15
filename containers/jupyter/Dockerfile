FROM nvidia/cuda:9.2-cudnn7-devel-ubuntu16.04
RUN apt update && apt install -y python3 python3-dev curl git

RUN ln -s /usr/bin/python3 /usr/bin/python
RUN curl https://bootstrap.pypa.io/get-pip.py -o /tmp/get-pip.py && \
python /tmp/get-pip.py && rm -f /tmp/get-pip.py
RUN pip install torch_nightly -f https://download.pytorch.org/whl/nightly/cu92/torch_nightly.html
RUN pip install fastai
RUN apt install -y libglib2.0-0
RUN apt update
RUN apt install -y libsm6 libxext6
VOLUME /fastai
WORKDIR /fastai
RUN jupyter nbextension enable --py widgetsnbextension
COPY jupyter_notebook_config.py /home/fastai/.jupyter/jupyter_notebook_config.py
EXPOSE 8888
CMD ["jupyter", "notebook"]
