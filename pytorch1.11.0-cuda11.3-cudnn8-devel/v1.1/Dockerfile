ARG PYTORCH="1.11.0"
ARG CUDA="11.3"
ARG CUDNN="8"
ARG VERSION="1.0"
ARG http_proxy="http://172.17.0.1:15224"
ARG https_proxy="http://172.17.0.1:15224"

FROM lntzm/pytorch${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel:v${VERSION}

# To fix GPG key error when running apt-get update
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

# add more python packages
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" tensorboard
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" fairseq==0.10.2

RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" lvis pycocotools