ARG PYTORCH="2.1.1"
ARG CUDA="12.1"
ARG CUDNN="8"
ARG VERSION="2.0"
ARG http_proxy="http://172.17.0.1:15224"
ARG https_proxy="http://172.17.0.1:15224"

FROM lntzm/pytorch${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel:v${VERSION}

# export proxy
RUN echo '\n' >> /root/.bashrc \
    && echo '# export proxy' >> /root/.bashrc \
    && echo 'export http_proxy=http://172.17.0.1:15224' >> /root/.bashrc \
    && echo 'export https_proxy=http://172.17.0.1:15224' >> /root/.bashrc

# config git
RUN git config --global user.name lntzm \
    && git config --global user.email liuzhihang@mail.ustc.edu.cn \
    && git config --global http.https://github.com.proxy http://172.17.0.1:15224 \
    && git config --global https.https://github.com.proxy https://172.17.0.1:15224