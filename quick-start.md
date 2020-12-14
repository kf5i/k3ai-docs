# Quick Start

## First things First

Start by installing K3Ai, what you **DON'T** need to start:

* A fancy super-duper computer/server with GPU's etc..\(we managed to install everything in a 4 GB RAM laptop\)
* A cluster: don't worry we will take care of it if you don't have anything
* Linux, Mac, Win: we got them all \(and working to support ARM too\)
* Do I need to go through 1000 pages of documentation? Nah just go for the below command and move to Hello-Start

Ready? Let's go, first pick up your flavor, we have a utility script but f for any reason it fails just go straight away to ****[**https://github.com/kf5i/k3ai-core/releases**](https://github.com/kf5i/k3ai-core/releases) and download the binary. Place it in your path and that's it.

**NOTE: Unfortunately not all plugins work with ARM. We will take care of this and make a way to let you know before installing them**

### \*\*\*\*

All you have to do is, download the binary for your Operating System, move it to your path \(if you like easy things\), and use it.

### Linux \(Including Microsoft WSL\)

```text
curl -fL "https://get.k3ai.in" -o k3ai.tar.gz
```

once downloaded untar the file  and move it to your path

```bash
tar -xvzf k3ai.tar.gz \
&& chmod +x ./k3ai \
&& sudo mv ./k3ai /usr/local/bin
```

### Windows

```bash
Invoke-WebRequest -Uri "https://get-win.k3ai.in" -OutFile k3ai.zip
```

once downloaded unzip the file and move it to your path or execute it from a folder of your choice \(i.e.: k3ai.exe -h\)

```bash
 Expand-Archive -Path .\k3ai.zip
```

### Mac

```text
curl -fL "https://get-mac.k3ai.in" -o k3ai.tar.gz
```

once downloaded untar the file  and move it to your path

```bash
tar -xvzf k3ai.tar.gz \
&& chmod +x ./k3ai \
&& sudo mv ./k3ai /usr/local/bin
```

### Arm64

```text
curl -fL "https://get-arm.k3ai.in" -o k3ai.tar.gz
```

once downloaded untar the file  and move it to your path

```bash
tar -xvzf k3ai.tar.gz \
&& chmod +x ./k3ai \
&& sudo mv ./k3ai /usr/local/bin
```

### Alternative method for Linux

If for any reason it fails just go straight away to [https://github.com/kf5i/k3ai-core/releases](https://github.com/kf5i/k3ai-core/releases) and download the binary. Place it in your path and that's it.

 or use the following

```bash
#Set a variable to grab latest version
Version=$(curl -s "https://api.github.com/repos/kf5i/k3ai-core/releases/latest" | awk -F '"' '/tag_name/{print $4}' | cut -c 2-6) 
# get the binaries
wget https://github.com/kf5i/k3ai-core/releases/download/v$Version/k3ai-core_${Version}_linux_amd64.tar.gz
```

### **Note: sometimes things take longer than expected resulting in the error below:**

```text
error: timed out waiting for the condition on xxxxxxx
```

Don't worry! Sometimes the installation takes a few minutes, especially the Vagrant version or if you have limited bandwidth.

Errors in the utility script? Use this \(on Linux\)

```bash
#Set a variable to grab latest version
Version=$(curl -s "https://api.github.com/repos/kf5i/k3ai-core/releases/latest" | awk -F '"' '/tag_name/{print $4}' | cut -c 2-6) 
# get the binaries
wget https://github.com/kf5i/k3ai-core/releases/download/v$Version/k3ai-core_${Version}_linux_amd64.tar.gz
```

