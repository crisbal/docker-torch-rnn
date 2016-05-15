# docker-torch-rnn

Docker images for using torch-rnn (https://github.com/jcjohnson/torch-rnn)

## Available tags

* `crisbal/torch-rnn:base`
    * Based on `ubuntu:14.04`
    * Allows usage of torch-rnn in CPU mode
* `crisbal/torch-rnn:cuda6.5`
    * Based on `nvidia/cuda:6.5` 
    * Allows usage of torch-rnn in GPU mode (Cuda 6.5 support)
    * Only run with nvidia-docker https://github.com/NVIDIA/nvidia-docker
* `crisbal/torch-rnn:cuda7.5`
    * Based on `nvidia/cuda:7.5`
    * Allows usage of torch-rnn in GPU mode (Cuda 7.5 support)
    * Only run with nvidia-docker https://github.com/NVIDIA/nvidia-docker
    
## How to

More details here: https://github.com/jcjohnson/torch-rnn#usage

### CPU Only

1. Start bash in the container
    * `docker run --rm -ti crisbal/torch-rnn:base bash`

2. Preprocess the sample data

    ```
    python scripts/preprocess.py \
    --input_txt data/tiny-shakespeare.txt \
    --output_h5 data/tiny-shakespeare.h5 \
    --output_json data/tiny-shakespeare.json
    ```

3. Train 

    ```
    th train.lua \
    -input_h5 data/tiny-shakespeare.h5 \
    -input_json data/tiny-shakespeare.json \
    -gpu -1
    ```

4. Sample
    * `th sample.lua -checkpoint cv/checkpoint_10000.t7 -length 2000 -gpu -1`

### CUDA

1. Install nvidia-docker
    * https://github.com/NVIDIA/nvidia-docker
2. Start bash in the container
    * `nvidia-docker run --rm -ti crisbal/torch-rnn:cuda7.5 bash`
3. Preprocess the sample data

    ```
    python scripts/preprocess.py \
    --input_txt data/tiny-shakespeare.txt \
    --output_h5 data/tiny-shakespeare.h5 \
    --output_json data/tiny-shakespeare.json
    ```

4. Train 
    
    ```
    th train.lua \
    -input_h5 data/tiny-shakespeare.h5 \
    -input_json data/tiny-shakespeare.json
    ```

5. Sample
    * `th sample.lua -checkpoint cv/checkpoint_10000.t7 -length 2000`
