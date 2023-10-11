FROM ubuntu:22.04
LABEL maintainer="haohu9753"
LABEL repository="llm"

RUN apt update && \
    apt install -y bash \
                   build-essential \
                   git \
                   curl \
                   wget \
                   vim \
                   bzip2 \
                   ca-certificates \
                   python3 \
                   python3-pip && \
    rm -rf /var/lib/apt/lists

# RUN python3 -m pip install --no-cache-dir --upgrade pip && \
#     python3 -m pip install --no-cache-dir \
#     jupyter 


RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O miniconda.sh && \
    bash miniconda.sh -b -u -p /opt/conda && \
    rm -rf miniconda.sh
    # opt/conda/bin/conda init bash

# 将 conda 添加到 PATH
ENV PATH /opt/conda/bin:$PATH

# 设置镜像源为阿里云的镜像 并  创建 gpt 虚拟环境
RUN conda create -n gpt python=3.10 && \
    echo "[global]\n\
index-url = https://pypi.tuna.tsinghua.edu.cn/simple/" > /opt/conda/envs/gpt/pip.conf


# Make RUN commands use the new environment:
SHELL ["conda", "run", "--no-capture-output", "-n", "gpt", "/bin/bash", "-c"]

# The code to run when container is started:
# ENTRYPOINT ["conda", "run", "-n", "myenv"]

# 安装 torch 和 LLaMA-Efficient-Tuning所需得一些环境
COPY requirements.txt .
RUN conda clean -a && \
    pip3 install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu && \
    pip install --no-cache-dir -r requirements.txt 

CMD ["/bin/bash"]