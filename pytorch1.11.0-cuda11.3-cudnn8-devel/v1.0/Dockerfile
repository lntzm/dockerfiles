ARG PYTORCH="1.11.0"
ARG CUDA="11.3"
ARG CUDNN="8"
ARG http_proxy="http://172.17.0.1:15224"
ARG https_proxy="http://172.17.0.1:15224"

FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

# To fix GPG key error when running apt-get update
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

RUN apt-get update && apt-get install -y libgl1-mesa-glx libglib2.0-dev openssh-server vim git curl tmux \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# pip install
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" --upgrade pip wheel setuptools
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" h5py termcolor nltk tqdm pandas ftfy regex
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" opencv-python scipy scikit-image scikit-learn pillow matplotlib
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" tensorboardX timm einops omegaconf torchprofile 
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" ffmpeg-python fvcore
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" -U openmim
RUN mim install --no-cache-dir --proxy="http://172.17.0.1:15224" mmcv-full

# choose ssh port
RUN sed -i 's/#Port 22/Port 9999/g' /etc/ssh/sshd_config

# # export PATH
# export $(cat /proc/1/environ |tr '\0' '\n' | xargs) is not a need now.

# export conda in shell
RUN echo '\n' >> /root/.bashrc
RUN echo '# activate conda' >> /root/.bashrc
RUN echo 'source /opt/conda/bin/activate base' >> /root/.bashrc

# add public key
RUN mkdir /root/.ssh
RUN echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQD4wTIb3BtUVnX7CV/rnPluPf+V1yO5QDnfA9phWptdLPyZ4iWx+Ub9Ha7EA8jnhfxyGkL7ySQkCMQVwYQOYnWPaoK4/JXos+05JHSQ151NA7OOYKJZwUZF8DjBsPggZFyJ0SmxCpXfFZaAsfTTW0qWvj6Pl4g+3n2gNpVDSqN30+7rFBCCrvX+yhzL1pYqwhDh1srwrA0pSrTGsCRsXCfPDfYmzP3RSqAgD30IF4cOhC2noJrLibFXikz6txNx42Y4g57iquoJrGeUhHk79mfUMAfk6liMPlNC+0FVeGsZlaY9x4Fc70gDbBpsMdjXjDRRKL/Qr0KrZpPH46OaZ1R8o7w6TrA0VXPerfNlq43DDR5rcxFAxgtTnM5DRMPsGyunPaDSaKpzD1plg3VKCsmgEBc9j/n32O9Vjy2Iazb9j7PbTcWdzk3zzoWOlkN2d+Zrm1NyxcDK56tnsc41Y70wECPsKI4YVWC+uZbfOe3qND1vOdsoUfUcgzWMZvZxM38= lntzm@container' > /root/.ssh/authorized_keys

# run ssh service automatically
RUN echo '\n' >> /root/.bashrc
RUN echo '# start ssh automatically'
RUN echo 'ssh_status=`service ssh status`' >> /root/.bashrc
RUN echo 'if [[ ${ssh_status} =~ "not running" ]]' >> /root/.bashrc
RUN echo 'then' >> /root/.bashrc
RUN echo '    service ssh start' >> /root/.bashrc
RUN echo 'fi' >> /root/.bashrc

# conda clean
RUN conda clean --all

# patch
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" transformers
RUN pip install --no-cache-dir --proxy="http://172.17.0.1:15224" timm==0.4.12