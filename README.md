# OWASP Juice Shop - ARMv7 Docker Image

This repository contains the Dockerfile needed to build the OWASP Juice Shop image for ARMv7 architecture. However, you don't need to build the image yourself as it is already available on Docker Hub under `bxbn/juice-shop`.

## Pulling the Pre-built Docker Image

You can easily pull the pre-built ARMv7 image from Docker Hub and run it on your ARMv7 machine (e.g., Raspberry Pi 2 Model B) using the following command:

```bash
docker run --rm -p 0.0.0.0:3000:3000 bxbn/juice-shop
```

This will start the OWASP Juice Shop on port 3000 and you can access it at `http://<your_raspberry_ip>:3000`

### Persisting Data

To persist data across container runs, you can use a volume or bind mount:

#### Bind Mount Example:

```bash
docker run --rm -p 127.0.0.1:3000:3000 \
  -v /path/on/host:/juice-shop/data \
  bxbn/juice-shop:armv7
```

#### Docker Volume Example:

```bash
docker volume create juice_shop_data

docker run --rm -p 127.0.0.1:3000:3000 \
  -v juice_shop_data:/juice-shop/data \
  bxbn/juice-shop:armv7
```

### Building the Docker Image

If you prefer to build the image yourself, especially if you want to customize it, you have two options: building it directly on an ARMv7 machine or using docker buildx to cross-build the image.

#### Option 1: Building on an ARMv7 Machine (Raspberry Pi)

You can build the image directly on an ARMv7 machine like a Raspberry Pi. However, this process can take a lot of time due to limited processing power.

Clone the original juice-shop repo and replace the Dockerfile with the one here then run this command inside the directory where the Dockerfile is located:

```bash
docker build -t <your-image-name>:armv7
```

This will build the image on your ARMv7 device.

#### Option 2: Cross-building with docker buildx
A faster way to build the image is to use docker buildx on a more powerful machine (like your laptop or desktop) to cross-compile the image for ARMv7. Here's how you can do it:

1. Ensure buildx is installed:

```bash
docker buildx version
```

2. Create a new builder instance (if not already created):

```bash
docker buildx create --use
```

3. Build the image for ARMv7:

```bash
docker buildx build --platform linux/arm/v7 -t <your-dockerhub-username>/juice-shop:armv7 --push .
```

This will build and push the image to Docker Hub under the armv7 tag. You can replace <your-dockerhub-username> with your actual Docker Hub username.

### Running the Built Image
After building, you can run the image using the same commands mentioned above:
```bash
docker run --rm -p 127.0.0.1:3000:3000 <your-dockerhub-username>/juice-shop
```

### Official Juice Shop Documentation
For more information about OWASP Juice Shop and its usage, visit the [official documentation](https://owasp-juice.shop/).

