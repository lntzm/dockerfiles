ARG PYTORCH="1.10.0"
ARG CUDA="11.3"
ARG CUDNN="8"

FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

# To fix GPG key error when running apt-get update
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

RUN apt-get update && apt-get install -y libgl1-mesa-glx libglib2.0-dev openssh-server vim git curl tmux \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# pip install
RUN pip install --no-cache-dir --upgrade pip wheel setuptools
RUN pip install --no-cache-dir tqdm pandas albumentations ipdb==0.13.9 Pillow==9.2.0
RUN pip install --no-cache-dir opencv-python scipy scikit-image scikit-learn matplotlib pycocotools
RUN pip install --no-cache-dir tensorboardX timm lightning-bolts

# # export PATH
# export $(cat /proc/1/environ |tr '\0' '\n' | xargs) is not a need now.

# export conda in shell
RUN echo '\n' >> /root/.bashrc
RUN echo '# activate conda' >> /root/.bashrc
RUN echo 'source /opt/conda/bin/activate base' >> /root/.bashrc

# conda clean
RUN conda clean --all