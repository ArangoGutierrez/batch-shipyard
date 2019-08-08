# Torch-GPU
This recipe shows how to run [Torch](http://torch.ch/) on
GPUs using N-series Azure VM instances in an Azure Batch compute pool.

## Configuration
Please see refer to this [set of sample configuration files](./config) for
this recipe.

### Pool Configuration
The pool configuration should enable the following properties:
* `vm_size` must be a GPU enabled VM size. Because Torch is a GPU-accelerated
compute application, you should choose a GPU compute accelerated VM
instance size.
* `vm_configuration` is the VM configuration. Please select an appropriate
`platform_image` with GPU as
[supported by Batch Shipyard](../../docs/25-batch-shipyard-platform-image-support.md).

### Global Configuration
The global configuration should set the following properties:
* `docker_images` array must have a reference to a valid Torch GPU-enabled
Docker image. [alfpark/torch:gpu](https://hub.docker.com/r/alfpark/torch/) can
be used for this recipe.

### Jobs Configuration
The jobs configuration should set the following properties within the `tasks`
array which should have a task definition containing:
* `docker_image` should be the name of the Docker image for this container invocation,
e.g., `alfpark/torch:gpu`
* `command` should contain the command to pass to the Docker run invocation.
For the `alfpark/torch:gpu` Docker image and to run the MNIST convolutional
example on the GPU, the [`run_mnist.sh` helper script](docker/run_mnist.sh) is
used. The `command` should be: `"/root/torch/run_mnist.sh"`
* `gpus` can be set to `all`, however, it is implicitly enabled by Batch
Shipyard when executing on a GPU-enabled compute pool and can be omitted.

## Dockerfile and supplementary files
The `Dockerfile` for the Docker image can be found [here](./docker).

You must agree to the [Torch License](https://github.com/torch/torch7/blob/master/COPYRIGHT.txt)
prior to use.
