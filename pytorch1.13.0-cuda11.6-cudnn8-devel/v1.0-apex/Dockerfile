ARG PYTORCH="1.13.0"
ARG CUDA="11.6"
ARG CUDNN="8"
ARG VERSION="1.0"
ARG http_proxy="http://172.17.0.1:15224"
ARG https_proxy="http://172.17.0.1:15224"

FROM lntzm/pytorch${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel:v${VERSION}

# To fix GPG key error when running apt-get update
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

# add support for apex
WORKDIR /tmp
COPY apex apex/
# ARG PATH="/usr/local/cuda-11.6/bin:$PATH"
# RUN export LD_LIBRARY_PATH="/usr/local/cuda-11.6/lib64:$LD_LIBRARY_PATH"
ENV PATH="/usr/local/cuda-11.6/bin:$PATH"
RUN echo $PATH
RUN pip install -v --disable-pip-version-check --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" apex/
RUN rm -r apex
WORKDIR /workspace
