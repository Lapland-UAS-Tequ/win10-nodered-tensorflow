# win10-nodered-tensorflow
This guide is for preparing your Windows 10 machine for computer vision in Node-RED with "tfjs-node-gpu" and node-canvas modules. GPU support requires Windows 10 PC with CUDA compatible NVIDIA GPU. If you skip CUDA installations, Tensorflow should fall back to CPU backend. 

After running all commands you should have following versions of the components and be able to make predictions on images.

| Software      | Version       | 
| ------------- |:-------------:| 
| CUDA          | 11.3.326      |  
| cuDNN         | 8.2.0.53	    | 
| node-red	    | 2.0.4	        |
| node.js       | 16.1.0        |
| tfjs-node-gpu | 3.8.0	        | 
| node-red-contrib-cloud-annotations-gpu | 0.0.5 |

PC setup which I have used:

Dell Laptop
Intel Core i7-8850H @ 2.60 GHz

Windows 10

16 GB RAM

NVIDIA Quadro P600


## Installation


### 1. Download CUDA 11 Toolkit.

Download CUDA 11 toolkit and run installer.

https://tequ-win10-nodered-tensorflow.s3.eu.cloud-object-storage.appdomain.cloud/cuda_11.3.0_465.89_win10.exe


### 2. Download and unzip cuDNN 8.

Download cuDNN 8 package and install files.

https://tequ-win10-nodered-tensorflow.s3.eu.cloud-object-storage.appdomain.cloud/cudnn-11.3-windows-x64-v8.2.0.53.zip

Copy extracted files to CUDA Toolkit installation folder following the same folder structure.

Copy extracted files in folder ```Cuda\bin``` to ```C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.3\bin```

Copy extracted files in folder ```Cuda\lib``` to ```C::\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.3\lib```

Copy extracted files in folder ```Cuda\include``` to ```C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.3\include```


You could also setup environment variables to point the location of cuDNN files to make things work.


### 3. Download and install cusolver64_10.dll if its missing

https://tequ-win10-nodered-tensorflow.s3.eu.cloud-object-storage.appdomain.cloud/cusolver64_10.dll

Copy ```cusolver64_10.dll``` to ```C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.3\bin```


### 4. Download and install Node.js

https://tequ-win10-nodered-tensorflow.s3.eu.cloud-object-storage.appdomain.cloud/node-v16.1.0-x64.msi

**Install with all options and addons.**


### 5. Install Node-RED

```
npm install -g --unsafe-perm node-red
```


### 6. Install @tensorflow/tfjs-node-gpu@3.8.0
```
cd c:\users\<your-user-name>\.node-red
```

```
npm install @tensorflow/tfjs-node-gpu@3.8.0
```


### 7. Install node-red-contrib-cloud-annotations-gpu

```
cd c:\users\<your-user-name>\.node-red
```

```
npm install node-red-contrib-cloud-annotations-gpu
```


### 8. Remove "node_modules"-folder from  

```
C:\Users\<your-user-name>\.node-red\node_modules\@cloud-annotations\models-node-gpu
```

***If you install new nodes you need to remove "node_modules" again after every installation.***


### 9. Test your setup

```
cd c:\users\<your-user-name>\.node-red
```

```
node
```

```
var tf = require('@tensorflow/tfjs-node-gpu')
```

You should see something like this:

![alt text](
https://github.com/juhaautioniemi/win10-nodered-tensorflow/blob/master/images/node_test.JPG "Node-RED log")

### 12. Start Node-RED 

### 13. node-red-contrib-cloud-annotations-gpu is ready to use

First inference has slow start and it takes something like ~1-5 seconds. After that it should run smoothly.

-----------------
-----------------
-----------------

## Installing node-canvas for fast annotation and image processing

### 1. Download and install libjpeg-turbo 64bit to folder C:\libjpeg-turbo64

https://sourceforge.net/projects/libjpeg-turbo/files/


### 2. Download and install GTK 2 64bit to folder C:\GTK

http://ftp.gnome.org/pub/GNOME/binaries/win64/gtk+/2.22/gtk+-bundle_2.22.1-20101229_win64.zip

### 3. Execute following commands

```
cd c:\users\<your-user-name>\.node-red
```

```
npm install node-canvas 
```

```
cd c:\users\<your-user-name>\.node-red\node_modules\canvas
```

```
node-gyp configure
```

```
node-gyp build
```

### 4. Node-canvas is ready to use in function-node.
