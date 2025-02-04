# docker image build --build-arg USER_ID=$(id -u) --build-arg GROUP_ID=$(id -g) --file Dockerfile --tag lonce:astyle ../
FROM nvcr.io/nvidia/pytorch:21.06-py3

# create a project directory inside user home
ENV PROJECT_DIR /working
RUN mkdir $PROJECT_DIR
WORKDIR $PROJECT_DIR

#RUN pip install --upgrade --ignore-installed "llvmlite>=0.34.0.dev0,<0.35"
RUN pip install  llvmlite


COPY requirements.txt .
RUN pip install -r requirements.txt

WORKDIR /astyle

# install tifresi (see https://github.com/andimarafioti/tifresi)
RUN apt-get update \
 && DEBIAN_FRONTEND="noninteractive" apt-get -y install tzdata \
 && apt-get install -y libfftw3-dev liblapack-dev cmake 
RUN pip install cython
RUN pip install tifresi


ARG USER_ID
ARG GROUP_ID

RUN addgroup --gid $GROUP_ID user
RUN adduser --disabled-password --gecos '' --uid $USER_ID --gid $GROUP_ID user
USER user

# default command will launch JupyterLab server for development
CMD ["bash"]

# To run - go to your CompareGan directory, run 
# docker run --ipc=host --gpus all -it -v $(pwd):/sonyGan -v /full/path/to/folder/containing/nsynth-train:/mydata foo:bar
# docker run  --ipc=host --gpus "device=0" -it -v $(pwd):/astyle -v /scratch/lonce:/mydata --rm lonce:astyle
# docker run  --ipc=host --gpus "device=0" -it -v $(pwd):/astyle --rm lonce:astyle