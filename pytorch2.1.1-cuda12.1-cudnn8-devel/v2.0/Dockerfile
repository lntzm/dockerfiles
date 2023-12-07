ARG PYTORCH="2.1.1"
ARG CUDA="12.1"
ARG CUDNN="8"
ARG http_proxy="http://172.17.0.1:15224"
ARG https_proxy="http://172.17.0.1:15224"

FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

# # To fix GPG key error when running apt-get update
# RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub
# RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

# modify time zone
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update \
    && apt-get install -y libgl1-mesa-glx libglib2.0-dev vim git git-lfs curl wget tmux openssh-server \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# pip install
# RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" --upgrade pip wheel setuptools
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" \
    h5py nltk tqdm termcolor omegaconf timm einops tensorboard wandb jupyterlab \
    numpy pandas scipy scikit-learn scikit-image pillow matplotlib opencv-python \
    ftfy decord ffmpeg-python fvcore pycocotools openai \
    transformers==4.35.2 tokenizers==0.15.0 accelerate==0.25.0 peft==0.6.2 sentencepiece \
    protobuf

# RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" -U openmim
# RUN mim install --no-cache-dir --proxy="http://172.17.0.1:15224" mmcv-full

# choose ssh port
RUN sed -i 's/#Port 22/Port 9999/g' /etc/ssh/sshd_config

# # export PATH
# export $(cat /proc/1/environ |tr '\0' '\n' | xargs) is not a need now.

# export conda in shell
RUN echo '\n' >> /root/.bashrc \
    && echo '# activate conda' >> /root/.bashrc \
    && echo 'source /opt/conda/bin/activate base' >> /root/.bashrc

# add public key
RUN mkdir -p /root/.ssh \
    && chmod 700 /root/.ssh \
    && echo 'ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAID9dgCjYZSIBWAShfHsLuGDZdqPf4JToOJSt/5214oIR lntzm@container_v2' > /root/.ssh/authorized_keys \
    && chmod 600 /root/.ssh/authorized_keys

# run ssh service automatically
RUN echo '\n' >> /root/.bashrc \
    && echo '# start ssh automatically' \
    && echo 'ssh_status=`service ssh status`' >> /root/.bashrc \
    && echo 'if [[ ${ssh_status} =~ "not running" ]]' >> /root/.bashrc \
    && echo 'then' >> /root/.bashrc \
    && echo '    service ssh start' >> /root/.bashrc \
    && echo 'fi' >> /root/.bashrc

# conda clean
RUN conda clean --all
