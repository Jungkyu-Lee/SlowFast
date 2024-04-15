ARG PYTORCH="1.8.0"
ARG CUDA="11.1"
ARG CUDNN="8"

FROM pytorch/pytorch:${PYTORCH}-cuda${CUDA}-cudnn${CUDNN}-devel

ENV TORCH_CUDA_ARCH_LIST="6.0 6.1 7.0 7.5 8.0 8.6+PTX" \
    TORCH_NVCC_FLAGS="-Xfatbin -compress-all" \
    CMAKE_PREFIX_PATH="$(dirname $(which conda))/../" \
    FORCE_CUDA="1"

RUN rm /etc/apt/sources.list.d/cuda.list \
    && rm /etc/apt/sources.list.d/nvidia-ml.list \
    && apt-key del 7fa2af80 \
    && apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/3bf863cc.pub \
    && apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1804/x86_64/7fa2af80.pub

# (Optional)
# RUN sed -i 's/http:\/\/archive.ubuntu.com\/ubuntu\//http:\/\/mirrors.aliyun.com\/ubuntu\//g' /etc/apt/sources.list && \
#    pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

RUN apt-get update \
    && apt-get install -y ffmpeg libsm6 libxext6 git ninja-build libglib2.0-0 libxrender-dev gcc g++ \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN conda install ffmpeg=4.2

RUN pip install "git+https://github.com/facebookresearch/pytorchvideo.git"

RUN pip install 'git+https://github.com/facebookresearch/fvcore'

RUN pip install simplejson && \
    pip install -U iopath && \
    pip install psutil && \
    pip install opencv-python

RUN conda install tensorboard

RUN pip install scikit-learn

RUN pip install 'git+https://github.com/facebookresearch/fairscale'

RUN pip install cython

RUN pip install -U 'git+https://github.com/facebookresearch/fvcore.git' 'git+https://github.com/cocodataset/cocoapi.git#subdirectory=PythonAPI'
# git에서 Detectron2 소스코드를 복제합니다.
RUN git clone https://github.com/facebookresearch/detectron2 detectron2_repo
# 복제한 디렉토리로 이동하고, 패키지를 설치합니다.
RUN pip install -e detectron2_repo

ENV SKLEARN_ALLOW_DEPRECATED_SKLEARN_PACKAGE_INSTALL=True
RUN git clone https://github.com/facebookresearch/slowfast
ENV PYTHONPATH ./slowfast:$PYTHONPATH
WORKDIR ./slowfast
RUN python setup.py build develop

