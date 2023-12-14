FROM continuumio/miniconda3
SHELL ["/bin/bash", "-c"]
# RUN sed -i 's/deb.debian.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apt/sources.list
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        less \
        libboost-all-dev \
        make \
        g++ \
    && rm -rf /var/lib/apt/lists/*
RUN mkdir /tools
WORKDIR /tools
COPY environment.yaml .
RUN . /opt/conda/bin/activate && \
    conda env create -n hylight -f environment.yaml && \
    conda clean -ya
RUN git clone https://github.com/kangxiongbin/HyLight.git && \
    cd HyLight/tools/HaploConduct && \
    make clean && \
    make
ENV PATH=/opt/conda/envs/hylight/bin/:$PATH
CMD ["conda", "run", "-n", "hylight"]

# docker build -t hylight .
# # 1. run directly in your path with data
# docker run -v $PWD:/$PWD -w $PWD hylight python /tools/HyLight/script/HyLight.py -l long_reads.fq -s short_reads.fq --nsplit 100 -t 30 -o /home/work/wenhai/hylight/docker/HyLight/example
# # 2. start an interactive docker container session and run in your path with data
# docker run -it --rm -v $PWD:/wd -w /wd -v /var/run/docker.sock:/var/run/docker.sock hylight /bin/bash
# conda activate hylight
# python /tools/HyLight/script/HyLight.py -l long_reads.fq -s short_reads.fq --nsplit 100 -t 30 -o /home/work/wenhai/hylight/docker/HyLight/example