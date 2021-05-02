FROM ubuntu:latest
ARG DEBIAN_FRONTEND="noninteractive"
ARG TZ="Europe/Amsterdam"
RUN \
    apt-get update && \
    apt-get -y install \
        apt-utils \
        apt-transport-https \
        ca-certificates \
        curl \
        openjdk-14-jdk \
        python3.8 \
        python3-openssl \
        software-properties-common \ 
        zip \
        unzip && \
    add-apt-repository ppa:git-core/ppa && \
    apt-get update && \
    apt-get -y install \
        git 
RUN \
    echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | tee /etc/apt/sources.list.d/bazel.list && \
    echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt testing jdk1.8" | tee -a /etc/apt/sources.list.d/bazel.list && \
    curl https://bazel.build/bazel-release.pub.gpg | apt-key add - && \
    apt-get update && \
    apt-get -y install bazel && \
    apt-get -y upgrade bazel
RUN useradd -ms /bin/bash worker && \
    mkdir -p /workspace/src && \
    chown -R worker:worker /workspace
USER worker
# To extract the server
WORKDIR /workspace/src
RUN echo 'startup --output_base=/workspace/.cache' > ~/.bazelrc
RUN bazel version
ENTRYPOINT ["bash"]
