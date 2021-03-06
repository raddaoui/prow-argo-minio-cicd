FROM ubuntu:xenial

# Never prompt the user for choices on installation/configuration of packages
ENV DEBIAN_FRONTEND=noninteractive
ENV TERM=linux
ENV LC_ALL=C.UTF-8
ENV LANG=C.UTF-8

# gcc & python-dev are needed so we can install crcmod for gsutil
# also includes installations for Python3
RUN set -ex \
    && apt-get update -yqq \
    && apt-get install -yqq --no-install-recommends \
    build-essential \
    curl \
    locales \
    wget \
    ca-certificates \
    git \
    zip \
    unzip \
    gcc \
    ssh \
    jq \
    python-dev \
    python-setuptools \
    python-pip \
    python3-dev \
    python3-setuptools \
    python3-pip \
    && python -V \
    && python3 -V \
    && apt-get clean \
    && rm -rf \
    /var/lib/apt/lists/* \
    /tmp/* \
    /var/tmp/* \
    /usr/share/man \
    /usr/share/doc \
    /usr/share/doc-base

# Install Helm
RUN wget -O /tmp/get_helm.sh \
    https://raw.githubusercontent.com/kubernetes/helm/master/scripts/get && \
    chmod 700 /tmp/get_helm.sh && \
    /tmp/get_helm.sh && \
    rm /tmp/get_helm.sh

# Initialize helm
RUN helm init --client-only


COPY checkout.sh /usr/local/bin
RUN chmod a+x /usr/local/bin/checkout.sh

COPY run_workflows.sh /usr/local/bin
RUN chmod a+x /usr/local/bin/run_workflows.sh

# Install docker.
RUN curl  https://get.docker.com/ | sh

# Install kubectl
RUN  curl -LO https://storage.googleapis.com/kubernetes-release/release/v1.10.0/bin/linux/amd64/kubectl && \
    mv kubectl /usr/local/bin && \
    chmod a+x /usr/local/bin/kubectl

# Install argo
RUN curl -sSL -o /usr/local/bin/argo https://github.com/argoproj/argo/releases/download/v2.1.1/argo-linux-amd64 && \
    chmod +x /usr/local/bin/argo

ENTRYPOINT ["/usr/local/bin/run_workflows.sh"]
