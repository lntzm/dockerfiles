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

# export proxy
RUN echo '\n' >> /root/.bashrc
RUN echo '# export proxy' >> /root/.bashrc
RUN echo 'export http_proxy=http://172.17.0.1:15224' >> /root/.bashrc
RUN echo 'export https_proxy=http://172.17.0.1:15224' >> /root/.bashrc

# config git
RUN git config --global user.name lntzm
RUN git config --global user.email liuzhihang@mail.ustc.edu.cn
RUN git config --global http.https://github.com.proxy http://172.17.0.1:15224
RUN git config --global https.https://github.com.proxy https://172.17.0.1:15224