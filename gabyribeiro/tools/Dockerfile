FROM ubuntu:focal
ARG DEBIAN_FRONTEND="noninteractive"
ARG TZ="Europe/Amsterdam"
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# hadolint ignore=DL3008,DL3015
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
        git && \
    echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt stable jdk1.8" | tee /etc/apt/sources.list.d/bazel.list && \
    echo "deb [arch=amd64] http://storage.googleapis.com/bazel-apt testing jdk1.8" | tee -a /etc/apt/sources.list.d/bazel.list && \
    curl https://bazel.build/bazel-release.pub.gpg | apt-key add - && \
    apt-get update && \
    apt-get -y install bazel && \
    apt-get -y upgrade bazel && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    useradd -ms /bin/bash worker && \
    mkdir -p /workspace/src && \
    chown -R worker:worker /workspace
USER worker
# To extract the server
WORKDIR /workspace/src
RUN echo 'startup --output_base=/workspace/.cache' > ~/.bazelrc && \
    bazel version
ENTRYPOINT ["bash"]
