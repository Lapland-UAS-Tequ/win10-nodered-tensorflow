This repository is developed in Fish-IoT project

https://www.tequ.fi/en/project-bank/fish-iot/ 

---

# win10-nodered-tensorflow
This guide is for preparing your Windows 10 machine for computer vision in Node-RED with "tfjs-node-gpu" and canvas modules. GPU support requires Windows 10 PC with CUDA compatible NVIDIA GPU. If you skip CUDA installations, Tensorflow should fall back to CPU backend. 


After running all commands you should have following versions of the components and be able to make predictions on images.

| Software      | Version       | Link |
| ------------- |:-------------:| :-------------:| 
| CUDA          | 11.6.0_511.23 | https://developer.nvidia.com/cuda-downloads |
| cuDNN         | 8.3.2.44	    | https://developer.nvidia.com/cudnn |
| node-red	    | 3.0.2	        | https://www.npmjs.com/package/node-red |
| node.js       | 16.13.2       | https://nodejs.org/en/|
| tfjs-node-gpu | 3.13.0	      | https://www.npmjs.com/package/@tensorflow/tfjs-node-gpu |
| canvas        | 2.8.0         | https://www.npmjs.com/package/canvas |

PC setups which I have successfully used:

Dell Laptop, Intel Core i7-8850H @ 2.60 GHz, Windows 10, 16 GB RAM, NVIDIA Quadro P600 (511.09)

Dell Laptop,Intel Core i7-11850H @ 2.50 GHz,Windows 10, 32 GB RAM, NVIDIA RTX A3000 Laptop GPU (522.06)


# Installation

## 1. Download CUDA 11 Toolkit.

Download CUDA 11 toolkit and run installer.

https://tequ-files.s3.eu.cloud-object-storage.appdomain.cloud/cuda_11.6.0_511.23_windows.exe


## 2. Download and unzip cuDNN 8.

Download cuDNN 8 package and install files.

https://tequ-files.s3.eu.cloud-object-storage.appdomain.cloud/cudnn_8.3.2.44_windows.exe


Add following paths to PATH environment variable:

```
C:\Program Files\NVIDIA\CUDNN\v8.3\bin
```

```
C:\Program Files\NVIDIA\CUDNN\v8.3\lib\x64
```


Download ZLIB DLL

https://tequ-files.s3.eu.cloud-object-storage.appdomain.cloud/zlib123dllx64.zip

Extract to C:\zlib123dllx64

Add following path to PATH environment variable

```
C:\zlib123dllx64\dll_x64
```

## 3. Download and install Node.js

https://tequ-files.s3.eu.cloud-object-storage.appdomain.cloud/node-v16.13.2-x64.msi

**Install with all options and addons.**


## 4. Install Node-RED

```
npm install -g --unsafe-perm node-red
```


## 5. Install @tensorflow/tfjs-node-gpu@3.13.0
```
cd c:\users\<your-user-name>\.node-red
```

```
npm install @tensorflow/tfjs-node-gpu@3.13.0
```

## 6. Test your setup

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
https://github.com/Lapland-UAS-Tequ/win10-nodered-tensorflow/blob/master/images/node_test.JPG "Node-RED log")

## 8. Install canvas for fast annotation and image processing

Download and install libjpeg-turbo-2.1.2-vc64.exe (or newer) to folder C:\libjpeg-turbo64

https://sourceforge.net/projects/libjpeg-turbo/files/

Download and install GTK 2 64bit to folder C:\GTK

http://ftp.gnome.org/pub/GNOME/binaries/win64/gtk+/2.22/gtk+-bundle_2.22.1-20101229_win64.zip

Execute following commands

```
cd c:\users\<your-user-name>\.node-red
```

