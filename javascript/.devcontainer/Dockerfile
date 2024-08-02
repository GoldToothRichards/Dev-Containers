########################################################################################

# Set the Base Image from a build-arg
ARG BASE_IMAGE=ubuntu:22.04
FROM ${BASE_IMAGE}
USER root

# Make sure terminal does not wait at a prompt. Set timezone.
ENV TZ=America/New_York DEBIAN_FRONTEND=noninteractive

# This system may have been minimized by removing packages and content that are
# not required on a system that users do not log into.
# To restore this content, including manpages, you can run the 'unminimize'
# command. You will still need to ensure the 'man-db' package is installed.
# This process is slow, so we do a separate step so that it can be cached.
RUN apt-get update && apt-get install --no-install-recommends -y \
    man-db \
    && rm -rf /var/lib/apt/lists/*
RUN yes | unminimize

# Install system packages required for git
RUN apt-get update && apt-get install --no-install-recommends -y \
    software-properties-common \
    gnupg2 \
    dirmngr \
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
    && chmod 0440 /etc/sudoers.d/$USERNAME && \
    usermod -aG sudo $USERNAME && \
    mkdir /home/$USERNAME/workspace && \
    chown -R $USERNAME:$USERNAME /home/$USERNAME/workspace

# Default to non-root user with sudo permissions
USER $USERNAME

# Create a folder to store command history so that it can be persisted through container restarts
RUN sudo mkdir /commandhistory \
    && sudo touch /commandhistory/.bash_history \
    && sudo touch /commandhistory/.zsh_history \
    && sudo chown -R $USERNAME:$USERNAME /commandhistory

########################################################################################