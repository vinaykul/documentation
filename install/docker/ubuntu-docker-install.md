# Install Docker for Kata Containers on Ubuntu

> **Note:**
>
> - This guide assumes you have
>   [already installed the Kata Containers packages](../ubuntu-installation-guide.md).
>
> - If you do not want to copy or type all these instructions by hand, you can use the
>   [`kata-manager`](https://github.com/kata-containers/tests/blob/master/cmd/kata-manager/kata-manager.sh)
>   script to install the packaged system including your chosen container
>   manager. Alternatively, you can generate a runnable shell script from
>   individual documents using the
>   [`kata-doc-to-script`](https://github.com/kata-containers/tests/blob/master/.ci/kata-doc-to-script.sh) script.

1. Install the latest version of Docker with the following commands:

   > **Note:** This step is only required if Docker is not installed on the system.

   ```bash
   $ sudo -E apt-get -y install apt-transport-https ca-certificates software-properties-common
   $ curl -sL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
   $ arch=$(dpkg --print-architecture)
   $ sudo -E add-apt-repository "deb [arch=${arch}] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
   $ sudo -E apt-get update
   $ sudo -E apt-get -y install docker-ce
   ```

   For more information on installing Docker please refer to the
   [Docker Guide](https://docs.docker.com/engine/installation/linux/ubuntu).

2. Configure Docker to use Kata Containers by default with the following commands:

   ```bash
   $ sudo mkdir -p /etc/systemd/system/docker.service.d/
   $ cat <<EOF | sudo tee /etc/systemd/system/docker.service.d/kata-containers.conf
   [Service]
   ExecStart=
   ExecStart=/usr/bin/dockerd -D --add-runtime kata-runtime=/usr/bin/kata-runtime --default-runtime=kata-runtime
   EOF
   ```

3. Restart the Docker systemd service with the following commands:

   ```bash
   $ sudo systemctl daemon-reload
   $ sudo systemctl restart docker
   ```

4. Run Kata Containers

   You are now ready to run Kata Containers:

   ```bash
   $ sudo docker run busybox uname -a
   ```

   The previous command shows details of the kernel version running inside the
   container, which is different to the host kernel version.