```
npm install canvas 
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

### 9. Start Node-RED 

Start Node-RED from command line or configure Node-RED to start at boot

https://nodered.org/docs/faq/starting-node-red-on-boot


# Use Tensorflow in Node-RED

This example uses SSD MobileNet v2 320x320 model from TensorFlow 2 Detection Model Zoo. Model is used directly in savedemodel format.

https://github.com/tensorflow/models/blob/master/research/object_detection/g3doc/tf2_detection_zoo.md

### 1. Install dependencies

```
cd /.node-red
```

```
npm install node-red-contrib-image-info
```

```
npm install node-red-node-exif
```

```
npm install node-red-contrib-browser-utils
```

```
npm install node-red-contrib-image-output
```

```
node-red-start
```


### 2. Download object detection model

Download example model and unzip to /.node-red/savedmodel

```
https://tequ-files.s3.eu.cloud-object-storage.appdomain.cloud/example_savedmodel.zip
```

### 3. Import and deploy example flow

Screenshot after example flow is deployed and image with two dogs is injected to flow.

![alt text](
https://github.com/Lapland-UAS-Tequ/win10-nodered-tensorflow/blob/master/images/example-nodered-flow.JPG "Example flow")

Copy and import this flow into your Node-RED workspace.

```
[{"id":"83a7a965.1808a8","type":"subflow","name":"[IMG] Annotate","info":"","category":"Tequ-API Client","in":[{"x":120,"y":140,"wires":[{"id":"d05bfd8e.a02e"}]}],"out":[{"x":1080,"y":140,"wires":[{"id":"4e5f5c6c.bcf214","port":0}]}],"env":[{"name":"box_colors","type":"json","value":"{\"fish\":\"#FFFFFF\",\"pike\":\"#006400\",\"perch\":\"#008000\",\"smolt\":\"#ADD8E6\",\"salmon\":\"#0000FF\",\"trout\":\"#0000FF\",\"cyprinidae\":\"#808080\",\"zander\":\"#009000\",\"bream\":\"#008800\"}","ui":{"type":"input","opts":{"types":["json"]}}},{"name":"image_settings","type":"json","value":"{\"quality\":0.8}","ui":{"type":"input","opts":{"types":["json"]}}},{"name":"image_type","type":"str","value":"image/jpeg","ui":{"type":"select","opts":{"opts":[{"l":{"en-US":"JPG"},"v":"image/jpeg"},{"l":{"en-US":"PNG"},"v":"image/png"}]}}},{"name":"bbox_lineWidth","type":"num","value":"5","ui":{"type":"spinner","opts":{"min":0,"max":10}}},{"name":"bbox_text_color","type":"str","value":"white","ui":{"type":"select","opts":{"opts":[{"l":{"en-US":"white"},"v":"white"},{"l":{"en-US":"black"},"v":"black"},{"l":{"en-US":"blue"},"v":"blue"},{"l":{"en-US":"green"},"v":"green"},{"l":{"en-US":"yellow"},"v":"yellow"},{"l":{"en-US":"red"},"v":"red"},{"l":{"en-US":"orange"},"v":"orange"}]}}},{"name":"bbox_font","type":"str","value":"30px Arial","ui":{"type":"select","opts":{"opts":[{"l":{"en-US":"5px Arial"},"v":"5 px Arial"},{"l":{"en-US":"10px Arial"},"v":"10px Arial"},{"l":{"en-US":"15px Arial"},"v":"15px Arial"},{"l":{"en-US":"20px Arial"},"v":"20px Arial"},{"l":{"en-US":"25px Arial"},"v":"25px Arial"},{"l":{"en-US":"30px Arial"},"v":"30px Arial"},{"l":{"en-US":"35px Arial"},"v":"35px Arial"},{"l":{"en-US":"40px Arial"},"v":"40px Arial"},{"l":{"en-US":"45px Arial"},"v":"45px Arial"},{"l":{"en-US":"50px Arial"},"v":"50px Arial"}]}}},{"name":"label_offset_x","type":"num","value":"0","ui":{"type":"input","opts":{"types":["num"]}}},{"name":"label_offset_y","type":"num","value":"30","ui":{"type":"input","opts":{"types":["num"]}}},{"name":"threshold","type":"num","value":"0.75","ui":{"type":"spinner","opts":{"min":0,"max":1}}},{"name":"labels","type":"json","value":"[\"fish\",\"perch\", \"pike\", \"rainbow trout\", \"salmon\", \"trout\", \"cyprinidae\", \"zander\", \"smolt\", \"bream\"]","ui":{"type":"input","opts":{"types":["json"]}}}],"meta":{"module":"[IMG] Annotate","version":"0.0.1","author":"juha.autioniemi@lapinamk.fi","desc":"Annotates prediction results from [AI] Detect subflows.","license":"MIT"},"color":"#87A980","icon":"font-awesome/fa-pencil-square-o","status":{"x":1080,"y":280,"wires":[{"id":"7fd4f6bf24348b12","port":0}]}},{"id":"c19ac6bd.2a9d08","type":"function","z":"83a7a965.1808a8","name":"Annotate with  canvas","func":"const img = msg.payload.image.buffer;\nconst image_type = env.get(\"image_type\");\nconst image_settings = env.get(\"image_settings\");\nconst bbox_lineWidth = env.get(\"bbox_lineWidth\");\nconst bbox_text_color = env.get(\"bbox_text_color\");\nconst label_offset_x = env.get(\"label_offset_x\");\nconst label_offset_y = env.get(\"label_offset_y\");\nconst bbox_font = env.get(\"bbox_font\");\nconst COLORS = env.get(\"box_colors\");\nconst objects = msg.payload.inference.result\nconst labels = env.get(\"labels\")\n\n//Define threshold\nlet threshold = 0;\n\nconst global_settings = global.get(\"settings\") || undefined\nlet thresholdType = \"\"\n\nif(global_settings !== undefined){\n    if(\"threshold\" in global_settings){\n        threshold = global_settings[\"threshold\"]\n        thresholdType = \"global\";\n    }\n}\n\nelse if(\"threshold\" in msg){\n    threshold = msg.threshold;\n    thresholdType = \"msg\";\n    if(threshold < 0){\n        threshold = 0\n    }\n    else if(threshold > 1){\n        threshold = 1\n    }\n}\n\nelse{\n    threshold = env.get(\"threshold\");\n    thresholdType = \"env\";\n}\n\nmsg.thresholdUsed = threshold;\nmsg.thresholdTypeUsed = thresholdType;\n\nasync function annotateImage(image) {\n  const localImage = await canvas.loadImage(image);  \n  const cvs = canvas.createCanvas(localImage.width, localImage.height);\n  const ctx = cvs.getContext('2d');  \n  ctx.drawImage(localImage, 0, 0); \n  \n  objects.forEach((obj) => {\n        if(labels.includes(obj.class) && obj.score >= threshold){\n            let [x, y, w, h] = obj.bbox;\n            ctx.lineWidth = bbox_lineWidth;\n            ctx.strokeStyle = COLORS[obj.class];\n            ctx.strokeRect(x, y, w, h);\n            ctx.fillStyle = bbox_text_color;\n            ctx.font = bbox_font;\n            ctx.fillText(obj.class+\" \"+Math.round(obj.score*100)+\" %\",x+label_offset_x,y+label_offset_y);\n        }\n      });\n  \n  return cvs.toBuffer(image_type, image_settings);\n}\n\nif(objects.length > 0){\n    msg.annotated_image = await annotateImage(img)\n    //node.done()\n    msg.objects_found = true\n}\nelse{\n    msg.objects_found = false\n}\n\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[{"var":"canvas","module":"canvas"}],"x":440,"y":140,"wires":[["a801355d.9f7ac8"]]},{"id":"d05bfd8e.a02e","type":"change","z":"83a7a965.1808a8","name":"timer","rules":[{"t":"set","p":"start","pt":"msg","to":"","tot":"date"}],"action":"","property":"","from":"","to":"","reg":false,"x":230,"y":140,"wires":[["c19ac6bd.2a9d08"]]},{"id":"a801355d.9f7ac8","type":"change","z":"83a7a965.1808a8","name":"end timer","rules":[{"t":"set","p":"payload.annotation.time_ms","pt":"msg","to":"$millis() - msg.start","tot":"jsonata"},{"t":"set","p":"payload.annotation.buffer","pt":"msg","to":"annotated_image","tot":"msg"},{"t":"set","p":"payload.annotation.objects_found","pt":"msg","to":"objects_found","tot":"msg"},{"t":"delete","p":"annotated_image","pt":"msg"},{"t":"delete","p":"start","pt":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":640,"y":140,"wires":[["4e5f5c6c.bcf214","c20a6448.e6f218"]]},{"id":"4e5f5c6c.bcf214","type":"change","z":"83a7a965.1808a8","name":"delete useless","rules":[{"t":"delete","p":"annotated_image","pt":"msg"},{"t":"delete","p":"start","pt":"msg"},{"t":"delete","p":"objects_found","pt":"msg"}],"action":"","property":"","from":"","to":"","reg":false,"x":880,"y":140,"wires":[[]]},{"id":"c20a6448.e6f218","type":"switch","z":"83a7a965.1808a8","name":"objects found?","property":"objects_found","propertyType":"msg","rules":[{"t":"true"},{"t":"false"}],"checkall":"true","repair":false,"outputs":2,"x":660,"y":200,"wires":[["a9379cd1321a02da"],["0ec56ca8f000a540"]]},{"id":"a9379cd1321a02da","type":"function","z":"83a7a965.1808a8","name":"","func":"node.status({fill:\"green\",shape:\"dot\",text:msg.thresholdTypeUsed+\" \"+msg.thresholdUsed+\" in \"+msg.payload.annotation.time_ms+\" ms\"})","outputs":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":860,"y":180,"wires":[]},{"id":"0ec56ca8f000a540","type":"function","z":"83a7a965.1808a8","name":"","func":"node.status({fill:\"green\",shape:\"dot\",text:msg.thresholdTypeUsed+\" \"+msg.thresholdUsed+\" No objects to annotate\"})","outputs":0,"noerr":0,"initialize":"","finalize":"","libs":[],"x":860,"y":220,"wires":[]},{"id":"7fd4f6bf24348b12","type":"status","z":"83a7a965.1808a8","name":"","scope":null,"x":860,"y":280,"wires":[[]]},{"id":"1261d76017c1af2f","type":"subflow","name":"[AI] Detect-sm","info":"Make prediction on image with Tensorflow saved model trained with tequ-tf2-ca-training-pipeline.\n\nInput image must be image buffer in **'msg.payload'**.\n\nModel is loaded from configured folder.\n\nInference image and add result to output message. \n\nCalculates approximation of length in centimeters of detected object(s) based on given **image_width_cm**. \n\nParameter **image_width_cm** can be set in 'settings.js'-file separately for each msg.topic (datasource id).\n\nFor example:\n\n`process.env.image_width_cm = JSON.stringify({\"10\":130,\"11\":130,\"20\":130,\"21\":130});`\n\n`{\n    { msg.topic:image width [cm] },\n    { msg.topic:image width [cm] }\n}`\n\n\nBasic image info and exif is added to output message, if available.\n\nTo train a model, please look:\n\nhttps://github.com/juhaautioniemi/tequ-tf2-ca-training-pipeline\n","category":"Tequ-API Client","in":[{"x":100,"y":100,"wires":[{"id":"feaddfb1e7b83060"}]}],"out":[{"x":1020,"y":180,"wires":[{"id":"af7ab0534150089f","port":0}]},{"x":1020,"y":300,"wires":[{"id":"af7ab0534150089f","port":1}]},{"x":1020,"y":380,"wires":[{"id":"af7ab0534150089f","port":2}]}],"env":[{"name":"model_folder","type":"str","value":"savedmodel","ui":{"type":"input","opts":{"types":["str","env"]}}},{"name":"threshold","type":"num","value":"0.75","ui":{"type":"spinner","opts":{"min":0,"max":1}}},{"name":"image_width_cm","type":"env","value":"image_width_cm","ui":{"type":"input","opts":{"types":["env"]}}}],"meta":{"module":"node-red-tequ-ai-detect-sm","version":"0.0.1","author":"juha.autioniemi@lapinamk.fi","desc":"Run prediction on input image using TF2 Savedmodel.","license":"MIT"},"color":"#FFCC66","inputLabels":["msg.payload (image buffer)"],"outputLabels":["result","metagraph","tensorflow info"],"icon":"node-red/status.svg","status":{"x":1020,"y":240,"wires":[{"id":"450407a5f0871af6","port":0}]}},{"id":"af7ab0534150089f","type":"function","z":"1261d76017c1af2f","name":"Predict saved model","func":"const savedmodel = context.get(\"savedmodel\")\nconst imageBuffer = msg.payload;\nlet results = [];\nconst labels = context.get(\"labels\");\nconst threshold = msg.threshold;\nconst image_width = msg.width;\nconst image_height = msg.height;\n\n//node.warn(labels)\n\nfunction detect(input){\n    return tf.tidy(() => {\n        const inputTensor = tf.node.decodeImage(input, 3).expandDims(0);  \n        const outputTensor =  savedmodel.predict({input_tensor:inputTensor});\n        const scores = outputTensor['detection_scores'].arraySync();\n        const boxes = outputTensor['detection_boxes'].arraySync();\n        const names = outputTensor['detection_classes'].arraySync();\n        \n        //node.warn(outputTensor)\n        //node.warn(scores)\n        //node.warn(boxes)\n        //node.warn(names)\n        \n        for (let i = 0; i < scores[0].length; i++) {\n            try{\n                if (scores[0][i] > threshold) {\n                    newObject = {\n                        \"bbox\":[\n                            boxes[0][i][1] * image_width,\n                            boxes[0][i][0] * image_height,\n                            (boxes[0][i][3] - boxes[0][i][1]) * image_width,\n                            (boxes[0][i][2] - boxes[0][i][0]) * image_height\n                            ],\n                        \"class\":labels[names[0][i]-1],\n                        \"label\":labels[names[0][i]-1],\n                        \"score\":scores[0][i],\n                        \"length_cm\":NaN\n                    }\n                    results.push(newObject)\n                }\n            }\n            catch(error){\n                node.warn(error)\n            }\n        }\n        \n        //Calculate object width if image_width_cm is given input message\n        if(\"image_width_cm\" in msg){\n            const image_width_cm = msg.image_width_cm;    \n            for(let j=0;j<results.length;j++){\n                px_in_cm = image_width_cm / msg.width\n                object_size_cm = px_in_cm * results[j].bbox[2]\n                results[j].length_cm = Math.round(object_size_cm)\n            }\n        }\n        \n        // Create output message\n        let result_message = {\n            \"labels\":context.get(\"labels\"),\n            \"thresholdType\":msg.thresholdType,\n            \"threshold\": msg.threshold,\n            \"image_width_cm\":msg.image_width_cm,\n            \"image_width_cm_type\":msg.image_width_cm_type,\n            \"topic\":msg.topic,\n            \"payload\":{\n                \"inference\":{\n                    \"metadata\":context.get(\"metadata\"),\n                    \"time_ms\": new Date().getTime() - msg.start,\n                    \"validated\":false,\n                    \"result\":results,\n                    \"type\":\"object detection\"\n                },\n                \"image\":{\n                    \"buffer\":imageBuffer,\n                    \"width\": msg.width,\n                    \"height\": msg.height,\n                    \"type\": msg.type,\n                    \"size\": (imageBuffer).length,\n                    \"exif\":{}\n                }\n            }\n        }\n\n        // Add exif information\n        if(msg.exif){\n             result_message.payload.image.exif = msg.exif\n        }\n        \n        node.status({fill:\"blue\",shape:\"dot\",text:(result_message.payload.inference.result).length+\" object(s) found in \"+ result_message.payload.inference.time_ms+\" ms\"});  \n        return result_message;\n    });\n}\n\nreturn [ detect(msg.payload), null, { payload:tf.memory() } ];","outputs":3,"noerr":0,"initialize":"// Code added here will be run once\n// whenever the node is started.\n// Code added here will be run once\n// whenever the node is started.\nconst platform = os.platform()\n\nasync function loadModel(model_path){\n    loaded_model = await tf.node.loadSavedModel(model_path);\n    context.set(\"savedmodel\", loaded_model);\n}\n\nasync function loadMetaGraphs(model_path){\n    const metagraphs = await tf.node.getMetaGraphsFromSavedModel(model_path);\n    context.set(\"metagraphs\", metagraphs);\n    node.send([null,{payload:metagraphs},null]);\n}\n\nasync function warmUpModel(model){\n    tf.tidy(() => {\n        const tempTensor = tf.zeros([1, 2, 2, 3]).toInt();\n        model.predict(tempTensor)\n    });    \n}\n\nif(platform == \"win32\"){\n    model_folder = env.get(\"model_folder\")\n    model_file = model_folder+\"\\\\saved_model.pb\"\n    labels_file = model_folder+\"\\\\labels.json\"\n    metadata_file = model_folder+\"\\\\metadata.json\"\n}\nelse{\n    model_folder = env.get(\"model_folder\")\n    model_file = model_folder+\"/saved_model.pb\"\n    labels_file = model_folder+\"/labels.json\"\n    metadata_file = model_folder+\"/metadata.json\"    \n}\n\nnode.warn(model_folder)\n\nif (context.get(\"labels\") === undefined) {\n    try {\n        context.set(\"labels\",JSON.parse(fs.readFileSync(labels_file, 'utf8')))\n    } catch (err) {\n        node.error(\"Error reading labels\",err)\n    }\n}\n\nif (context.get(\"metadata\") === undefined) {\n    try {\n        context.set(\"metadata\",JSON.parse(fs.readFileSync(metadata_file, 'utf8')))\n    } catch (err) {\n        node.error(\"Error reading metadata\",err)\n    }\n}\n\ntry {\n        if(fs.existsSync(model_folder)){\n            if(fs.existsSync(model_file)){\n                    node.status({fill:\"yellow\",shape:\"dot\",text:\"Loading savedmodel...\"});\n                    await loadModel(model_folder);\n                    node.status({fill:\"yellow\",shape:\"dot\",text:\"Loading metagraphs...\"});\n                    await loadMetaGraphs(model_folder)\n                    node.status({fill:\"yellow\",shape:\"dot\",text:\"Warming up savedmodel...\"});\n                    await warmUpModel(context.get(\"savedmodel\")) \n                    const backend = tf.getBackend()\n                    node.send([null,null,{payload:tf.memory()}]);\n                    node.status({fill:\"green\",shape:\"dot\",text:\"OS: \"+platform+\" | Backend: \"+backend})    \n            }\n            else{\n                node.status({fill:\"red\",shape:\"dot\",text:\"saved_model.pb not found\"})    \n            }\n        }\n        else{\n            node.status({fill:\"red\",shape:\"dot\",text:\"Model folder \"+model_folder+\" not found\"})\n        }\n}\ncatch (err) {\n        node.status({fill:\"red\",shape:\"dot\",text:\"Error loading model\"})\n        node.error(err,err)\n}","finalize":"// Code added here will be run when the\n// node is being stopped or re-deployed.\nconst model = context.get(\"savedmodel\")\ntf.dispose(model)\ncontext.set(\"model\", undefined)\ncontext.set(\"modelInfo\", undefined)","libs":[{"var":"fs","module":"fs"},{"var":"os","module":"os"},{"var":"tf","module":"@tensorflow/tfjs-node-gpu"}],"x":820,"y":180,"wires":[[],[],[]]},{"id":"8e1749840033978f","type":"function","z":"1261d76017c1af2f","name":"Set threshold & image_width_cm","func":"//Define threshold\nlet threshold = 0;\nconst global_settings = global.get(\"settings\") || undefined\nlet thresholdType = \"\"\n\nif(global_settings !== undefined){\n    if(\"threshold\" in global_settings){\n        threshold = global_settings[\"threshold\"]\n        thresholdType = \"global\";\n    }\n}\n\nelse if(\"threshold\" in msg){\n    threshold = msg.threshold;\n    thresholdType = \"msg\";\n    if(threshold < 0){\n        threshold = 0\n    }\n    else if(threshold > 1){\n        threshold = 1\n    }\n}\n\nelse{\n    try{\n        threshold = env.get(\"threshold\");\n        thresholdType = \"env\";\n    }\n    catch(err){\n        threshold = 0.5\n        thresholdType = \"default\";\n    }\n}\n\n\ntry{\n    image_width_cm_type = \"env\";\n    image_width_cm = JSON.parse(env.get(\"image_width_cm\"))[msg.topic];\n        \n}\ncatch(err){\n    image_width_cm = 130\n    image_width_cm_type = \"default\";\n}\n\n\nif(threshold == undefined){\n    threshold = 0\n}\n\nmsg.thresholdType = thresholdType;\nmsg.threshold = threshold;\nmsg.image_width_cm = image_width_cm;\nmsg.image_width_cm_type = image_width_cm_type;\n//node.status({fill:\"green\",shape:\"dot\",text:\"threshold: \"+threshold+\" | Image width: \"+image_width_cm});\nreturn msg;","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":780,"y":100,"wires":[["af7ab0534150089f"]]},{"id":"feaddfb1e7b83060","type":"function","z":"1261d76017c1af2f","name":"isBuffer?","func":"let timestamp = new Date().toISOString();\nmsg.start = new Date().getTime()\n\nif(Buffer.isBuffer(msg.payload)){\n    //node.status({fill:\"green\",shape:\"dot\",text:timestamp + \" OK\"});  \n    return msg;\n}\nelse{\n    node.error(\"msg.payload is not an image buffer\",msg)\n    node.status({fill:\"red\",shape:\"dot\",text:timestamp + \" msg.payload is not an image buffer\"});  \n    return null;\n}","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":220,"y":100,"wires":[["f8ca9de56442bbb6"]]},{"id":"450407a5f0871af6","type":"status","z":"1261d76017c1af2f","name":"","scope":["af7ab0534150089f","feaddfb1e7b83060"],"x":860,"y":240,"wires":[[]]},{"id":"ac2317efbd74fb7e","type":"exif","z":"1261d76017c1af2f","name":"","mode":"normal","property":"payload","x":550,"y":100,"wires":[["8e1749840033978f"]]},{"id":"f8ca9de56442bbb6","type":"image-info","z":"1261d76017c1af2f","name":"","x":390,"y":100,"wires":[["ac2317efbd74fb7e"]]},{"id":"58312b28dc1e97fb","type":"tab","label":"Flow 1","disabled":false,"info":"","env":[]},{"id":"f0703d64b4b491b9","type":"subflow:1261d76017c1af2f","z":"58312b28dc1e97fb","name":"","env":[{"name":"model_folder","value":"/home/tequ/.node-red/savedmodel","type":"str"},{"name":"threshold","value":"0.60","type":"num"}],"x":340,"y":100,"wires":[["8bc4af2aa1d02014","5c4fc592090853fc"],["74e105847cc1bdcf"],["1aa36ecfdd7bff3a"]]},{"id":"454f85026052428e","type":"fileinject","z":"58312b28dc1e97fb","name":"","x":140,"y":100,"wires":[["f0703d64b4b491b9"]]},{"id":"8bc4af2aa1d02014","type":"debug","z":"58312b28dc1e97fb","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","statusVal":"","statusType":"auto","x":590,"y":40,"wires":[]},{"id":"74e105847cc1bdcf","type":"debug","z":"58312b28dc1e97fb","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","statusVal":"","statusType":"auto","x":590,"y":80,"wires":[]},{"id":"1aa36ecfdd7bff3a","type":"debug","z":"58312b28dc1e97fb","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"false","statusVal":"","statusType":"auto","x":590,"y":120,"wires":[]},{"id":"5c4fc592090853fc","type":"subflow:83a7a965.1808a8","z":"58312b28dc1e97fb","name":"","env":[{"name":"box_colors","value":"{\"dog\":\"#FF0000\"}","type":"json"},{"name":"threshold","value":"0.50","type":"num"},{"name":"labels","value":"[\"person\",\"bicycle\",\"car\",\"motorcycle\",\"airplane\",\"bus\",\"train\",\"truck\",\"boat\",\"traffic light\",\"fire hydrant\",\"street sign\",\"stop sign\",\"parking meter\",\"bench\",\"bird\",\"cat\",\"dog\",\"horse\",\"sheep\",\"cow\",\"elephant\",\"bear\",\"zebra\",\"giraffe\",\"hat\",\"backpack\",\"umbrella\",\"shoe\",\"eye glasses\",\"handbag\",\"tie\",\"suitcase\",\"frisbee\",\"skis\",\"snowboard\",\"sports ball\",\"kite\",\"baseball bat\",\"baseball glove\",\"skateboard\",\"surfboard\",\"tennis racket\",\"bottle\",\"plate\",\"wine glass\",\"cup\",\"fork\",\"knife\",\"spoon\",\"bowl\",\"banana\",\"apple\",\"sandwich\",\"orange\",\"broccoli\",\"carrot\",\"hot dog\",\"pizza\",\"donut\",\"cake\",\"chair\",\"couch\",\"potted plant\",\"bed\",\"mirror\",\"dining table\",\"window\",\"desk\",\"toilet\",\"door\",\"tv\",\"laptop\",\"mouse\",\"remote\",\"keyboard\",\"cell phone\",\"microwave\",\"oven\",\"toaster\",\"sink\",\"refrigerator\",\"blender\",\"book\",\"clock\",\"vase\",\"scissors\",\"teddy bear\",\"hair drier\",\"toothbrush\",\"hair brush\"]","type":"json"}],"x":600,"y":160,"wires":[["6948d8ad0b0d58ca"]]},{"id":"6948d8ad0b0d58ca","type":"image","z":"58312b28dc1e97fb","name":"","width":"1280","data":"payload.annotation.buffer","dataType":"msg","thumbnail":false,"active":true,"pass":false,"outputs":0,"x":140,"y":200,"wires":[]}]
```

**Loading the model might take 1-3 minutes**

### 4. Use Tensorflow in Node-RED

- Configure "[AI] Detect-sm" model_folder to match folder where you downloaded the example model.

- Inject image to flow and start detecting objects.

- First inference is slow and can take something like ~5-30 seconds. After that it should run smoothly. Max fps depends of image size and machine you are using.

### 5. Custom object detection model

If you need to build your own model, you can follow this guide:

https://github.com/Lapland-UAS-Tequ/tequ-tf2-ca-training-pipeline
