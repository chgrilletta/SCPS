# Installing Docker Desktop on macOS and AlmaLinux 9

This guide provides step-by-step instructions for installing Docker Desktop on macOS Sequoia (15.3.1) and setting up an AlmaLinux 9 container.

## 1. Installing Docker Desktop on macOS

### Steps
1. Download Docker Desktop from the official website [Download Docker Desktop](https://www.docker.com/products/docker-desktop/)

2. Install Docker Desktop:
   - Open the downloaded `.dmg` file.
   - Drag the Docker icon into the `Applications` folder.

3. Launch Docker Desktop:
   - Go to the `Applications` folder and double-click `Docker`.
   - Follow the on-screen instructions to complete the setup.

4. Verify the installation by running the following command in the terminal:
```sh
docker --version
```
If everything is successful, you'll see the Docker version details.

## 2. Installing AlmaLinux 9 in a Container

### Download the AlmaLinux 9 Image
Run the following command to download the official AlmaLinux 9 image from Docker Hub:
```sh
docker pull almalinux:9
```

### Start an AlmaLinux 9 Container
To start an interactive container:
```sh
docker run -it almalinux:9
```

### Verify AlmaLinux 9 Installation
Inside the container run the following command to verify the installation:
```sh
cat /etc/os-release
```
