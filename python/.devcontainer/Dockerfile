########################################################################################

# Set the Base Image from a build-arg
ARG BASE_IMAGE=python:3.11.9
FROM ${BASE_IMAGE}
USER root

# Make sure terminal does not wait at a prompt. Set timezone.
ENV TZ=America/New_York DEBIAN_FRONTEND=noninteractive

# Use bash shell
SHELL ["/bin/bash", "-c"]

# Install system packages required for poetry
RUN apt-get update && apt-get install --no-install-recommends -y \
    curl \
    build-essential \
    && rm -rf /var/lib/apt/lists/*

# Install Poetry
ARG POETRY_VERSION=1.8.2
ARG POETRY_HOME="/opt/poetry"
ENV POETRY_HOME=${POETRY_HOME} \
    POETRY_VERSION=${POETRY_VERSION}
RUN curl -sSL https://install.python-poetry.org/ | python3

# Add Poetry to the path
ENV PATH="$POETRY_HOME/bin:$PATH"

# Install system packages required for git
RUN apt-get update && apt-get install --no-install-recommends -y \
    software-properties-common \
    gnupg2 \
    dirmngr \
    lsb-release \
    python3-launchpadlib \
    && rm -rf /var/lib/apt/lists/*

# Add the latest git package
RUN add-apt-repository ppa:git-core/ppa

# Install system packages
RUN apt-get update && apt-get install --no-install-recommends -y \
    sudo \
    aptitude \
    zsh \
    fish \
    tmux \
    git \
    vim \
    neovim \
    nano \
    emacs \
    atop \
    htop \
    tree \
    wget \
    fzf \
    rcm \
    xclip \
    tzdata \
    bat \
    pigz \
    ncdu \
    libicu-dev \
    zip \
    unzip \
    graphviz \
    iputils-ping \
    && rm -rf /var/lib/apt/lists/*

# Read user input
ARG USERNAME=dev
ARG USER_UID=1000
ARG USER_GID=2000

# Create the user with sudo permissions
RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME \
    && usermod -aG sudo $USERNAME \
    && mkdir /home/$USERNAME/workspace \
    && chown -R $USERNAME:$USERNAME /home/$USERNAME/workspace \
    && chown -R $USERNAME:$USERNAME /opt/poetry/

# Default to non-root user with sudo permissions
USER $USERNAME

# Create a folder to store command history so that it can be persisted through container restarts
RUN sudo mkdir /commandhistory \
    && sudo touch /commandhistory/.bash_history \
    && sudo touch /commandhistory/.zsh_history \
    && sudo chown -R $USERNAME:$USERNAME /commandhistory

########################################################################################
