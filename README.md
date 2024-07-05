# ComfyUI的ControlNet辅助预处理器
![](./examples/example_mesh_graphormer.png)
即插即用的[ComfyUI](https://github.com/comfyanonymous/ComfyUI)节点集，用于制作[ControlNet](https://github.com/lllyasviel/ControlNet/)提示图像

代码是从https://github.com/lllyasviel/ControlNet/tree/main/annotator中的相应文件夹复制粘贴的，并连接到[🤗 Hub](https://huggingface.co/lllyasviel/Annotators)。

所有信用和版权归https://github.com/lllyasviel所有。

# Marigold
查看Marigold深度估计器，它可以从高分辨率静态图像生成非常详细和清晰的深度图。它创建的网格甚至可以3D打印。由于扩散器的原因，它无法在此扩展中实现，但Kijai有一个Comfy实现
https://github.com/kijai/ComfyUI-Marigold

![](./examples/example_marigold_flat.jpg)
![](./examples/example_marigold.png)

# 更新
前往[更新页面](./UPDATES.md)以关注更新

# 安装：
## 使用ComfyUI Manager（推荐）：
安装[ComfyUI Manager](https://github.com/ltdrdata/ComfyUI-Manager)并按照那里的步骤安装此仓库。

## 替代方案：
如果你在Linux上运行，或者在Windows上的非管理员账户，你需要确保`/ComfyUI/custom_nodes`和`comfyui_controlnet_aux`具有写权限。

现在有一个**install.bat**你可以运行来安装到便携设备（如果检测到）。否则，它将默认安装到系统，并假设你已经按照ComfyUI的手动安装步骤操作。

如果你无法运行**install.bat**（例如，你是Linux用户）。打开CMD/Shell并执行以下操作：
  - 导航到你的`/ComfyUI/custom_nodes/`文件夹
  - 运行`git clone https://github.com/Fannovel16/comfyui_controlnet_aux/`
  - 导航到你的`comfyui_controlnet_aux`文件夹
    - 便携/venv：
       - 运行`path/to/ComfUI/python_embeded/python.exe -s -m pip install -r requirements.txt`
	- 使用系统python
	   - 运行`pip install -r requirements.txt`
  - 启动ComfyUI

# 节点
请注意，此仓库仅支持制作提示图像（例如火柴人、canny边缘等）的预处理器。
除了Inpaint之外的所有预处理器都集成到`AIO Aux Preprocessor`节点中。
此节点允许你快速获取预处理器，但预处理器自身的阈值参数无法设置。
你需要直接使用其节点来设置阈值。

# 节点（各部分是Comfy菜单中的分类）
## 线条提取器 - Line Extractors
| Preprocessor Node           | sd-webui-controlnet/other |          ControlNet/T2I-Adapter           |
|-----------------------------|---------------------------|-------------------------------------------|
| Binary Lines                | binary                    | control_scribble                          |
| Canny Edge                  | canny                     | control_v11p_sd15_canny <br> control_canny <br> t2iadapter_canny |
| HED Soft-Edge Lines         | hed                       | control_v11p_sd15_softedge <br> control_hed |
| Standard Lineart            | standard_lineart          | control_v11p_sd15_lineart                 |
| Realistic Lineart           | lineart (or `lineart_coarse` if `coarse` is enabled) | control_v11p_sd15_lineart |
| Anime Lineart               | lineart_anime             | control_v11p_sd15s2_lineart_anime         |
| Manga Lineart               | lineart_anime_denoise     | control_v11p_sd15s2_lineart_anime         |
| M-LSD Lines                 | mlsd                      | control_v11p_sd15_mlsd <br> control_mlsd  |
| PiDiNet Soft-Edge Lines     | pidinet                   | control_v11p_sd15_softedge <br> control_scribble |
| Scribble Lines              | scribble                  | control_v11p_sd15_scribble <br> control_scribble |
| Scribble XDoG Lines         | scribble_xdog             | control_v11p_sd15_scribble <br> control_scribble |
| Fake Scribble Lines         | scribble_hed              | control_v11p_sd15_scribble <br> control_scribble |
| TEED Soft-Edge Lines        | teed                      | [controlnet-sd-xl-1.0-softedge-dexined](https://huggingface.co/SargeZT/controlnet-sd-xl-1.0-softedge-dexined/blob/main/controlnet-sd-xl-1.0-softedge-dexined.safetensors) <br> control_v11p_sd15_softedge (Theoretically)
| Scribble PiDiNet Lines      | scribble_pidinet          | control_v11p_sd15_scribble <br> control_scribble |
| AnyLine Lineart             |                           | mistoLine_fp16.safetensors <br> mistoLine_rank256 <br> control_v11p_sd15s2_lineart_anime <br> control_v11p_sd15_lineart |

## 法线和深度估计器 - Normal and Depth Estimators
| Preprocessor Node           | sd-webui-controlnet/other |          ControlNet/T2I-Adapter           |
|-----------------------------|---------------------------|-------------------------------------------|
| MiDaS Depth Map           | (normal) depth            | control_v11f1p_sd15_depth <br> control_depth <br> t2iadapter_depth |
| LeReS Depth Map           | depth_leres               | control_v11f1p_sd15_depth <br> control_depth <br> t2iadapter_depth |
| Zoe Depth Map             | depth_zoe                 | control_v11f1p_sd15_depth <br> control_depth <br> t2iadapter_depth |
| MiDaS Normal Map          | normal_map                | control_normal                            |
| BAE Normal Map            | normal_bae                | control_v11p_sd15_normalbae               |
| MeshGraphormer Hand Refiner ([HandRefinder](https://github.com/wenquanlu/HandRefiner))  | depth_hand_refiner | [control_sd15_inpaint_depth_hand_fp16](https://huggingface.co/hr16/ControlNet-HandRefiner-pruned/blob/main/control_sd15_inpaint_depth_hand_fp16.safetensors) |
| Depth Anything            |  depth_anything           | [Depth-Anything](https://huggingface.co/spaces/LiheYoung/Depth-Anything/blob/main/checkpoints_controlnet/diffusion_pytorch_model.safetensors) |
| Zoe Depth Anything <br> (Basically Zoe but the encoder is replaced with DepthAnything)       | depth_anything | [Depth-Anything](https://huggingface.co/spaces/LiheYoung/Depth-Anything/blob/main/checkpoints_controlnet/diffusion_pytorch_model.safetensors) |
| Normal DSINE              |                           | control_normal/control_v11p_sd15_normalbae |
| Metric3D Depth            |                           | control_v11f1p_sd15_depth <br> control_depth <br> t2iadapter_depth |
| Metric3D Normal           |                           | control_v11p_sd15_normalbae |
| Depth Anything V2         |                           | [Depth-Anything](https://huggingface.co/spaces/LiheYoung/Depth-Anything/blob/main/checkpoints_controlnet/diffusion_pytorch_model.safetensors) |

## 面部和姿态估计器 - Faces and Poses Estimators
| Preprocessor Node           | sd-webui-controlnet/other |          ControlNet/T2I-Adapter           |
|-----------------------------|---------------------------|-------------------------------------------|
| DWPose Estimator                 | dw_openpose_full          | control_v11p_sd15_openpose <br> control_openpose <br> t2iadapter_openpose |
| OpenPose Estimator               | openpose (detect_body) <br> openpose_hand (detect_body + detect_hand) <br> openpose_faceonly (detect_face) <br> openpose_full (detect_hand + detect_body + detect_face)    | control_v11p_sd15_openpose <br> control_openpose <br> t2iadapter_openpose |
| MediaPipe Face Mesh         | mediapipe_face            | controlnet_sd21_laion_face_v2             | 
| Animal Estimator                 | animal_openpose           | [control_sd15_animal_openpose_fp16](https://huggingface.co/huchenlei/animal_openpose/blob/main/control_sd15_animal_openpose_fp16.pth) |

## 光流估计器 - Optical Flow Estimators
| Preprocessor Node           | sd-webui-controlnet/other |          ControlNet/T2I-Adapter           |
|-----------------------------|---------------------------|-------------------------------------------|
| Unimatch Optical Flow       |                           | [DragNUWA](https://github.com/ProjectNUWA/DragNUWA) |

### 如何获取OpenPose格式的JSON？
#### 用户端
此工作流程将图像保存到ComfyUI的输出文件夹（与输出图像的同一位置）。如果你还没有找到`Save Pose Keypoints`节点，请更新此扩展
![](./examples/example_save_kps.png)

#### 开发者端
一个与IMAGE批次中每个帧对应的[OpenPose格式JSON](https://github.com/CMU-Perceptual-Computing-Lab/openpose/blob/master/doc/02_output.md#json-output-format)数组可以通过在UI上使用`app.nodeOutputs`或在`/history` API端点上使用DWPose和OpenPose获取。AnimalPose的JSON输出使用与OpenPose JSON类似的格式：
```
[
    {
        "version": "ap10k",
        "animals": [
            [[x1, y1, 1], [x2, y2, 1],..., [x17, y17, 1]],
            [[x1, y1, 1], [x2, y2, 1],..., [x17, y17, 1]],
            ...
        ],
        "canvas_height": 512,
        "canvas_width": 768
    },
    ...
]
```

对于扩展开发者（例如Openpose编辑器）：
```js
const poseNodes = app.graph._nodes.filter(node => ["OpenposePreprocessor", "DWPreprocessor", "AnimalPosePreprocessor"].includes(node.type))
for (const poseNode of poseNodes) {
    const openposeResults = JSON.parse(app.nodeOutputs[poseNode.id].openpose_json[0])
    console.log(openposeResults) //An array containing Openpose JSON for each frame
}
```

对于API用户：
Javascript
```js
import fetch from "node-fetch" //Remember to add "type": "module" to "package.json"
async function main() {
    const promptId = '792c1905-ecfe-41f4-8114-83e6a4a09a9f' //Too lazy to POST /queue
    let history = await fetch(`http://127.0.0.1:8188/history/${promptId}`).then(re => re.json())
    history = history[promptId]
    const nodeOutputs = Object.values(history.outputs).filter(output => output.openpose_json)
    for (const nodeOutput of nodeOutputs) {
        const openposeResults = JSON.parse(nodeOutput.openpose_json[0])
        console.log(openposeResults) //An array containing Openpose JSON for each frame
    }
}
main()
```

Python
```py
import json, urllib.request

server_address = "127.0.0.1:8188"
prompt_id = '' #Too lazy to POST /queue

def get_history(prompt_id):
    with urllib.request.urlopen("http://{}/history/{}".format(server_address, prompt_id)) as response:
        return json.loads(response.read())

history = get_history(prompt_id)[prompt_id]
for o in history['outputs']:
    for node_id in history['outputs']:
        node_output = history['outputs'][node_id]
        if 'openpose_json' in node_output:
            print(json.loads(node_output['openpose_json'][0])) #An list containing Openpose JSON for each frame
```
## 语义分割 - Semantic Segmentation
| Preprocessor Node           | sd-webui-controlnet/other |          ControlNet/T2I-Adapter           |
|-----------------------------|---------------------------|-------------------------------------------|
| OneFormer ADE20K Segmentor  | oneformer_ade20k          | control_v11p_sd15_seg                     |
| OneFormer COCO Segmentor    | oneformer_coco            | control_v11p_sd15_seg                     |
| UniFormer Segmentor         | segmentation              |control_sd15_seg <br> control_v11p_sd15_seg|

## T2IAdapter-only
| Preprocessor Node           | sd-webui-controlnet/other |          ControlNet/T2I-Adapter           |
|-----------------------------|---------------------------|-------------------------------------------|
| Color Pallete               | color                     | t2iadapter_color                          |
| Content Shuffle             | shuffle                   | t2iadapter_style                          |

## ## 重新着色 - Recolor
| Preprocessor Node           | sd-webui-controlnet/other |          ControlNet/T2I-Adapter           |
|-----------------------------|---------------------------|-------------------------------------------|
| Image Luminance             | recolor_luminance         | [ioclab_sd15_recolor](https://huggingface.co/lllyasviel/sd_control_collection/resolve/main/ioclab_sd15_recolor.safetensors) <br> [sai_xl_recolor_256lora](https://huggingface.co/lllyasviel/sd_control_collection/resolve/main/sai_xl_recolor_256lora.safetensors) <br> [bdsqlsz_controlllite_xl_recolor_luminance](https://huggingface.co/bdsqlsz/qinglong_controlnet-lllite/resolve/main/bdsqlsz_controlllite_xl_recolor_luminance.safetensors) |
| Image Intensity             | recolor_intensity         | Idk. Maybe same as above? |

# 示例
> 一张图片胜过千言万语

以下大多数示例的功劳归于https://huggingface.co/thibaud/controlnet-sd21。你可以从本仓库的预处理器节点获得类似的结果。
## 线条提取器
### Canny边缘 - Canny Edge
![](https://huggingface.co/thibaud/controlnet-sd21/resolve/main/example_canny.png)
### HED线条 - HED Lines
![](https://huggingface.co/thibaud/controlnet-sd21/resolve/main/example_hed.png)
### 真实感线条艺术 - Realistic Lineart
![](https://huggingface.co/thibaud/controlnet-sd21/resolve/main/example_lineart.png)
### 涂鸦/假涂鸦 - Scribble/Fake Scribble
![](https://huggingface.co/thibaud/controlnet-sd21/resolve/main/example_scribble.png)
### TEED软边缘线条 - TEED Soft-Edge Lines
![](./examples/example_teed.png)
### 任意线条艺术 - Anyline Lineart
![](./examples/example_anyline.png)

## 法线和深度图
### 深度（不知道他们使用的预处理器） - Depth (idk the preprocessor they use)
![](https://huggingface.co/thibaud/controlnet-sd21/resolve/main/example_depth.png)
## Zoe - 深度图 - Zoe - Depth Map
![](https://huggingface.co/thibaud/controlnet-sd21/resolve/main/example_zoedepth.png)
## BAE - 法线图 - BAE - Normal Map
![](https://huggingface.co/thibaud/controlnet-sd21/resolve/main/example_normalbae.png)
## MeshGraphormer
![](./examples/example_mesh_graphormer.png)
## Depth Anything & Zoe Depth Anything
![](./examples/example_depth_anything.png)
## DSINE
![](./examples/example_dsine.png)
## Metric3D
![](./examples/example_metric3d.png)
## Depth Anything V2
![](./examples/example_depth_anything_v2.png)

## 脸部和姿势
### OpenPose
![](https://huggingface.co/thibaud/controlnet-sd21/resolve/main/example_openpose.png)
![](https://huggingface.co/thibaud/controlnet-sd21/resolve/main/example_openposev2.png)

### Animal Pose (AP-10K)
![](./examples/example_animal_pose.png)

### DensePose
![](./examples/example_densepose.png)

## 语义分割
### OneFormer ADE20K Segmentor
![](https://huggingface.co/thibaud/controlnet-sd21/resolve/main/example_ade20k.png)

### Anime Face Segmentor
![](./examples/example_anime_face_segmentor.png)

## T2IAdapter-only
### Color Pallete for T2I-Adapter
![](https://huggingface.co/thibaud/controlnet-sd21/resolve/main/example_color.png)

## 光流 - Optical Flow
### Unimatch
![](./examples/example_unimatch.png)

## 重新着色 - Recolor
![](./examples/example_recolor.png)

# 测试工作流程
https://github.com/Fannovel16/comfyui_controlnet_aux/blob/master/tests/test_cn_aux_full.json
![](https://github.com/Fannovel16/comfyui_controlnet_aux/blob/master/tests/pose.png?raw=true)

# 问答：
## 为什么在我安装了这个仓库后，有些节点没有出现？

这个仓库有一个新的机制，会跳过任何无法导入的自定义节点。如果你遇到这种情况，请在[Issues tab](https://github.com/Fannovel16/comfyui_controlnet_aux/issues)上创建一个issue，并附上命令行中的日志。

## DWPose/AnimalPose 只使用CPU，所以速度很慢。我怎样才能让它使用GPU？
有两种方法可以加速DWPose：使用TorchScript检查点（.torchscript.pt）或ONNXRuntime（.onnx）。TorchScript方式比ONNXRuntime稍慢，但不需要任何额外的库，并且仍然比CPU快很多。

一个torchscript的边界框检测器可以与一个onnx的姿态估计器兼容，反之亦然。
### TorchScript
根据这张图片设置`bbox_detector`和`pose_estimator`。如果输入图像理想，你可以尝试其他以`.torchscript.pt`结尾的边界框检测器来减少边界框检测时间。
![](./examples/example_torchscript.png)
### ONNXRuntime
如果onnxruntime安装成功，并且使用的检查点以`.onnx`结尾，它将替换默认的cv2后端以利用GPU。请注意，如果你使用的是NVidia显卡，这种方法目前只能在CUDA 11.8（ComfyUI_windows_portable_nvidia_cu118_or_cpu.7z）上工作，除非你自己编译onnxruntime。

1. 了解你的onnxruntime构建：
   - **NVidia CUDA 11.x 或以下/AMD GPU**: `onnxruntime-gpu`
   - **NVidia CUDA 12.x**: `onnxruntime-gpu --extra-index-url https://aiinfra.pkgs.visualstudio.com/PublicPackages/_packaging/onnxruntime-cuda-12/pypi/simple/`
   - **DirectML**: `onnxruntime-directml`
   - **OpenVINO**: `onnxruntime-openvino`

   请注意，如果你是第一次使用ComfyUI，请先测试它是否能在你的设备上运行，然后再进行下一步。

2. 将其添加到`requirements.txt`中

3. 运行`install.bat`或安装部分提到的pip命令

![](./examples/example_onnx.png)

# Assets files of preprocessors
* anime_face_segment:  [bdsqlsz/qinglong_controlnet-lllite/Annotators/UNet.pth](https://huggingface.co/bdsqlsz/qinglong_controlnet-lllite/blob/main/Annotators/UNet.pth), [anime-seg/isnetis.ckpt](https://huggingface.co/skytnt/anime-seg/blob/main/isnetis.ckpt)
* densepose:  [LayerNorm/DensePose-TorchScript-with-hint-image/densepose_r50_fpn_dl.torchscript](https://huggingface.co/LayerNorm/DensePose-TorchScript-with-hint-image/blob/main/densepose_r50_fpn_dl.torchscript)
* dwpose:  
* * bbox_detector: Either [yzd-v/DWPose/yolox_l.onnx](https://huggingface.co/yzd-v/DWPose/blob/main/yolox_l.onnx), [hr16/yolox-onnx/yolox_l.torchscript.pt](https://huggingface.co/hr16/yolox-onnx/blob/main/yolox_l.torchscript.pt), [hr16/yolo-nas-fp16/yolo_nas_l_fp16.onnx](https://huggingface.co/hr16/yolo-nas-fp16/blob/main/yolo_nas_l_fp16.onnx), [hr16/yolo-nas-fp16/yolo_nas_m_fp16.onnx](https://huggingface.co/hr16/yolo-nas-fp16/blob/main/yolo_nas_m_fp16.onnx), [hr16/yolo-nas-fp16/yolo_nas_s_fp16.onnx](https://huggingface.co/hr16/yolo-nas-fp16/blob/main/yolo_nas_s_fp16.onnx)
* * pose_estimator: Either [hr16/DWPose-TorchScript-BatchSize5/dw-ll_ucoco_384_bs5.torchscript.pt](https://huggingface.co/hr16/DWPose-TorchScript-BatchSize5/blob/main/dw-ll_ucoco_384_bs5.torchscript.pt), [yzd-v/DWPose/dw-ll_ucoco_384.onnx](https://huggingface.co/yzd-v/DWPose/blob/main/dw-ll_ucoco_384.onnx)
* animal_pose (ap10k):
* * bbox_detector: Either [yzd-v/DWPose/yolox_l.onnx](https://huggingface.co/yzd-v/DWPose/blob/main/yolox_l.onnx), [hr16/yolox-onnx/yolox_l.torchscript.pt](https://huggingface.co/hr16/yolox-onnx/blob/main/yolox_l.torchscript.pt), [hr16/yolo-nas-fp16/yolo_nas_l_fp16.onnx](https://huggingface.co/hr16/yolo-nas-fp16/blob/main/yolo_nas_l_fp16.onnx), [hr16/yolo-nas-fp16/yolo_nas_m_fp16.onnx](https://huggingface.co/hr16/yolo-nas-fp16/blob/main/yolo_nas_m_fp16.onnx), [hr16/yolo-nas-fp16/yolo_nas_s_fp16.onnx](https://huggingface.co/hr16/yolo-nas-fp16/blob/main/yolo_nas_s_fp16.onnx)
* * pose_estimator: Either [hr16/DWPose-TorchScript-BatchSize5/rtmpose-m_ap10k_256_bs5.torchscript.pt](https://huggingface.co/hr16/DWPose-TorchScript-BatchSize5/blob/main/rtmpose-m_ap10k_256_bs5.torchscript.pt), [hr16/UnJIT-DWPose/rtmpose-m_ap10k_256.onnx](https://huggingface.co/hr16/UnJIT-DWPose/blob/main/rtmpose-m_ap10k_256.onnx)
* hed:  [lllyasviel/Annotators/ControlNetHED.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/ControlNetHED.pth)
* leres:  [lllyasviel/Annotators/res101.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/res101.pth), [lllyasviel/Annotators/latest_net_G.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/latest_net_G.pth)
* lineart:  [lllyasviel/Annotators/sk_model.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/sk_model.pth), [lllyasviel/Annotators/sk_model2.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/sk_model2.pth)
* lineart_anime:  [lllyasviel/Annotators/netG.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/netG.pth)
* manga_line:  [lllyasviel/Annotators/erika.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/erika.pth)
* mesh_graphormer:  [hr16/ControlNet-HandRefiner-pruned/graphormer_hand_state_dict.bin](https://huggingface.co/hr16/ControlNet-HandRefiner-pruned/blob/main/graphormer_hand_state_dict.bin), [hr16/ControlNet-HandRefiner-pruned/hrnetv2_w64_imagenet_pretrained.pth](https://huggingface.co/hr16/ControlNet-HandRefiner-pruned/blob/main/hrnetv2_w64_imagenet_pretrained.pth)
* midas:  [lllyasviel/Annotators/dpt_hybrid-midas-501f0c75.pt](https://huggingface.co/lllyasviel/Annotators/blob/main/dpt_hybrid-midas-501f0c75.pt)
* mlsd:  [lllyasviel/Annotators/mlsd_large_512_fp32.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/mlsd_large_512_fp32.pth)
* normalbae:  [lllyasviel/Annotators/scannet.pt](https://huggingface.co/lllyasviel/Annotators/blob/main/scannet.pt)
* oneformer:  [lllyasviel/Annotators/250_16_swin_l_oneformer_ade20k_160k.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/250_16_swin_l_oneformer_ade20k_160k.pth)
* open_pose:  [lllyasviel/Annotators/body_pose_model.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/body_pose_model.pth), [lllyasviel/Annotators/hand_pose_model.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/hand_pose_model.pth), [lllyasviel/Annotators/facenet.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/facenet.pth)
* pidi:  [lllyasviel/Annotators/table5_pidinet.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/table5_pidinet.pth)
* sam:  [dhkim2810/MobileSAM/mobile_sam.pt](https://huggingface.co/dhkim2810/MobileSAM/blob/main/mobile_sam.pt)
* uniformer:  [lllyasviel/Annotators/upernet_global_small.pth](https://huggingface.co/lllyasviel/Annotators/blob/main/upernet_global_small.pth)
* zoe:  [lllyasviel/Annotators/ZoeD_M12_N.pt](https://huggingface.co/lllyasviel/Annotators/blob/main/ZoeD_M12_N.pt)
* teed:  [bdsqlsz/qinglong_controlnet-lllite/7_model.pth](https://huggingface.co/bdsqlsz/qinglong_controlnet-lllite/blob/main/Annotators/7_model.pth)
* depth_anything: Either [LiheYoung/Depth-Anything/checkpoints/depth_anything_vitl14.pth](https://huggingface.co/spaces/LiheYoung/Depth-Anything/blob/main/checkpoints/depth_anything_vitl14.pth), [LiheYoung/Depth-Anything/checkpoints/depth_anything_vitb14.pth](https://huggingface.co/spaces/LiheYoung/Depth-Anything/blob/main/checkpoints/depth_anything_vitb14.pth) or [LiheYoung/Depth-Anything/checkpoints/depth_anything_vits14.pth](https://huggingface.co/spaces/LiheYoung/Depth-Anything/blob/main/checkpoints/depth_anything_vits14.pth)
* diffusion_edge: Either [hr16/Diffusion-Edge/diffusion_edge_indoor.pt](https://huggingface.co/hr16/Diffusion-Edge/blob/main/diffusion_edge_indoor.pt), [hr16/Diffusion-Edge/diffusion_edge_urban.pt](https://huggingface.co/hr16/Diffusion-Edge/blob/main/diffusion_edge_urban.pt) or [hr16/Diffusion-Edge/diffusion_edge_natrual.pt](https://huggingface.co/hr16/Diffusion-Edge/blob/main/diffusion_edge_natrual.pt)
* unimatch: Either [hr16/Unimatch/gmflow-scale2-regrefine6-mixdata.pth](https://huggingface.co/hr16/Unimatch/blob/main/gmflow-scale2-regrefine6-mixdata.pth), [hr16/Unimatch/gmflow-scale2-mixdata.pth](https://huggingface.co/hr16/Unimatch/blob/main/gmflow-scale2-mixdata.pth) or [hr16/Unimatch/gmflow-scale1-mixdata.pth](https://huggingface.co/hr16/Unimatch/blob/main/gmflow-scale1-mixdata.pth)
* zoe_depth_anything: Either [LiheYoung/Depth-Anything/checkpoints_metric_depth/depth_anything_metric_depth_indoor.pt](https://huggingface.co/spaces/LiheYoung/Depth-Anything/blob/main/checkpoints_metric_depth/depth_anything_metric_depth_indoor.pt) or [LiheYoung/Depth-Anything/checkpoints_metric_depth/depth_anything_metric_depth_outdoor.pt](https://huggingface.co/spaces/LiheYoung/Depth-Anything/blob/main/checkpoints_metric_depth/depth_anything_metric_depth_outdoor.pt)
# 1500 Stars 😄
<a href="https://star-history.com/#Fannovel16/comfyui_controlnet_aux&Date">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/svg?repos=Fannovel16/comfyui_controlnet_aux&type=Date&theme=dark" />
    <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/svg?repos=Fannovel16/comfyui_controlnet_aux&type=Date" />
    <img alt="Star History Chart" src="https://api.star-history.com/svg?repos=Fannovel16/comfyui_controlnet_aux&type=Date" />
  </picture>
</a>

感谢大家的支持。我从未想过星星的图表会是线性的，哈哈。
