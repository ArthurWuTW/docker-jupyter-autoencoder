FROM nvidia/cuda:12.1.0-cudnn8-devel-rockylinux8

ARG HOSTUSER
ARG HOSTUID
ARG HOSTGID

RUN groupadd -g ${HOSTGID} ${HOSTUSER}
RUN useradd ${HOSTUSER} -u ${HOSTUID} -g ${HOSTGID} -m

RUN yum install -y ncurses git
RUN yum install -y python3.12 python3.12-pip
RUN yum install -y libnccl libnccl-devel libnccl-static

RUN pip3.12 install torch==2.3.1 torchvision==0.18.1 torchaudio==2.3.1 --index-url https://download.pytorch.org/whl/cu121
RUN pip3.12 install six matplotlib
RUN pip3.12 install torchsummary seaborn statsmodels scikit-learn keras tensorflow tiktoken tqdm

RUN pip3.12 install jupyter
COPY run-jupyter.sh /home/${HOSTUSER}/run-jupyter
RUN chown ${HOSTUSER}:${HOSTUSER} /home/${HOSTUSER}/run-jupyter



# Install vLLM dependencies
#RUN yum install -y python3.12-devel
#RUN pip3.12 install vllm


USER ${HOSTUSER}
RUN jupyter notebook --generate-config
RUN sed -i "s/# c.ServerApp.allow_origin = ''/c.ServerApp.allow_origin = '*'/g" /home/${HOSTUSER}/.jupyter/jupyter_notebook_config.py
RUN sed -i "s/# c.ServerApp.ip = 'localhost'/c.ServerApp.ip = '0.0.0.0'/g" /home/${HOSTUSER}/.jupyter/jupyter_notebook_config.py
RUN sed -i "s/# c.ServerApp.allow_remote_access = False/c.ServerApp.allow_remote_access = True/g" /home/${HOSTUSER}/.jupyter/jupyter_notebook_config.py
RUN sed -i "s~# c.ServerApp.notebook_dir = ''~c.ServerApp.notebook_dir = '/home/${HOSTUSER}/'~g" /home/${HOSTUSER}/.jupyter/jupyter_notebook_config.py

