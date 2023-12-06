ARG PYTORCH="1.11.0"
ARG CUDA="11.3"
ARG CUDNN="8"
ARG VERSION="1.0"

FROM lntzm/pytorch${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel:v${VERSION}

RUN sed -i 's/Port 9999/#Port 22/g' /etc/ssh/sshd_config
RUN sed -i '105,110d' /root/.bashrc
RUN rm -rf /root/.ssh
