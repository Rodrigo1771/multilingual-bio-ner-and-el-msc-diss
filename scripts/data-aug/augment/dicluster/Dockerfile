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
    && rm -rf /var/lib/apt/lists/*

# Create and activate the virtual environment
RUN python3 -m venv venv
ENV PATH="/usr/src/app/venv/bin:$PATH"

# Install Python dependencies
RUN pip3 install --no-cache-dir tqdm humanize gensim

# Get arguments
ARG TASK
ARG ARG
ARG MODEL_TYPE

# Set environment variables
ENV TASK=$TASK
ENV ARG=$ARG
ENV MODEL_TYPE=$MODEL_TYPE

# Clone GitHub repo
RUN git clone https://github.com/Rodrigo1771/multilingual-bio-ner-and-el-msc-diss

# Copy models to container
COPY models/word2vec /usr/src/app/multilingual-bio-ner-and-el-msc-diss/scripts/data-aug/train-word2vec/out
COPY models/fasttext /usr/src/app/multilingual-bio-ner-and-el-msc-diss/scripts/data-aug/fasttext-models

# Run the training script
CMD ["bash", "-c", "source venv/bin/activate && cd multilingual-bio-ner-and-el-msc-diss/scripts/data-aug/augment/${TASK} && ./augment_${TASK}_data.sh ${ARG} ${MODEL_TYPE}"]
