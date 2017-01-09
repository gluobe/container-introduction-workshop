# Lab 01 - Installing Docker

The easiest way to install Docker is by running the [https://get.docker.com/](https://get.docker.com/) script:

```
curl -sSL https://get.docker.com/ | sh
```

To verify that the installation was succesfull, run the following command:

```
docker version
```

You should get output similar to the output below:

```
Client:
 Version:      1.12.5
 API version:  1.24
 Go version:   go1.6.4
 Git commit:   7392c3b
 Built:        Fri Dec 16 02:42:17 2016
 OS/Arch:      linux/amd64

Server:
 Version:      1.12.5
 API version:  1.24
 Go version:   go1.6.4
 Git commit:   7392c3b
 Built:        Fri Dec 16 02:42:17 2016
 OS/Arch:      linux/amd64
```

NOTE: ensure that you have Docker Version 1.12 or higher running, otherwise you might run into issues in later labs.
