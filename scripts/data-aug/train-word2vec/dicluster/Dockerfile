# This Dockerfile builds an image based on Ubuntu. It installs the necessary packages, dependencies, and GitHub repository,
# and then runs the word2vec model training pipeline for the designated language. It is NOT meant to be interactive.

# Use the NVIDIA CUDA runtime as a parent image
FROM ubuntu:latest

# Set the working directory in the container
WORKDIR /usr/src/app

# Install the necessary packages
RUN apt-get update && apt-get install -y \
    python3 \
    python3-pip \
    python3-venv \
    git \
    wget \
    bzip2 \
    pv \
    && rm -rf /var/lib/apt/lists/*

# Create and activate the virtual environment
RUN python3 -m venv venv
ENV PATH="/usr/src/app/venv/bin:$PATH"

# Install Python dependencies
RUN pip3 install --no-cache-dir tqdm regex lxml humanize gensim numpy

# Get arguments
ARG LANG

# Set environment variables
ENV LANG=$LANG

# Clone GitHub repo
RUN git clone https://github.com/Rodrigo1771/multilingual-bio-ner-and-el-msc-diss

# Run the training pipeline script
CMD ["bash", "-c", "source venv/bin/activate && cd multilingual-bio-ner-and-el-msc-diss/scripts/data-aug/train-word2vec && ./training_pipeline.sh ${LANG}"]
