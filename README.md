# win10-nodered-tensorflow
This guide is for using "node-red-contrib-cloud-annotations-gpu"-node to make predictions on images. GPU support requires Windows 10 PC with CUDA compatible NVIDIA GPU. If you skip CUDA installations, Tensorflow should fall back to CPU backend.

After running all commands you should have following versions of the components

| Software      | Version       | 
| ------------- |:-------------:| 
| CUDA          | 11.3.326      |  
| cuDNN         | 8.2.0.53	    | 
| node-red	    | 1.3.4	        |
| node.js       | 16.1.0        |
| tfjs-node-gpu | 3.6.1	        | 
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


### 6. Install @tensorflow/tfjs-node-gpu@3.6.1
```
cd c:\users\<your-user-name>\.node-red
```

```
npm install @tensorflow/tfjs-node-gpu@3.6.1
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
