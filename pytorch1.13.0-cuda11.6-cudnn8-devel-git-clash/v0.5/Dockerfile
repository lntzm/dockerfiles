ARG PYTORCH="1.13.0"
ARG CUDA="11.6"
ARG CUDNN="8"
ARG http_proxy="http://172.17.0.1:17890"
ARG https_proxy="http://172.17.0.1:17890"

FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

# To fix GPG key error when running apt-get update
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

RUN apt-get update \
    && apt-get install -y libgl1-mesa-glx libglib2.0-dev openssh-server vim git curl tmux \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# pip install
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:17890" --upgrade pip wheel setuptools
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:17890" h5py termcolor nltk tqdm pandas ftfy regex
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:17890" opencv-python scipy scikit-image scikit-learn pillow matplotlib
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:17890" tensorboardX timm einops fvcore ffmpeg-python omegaconf
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:17890" transformers

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

# choose ssh port
RUN sed -i 's/#Port 22/Port 9999/g' /etc/ssh/sshd_config

# # export PATH
# RUN echo '\n' >> /root/.bashrc
# RUN echo '# export PATH when logging with ssh' >> /root/.bashrc
# # RUN echo 'export $(cat /proc/1/environ |tr' "'"'\0'"'" "'"'\n'"'" '| xargs)' >> /root/.bashrc
# # RUN sed -i '$a export $(cat /proc/1/environ |tr' "'"\\0"'" "'"\\n"'" '| xargs)' /root/.bashrc
# RUN echo -n 'export $(cat /proc/1/environ |tr ' >> /root/.bashrc
# RUN echo -n "'" >> /root/.bashrc
# RUN echo -n '\\' >> /root/.bashrc
# RUN echo -n '0' >> /root/.bashrc
# RUN echo -n "'" >> /root/.bashrc
# RUN echo -n ' '  >> /root/.bashrc
# RUN echo -n "'" >> /root/.bashrc
# RUN echo -n '\\' >> /root/.bashrc
# RUN echo -n 'n' >> /root/.bashrc
# RUN echo -n "'" >> /root/.bashrc
# RUN echo ' | xargs)'  >> /root/.bashrc

# export conda in shell
RUN echo '\n' >> /root/.bashrc
RUN echo '# activate conda' >> /root/.bashrc
RUN echo 'source /opt/conda/bin/activate base' >> /root/.bashrc

# export proxy
RUN echo '\n' >> /root/.bashrc
RUN echo '# export proxy' >> /root/.bashrc
RUN echo 'export http_proxy=http://172.17.0.1:17890' >> /root/.bashrc
RUN echo 'export https_proxy=http://172.17.0.1:17890' >> /root/.bashrc

# config git
RUN git config --global user.name lntzm
RUN git config --global user.email liuzhihang@mail.ustc.edu.cn
RUN git config --global http.https://github.com.proxy http://172.17.0.1:17890
RUN git config --global https.https://github.com.proxy https://172.17.0.1:17890
# RUN git config --global credential.helper store
# RUN git config --global http.sslverify false
# RUN git config --global https.sslverify false

# add public key
RUN mkdir /root/.ssh
RUN echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDc6MJhrcyCP5Xxl3GFMDGMLz2iTDIHpyMP0erpaDpo8wleAB9W512hOZchehR3WPCgPsQIiGbFwmBL7exM/HPSa3V+aEFheYafr0vYrgqy4AgQWBbiY69QeURm0oO9rbhxQoWBeOdj/G9uJdIqdfbpRZQ6gpTU+IrClFK3wz7RnpqGtc5lVtkzskP9S6EXfeTaB4b8yyvrf13wO5F1VAD3MfRm5YsaorRO9BQ1Q3Mx/ZD77Q0f4wDFlGtPv1QcRkZs+0EX1S7Ltn/T6hmN02exddBfcS+MY58e2t+eEguOc1/OvHDtmXAMltaq+1jGooNBnXlACIo7dv8LnSwykepTZHB9Z0d34oFT5HXumbNJIYwAsE+U01xDBTFc2wi1y0sd7swBxWZ5f8R9DFwxQOvRv0rpGNTGICzxDGE6KP09odn1qF4nFqRBjPB+OKej0r224OeGSuPaEaR81VSfqGxzSgtGXCmpvMx34WCn8mpEW6F2KyfiKoZR+vwNzBoZQNE= 275262559@qq.com' > /root/.ssh/authorized_keys

# conda clean
RUN conda clean --all

# commands for build
# sudo docker build -t lntzm/pytorch1.13.0-cuda11.6-cudnn8-devel-git-clash:v0.5 .
# sudo docker push lntzm/pytorch1.13.0-cuda11.6-cudnn8-devel-git-clash:v0.5 .