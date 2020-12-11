# Quick Start

## First things First

Start by installing K3Ai, what you **DON'T** need to start:

* A fancy super-duper computer/server with GPU's etc..\(we managed to install everything in a 4 GB RAM laptop\)
* A cluster: don't worry we will take care of it if you don't have anything
* Linux, Mac, Win: we got them all \(and working to support ARM too\)
* Do I need to go through 1000 pages of documentation? Nah just go for the below command and move to Hello-Start

Ready? Let's go, first pick up your flavor, we have a utility script but f for any reason it fails just go straight away to ****[**https://github.com/kf5i/k3ai-core/releases**](https://github.com/kf5i/k3ai-core/releases) and download the binary. Place it in your path and that's it.

### **on Linux \(including Windows Subsystem for Linux\)**

```text
curl -sfL https://get-core.k3ai.in | bash -
```

### **on Windows**

```text
. { iwr -useb https://get-core.k3ai.in  } | iex;
```

### **Mac?** 

go to the latest release and grab your own k3ai cli. If you want this automated please open an [**issue**](https://github.com/kf5i/k3ai-core/issues/new/choose)\*\*\*\*

```bash
https://github.com/kf5i/k3ai-core/releases
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

