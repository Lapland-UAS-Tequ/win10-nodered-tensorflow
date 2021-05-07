# win10-nodered-tensorflow
This guide is for using "node-red-contrib-cloud-annotations-gpu"-node to make predictions on images. This setup requires Windows 10 PC with NVIDIA GPU. If you skip CUDA installations, Tensorflow should fall back to CPU.

After running all commands you should have following versions of the components

| Software      | Version       | 
| ------------- |:-------------:| 
| CUDA          | 11.3.326      |  
| cuDNN         | 8.2.0.53	    | 
| node-red	    | 1.3.4	        |
| node.js       | 16.1.0        |
| tfjs-node-gpu | 3.6.1	        | 
| node-red-contrib-cloud-annotations-gpu | 0.0.5 |

This guide has been tested with following PC-setup:

Intel i7
Windows 10
NVIDIA Quadro P600

## Installation

1. Download and CUDA 11 Toolkit.

https://tequ-win10-nodered-tensorflow.s3.eu.cloud-object-storage.appdomain.cloud/cuda_11.3.0_465.89_win10.exe

2. Download and unzip cuDNN 8.

https://tequ-win10-nodered-tensorflow.s3.eu.cloud-object-storage.appdomain.cloud/cudnn-11.3-windows-x64-v8.2.0.53.zip

Copy extracted files to CUDA Toolkit installation folder following the same folder structure.

Copy extracted files in folder Cuda\bin to C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.3\bin
Copy extracted files in folder Cuda\lib to C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.3\lib
Copy extracted files in folder Cuda\include to C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.3\include

You could also setup environment variables to point the location of cuDNN files to make things work.

3. Download and install cusolver64_10.dll if its missing

https://tequ-win10-nodered-tensorflow.s3.eu.cloud-object-storage.appdomain.cloud/cusolver64_10.dll

Copy cusolver64_10.dll to C:\Program Files\NVIDIA GPU Computing Toolkit\CUDA\v11.3\bin

4. Download and install Node.js

https://tequ-win10-nodered-tensorflow.s3.eu.cloud-object-storage.appdomain.cloud/node-v16.1.0-x64.msi

5. Install windows-build-tools using command line (run as administrator)

```npm --add-python-to-path='true' --debug install --global windows-build-tools```

Add path "C:\Users\<your-user-name>\.windows-build-tools\python27 into environment varibles PYTHON & PATH

6. Install node-gyp

```npm install -g node-gyp```

7. Install Node-RED

```npm install -g --unsafe-perm node-red```

8. Install @tensorflow@tfjs-node-gpu@3.6.1

```npm install @tensorflow@tfjs-node-gpu@3.6.1```

9. Install node-red-contrib-cloud-annotations-gpu

```cd c:\users\<your-user-name>\.node-red```

```npm install node-red-contrib-cloud-annotations-gpu```

10. Remove "node_modules"-folder from  

```C:\Users\<your-user-name>\.node-red\node_modules\@cloud-annotations\models-node-gpu```

11. Test your setup

```cd c:\users\<your-user-name>\.node-red```

```node```

```var tf = require('@tensorflow/tfjs-node-gpu')```

You should see something like this:

![alt text](
https://github.com/juhaautioniemi/win10-nodered-tensorflow/blob/master/images/node_test.JPG "Node-RED log")

13. Start Node-RED 
14. node-red-contrib-cloud-annotations-gpu is ready to use

First inference has slow start and it takes something like ~1-5 seconds. After that it should run smoothly.
