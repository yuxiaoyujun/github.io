---
title: 【stable diffution】什麼是ControlNet？
date: 2024-07-05 15:16:34
tags: 
  - ai繪畫  
  - sd
categories:
  - 繪畫
---

### 什麼是ControlNet？

ControlNet 的作用是通过添加额外控制条件，来引导 Stable Diffusion 按照创作者的创作思路生成图像，从而提升 AI 图像生成的可控性和精度。在使用 ControlNet前，需要确保已经正确安装 Stable Diffusion和 ControlNet 插件。

目前 ControlNet 已经更新到 1.1 版本，相较于 1.0 版本，ControlNet1.1 新增了更多的预处理器和模型，原有的模型也通过更好的数据训练获得了更优的性能。以下我做简要梳理，想要了解更多内容可以参考作者的文档[https://github.com/lllyasviel/ControlNet-v1-1-nightly](https://link.uisdc.com/?redirect=https%3A%2F%2Fgithub.com%2Flllyasviel%2FControlNet-v1-1-nightly)

![](/images/image-20240705183328225.png)

ControlNet 的使用方式灵活，既可以单模型应用，也可以多模型组合应用。清楚 ControlNet 的一些原理方法后，可以帮助我们更好的提升出图效果。以下通过一些示例，简要介绍 ControlNet 的实际用法。

#### 一、ControlNet 单模型应用

**1. 线稿上色**

**方法：**通过 ControlNet 边缘检测模型或线稿模型提取线稿（可提取参考图片线稿，或者手绘线稿），再根据提示词和风格模型对图像进行着色和风格化。

**应用模型：**Canny、SoftEdge、Lineart。

Canny 边缘检测：

Canny 是比较常用的一种线稿提取方式，该模型能够很好的识别出图像内各对象的边缘轮廓。

使用说明（以下其它模型同理）：

1. 展开 ControlNet 面板，上传参考图，勾选 Enable 启用（如果显存小于等于 4G，勾选低显存模式）。
2. 预处理器选择 Canny（**注意：**如果上传的是已经经过预处理的线稿图片，则预处理器选择 none，不进行预处理），模型选择对应的 control_v11p_sd15_canny 模型。
3. 勾选 Allow Preview 允许预览，点击预处理器旁的🎆按钮生成预览。

其它参数说明：

1. Control Weight：使用 ControlNet 生成图片的权重占比影响（多个 ControlNet 组合使用时，需要调整权重占比）。
2. Starting Control Step：ControlNet 开始参与生图的步数。
3. Ending Control Step：ControlNet 结束参与生图的步数。
4. Preprocessor resolution：预处理器分辨率，默认 512，数值越高线条越精细，数值越低线条越粗糙。
5. Canny 低阈值/高阈值：数值越低线条越复杂，数值越高线条越简单。

Canny 示例：（保留结构，再进行着色和风格化）

![](/images/image-20240705183423790.png)

SoftEdge 软边缘检测：

SoftEdge 可以理解为是 ControlNet1.0 中 HED 边缘检测的升级版。ControlNet1.1 版本中 4 个预处理器按结果质量排序：SoftEdge_HED > SoftEdge_PIDI > SoftEdge_HED_safe > SoftEdge_PIDI_safe，其中带 safe 的预处理器可以防止生成的图像带有不良内容。相较于 Canny，SoftEdge 边缘能够保留更多细节。

SoftEdge 示例：（保留结构，再进行着色和风格化）

![](/images/image-20240705183500518.png)

Lineart 精细线稿提取：

Lineart 精细线稿提取是 ControlNet1.1 版本中新增的模型，相较于 Canny，Lineart 提取的线稿更加精细，细节更加丰富。

Lineart 的预处理器有三种模式：lineart_coarse（粗略模式），lineart_realistic（详细模式），lineart_standard（标准模式），处理效果有所不同，对比如下：

![](/images/image-20240705183520291.png)

Lineart 示例：（保留结构，再进行着色和风格化）

![](/images/image-20240705183549871.png)

**2. 涂鸦成图**

方法：通过 ControlNet 的 Scribble 模型提取涂鸦图（可提取参考图涂鸦，或者手绘涂鸦图），再根据提示词和风格模型对图像进行着色和风格化。

应用模型：Scribble。

Scribble 比 Canny、SoftEdge 和 Lineart 的自由发挥度要更高，也可以用于对手绘稿进行着色和风格处理。Scribble 的预处理器有三种模式：Scribble_hed，Scribble_pidinet，Scribble_Xdog，对比如下，可以看到 Scribble_Xdog 的处理细节更为丰富：

![](/images/image-20240705183604719.png)

Scribble 参考图提取示例（保留大致结构，再进行着色和风格化）：

![](/images/image-20240705183616329.png)

Scribble 手动涂鸦示例（根据手绘草图，生成图像）：

也可以不用参考图，直接创建空白画布，手绘涂鸦成图。

![](/images/image-20240705183628539.png)

**3. 建筑/室内设计**

方法：通过 ControlNet 的 MLSD 模型提取建筑的线条结构和几何形状，构建出建筑线框（可提取参考图线条，或者手绘线条），再配合提示词和建筑/室内设计风格模型来生成图像。

应用模型：MLSD。

MLSD 示例：（毛坯变精装）

![](/images/image-20240705183741384.png)

**4. 颜色控制画面**

方法：通过 ControlNet 的 Segmentation 语义分割模型，标注画面中的不同区块颜色和结构（不同颜色代表不同类型对象），从而控制画面的构图和内容。

应用模型：Seg。

Seg 语义参考： [https://docs.qq.com/sheet/DYmtkWG5taWxhVkx2?tab=BB08J2](https://link.uisdc.com/?redirect=https%3A%2F%2Fdocs.qq.com%2Fsheet%2FDYmtkWG5taWxhVkx2%3Ftab%3DBB08J2)

![](/images/image-20240705183812030.png)

Seg 示例：（提取参考图内容和结构，再进行着色和风格化）

![](/images/image-20240705183827557.png)

如果还想在车前面加一个人，只需在 Seg 预处理图上对应人物色值，添加人物色块再生成图像即可。

![](/images/image-20240705183929895.png)

**5. 背景替换**

方法：在 img2img 图生图模式中，通过 ControlNet 的 Depth_leres 模型中的 remove background 功能移除背景，再通过提示词更换想要的背景。

应用模型：Depth，预处理器 Depth_leres。

要点：如果想要比较完美的替换背景，可以在图生图的 Inpaint 模式中，对需要保留的图片内容添加蒙版，remove background 值可以设置在 70-80%。

Depth_leres 示例：（将原图背景替换为办公室背景）

![](/images/image-20240705183840642.png)

**6. 图片指令**

方法：通过 ControlNet 的 Pix2Pix 模型（ip2p），可以对图片进行指令式变换。

应用模型：ip2p，预处理器选择 none。

要点：采用指令式提示词（make Y into X），如下图示例中的 make it snow，让非洲草原下雪。

Pix2Pix 示例：（让非洲草原下雪

![](/images/image-20240705183950085.png)

**7. 风格迁移**

方法：通过 ControlNet 的 Shuffle 模型提取出参考图的风格，再配合提示词将风格迁移到生成图上。

应用模型：Shuffle。

Shuffle 示例：（根据魔兽道具风格，重新生成一个宝箱道具）

![](/images/image-20240705184002642.png)

**8. 色彩继承**

方法：通过 ControlNet 的 t2iaColor 模型提取出参考图的色彩分布情况，再配合提示词和风格模型将色彩应用到生成图上。

应用模型：Color。

Color 示例：（把参考图色彩分布应用到生成图上）

![](/images/image-20240705184024748.png)

**9. 角色三视图**

方法：通过 ControlNet 的 Openpose 模型精准识别出人物姿态，再配合提示词和风格模型生成同样姿态的图片。

应用模型：OpenPose。在 ControlNet1.1 版本中，提供了多种姿态检测方式，包含：openpose 身体、openpose_face 身体+脸、openpose_faceonly 只有脸、openpose_full 身体+手+脸、openpose_hand 手，可以根据实际需要灵活应用。

![](/images/image-20240705184038245.png)

OpenPose 角色三视图示例：

要点：上传 openpose 三视图，加载 charturner 风格模型（ [https://civitai.com/?query=charturner](https://link.uisdc.com/?redirect=https%3A%2F%2Fcivitai.com%2F%3Fquery%3Dcharturner) ），添加提示词保持背景干净 (simple background, white background:1.3), multiple views

![](/images/image-20240705184058855.png)

**10. 图片光源控制**

方法：如果想对生成的图片进行打光，可以在 img2img 模式下，把光源图片上传到图生图区域，ControlNet 中放置需要打光的原图，ControlNet 模型选择 Depth。

应用模型：Depth。

要点：图生图中的所有参数和提示词信息需要与原图生成时的参数一样，具体原图参数可以在 PNG Info 面板中查看并复制。

示例：

![](/images/image-20240705184112095.png)

#### 二、ControlNet 多模型组合应用

ControlNet 还支持多个模型的组合使用，从而对图像进行多条件控制。ControlNet 的多模型控制可以在设置面板中的 ControlNet 模块中开启：

![](/images/image-20240705184125721.png)

**1. 人物和背景分别控制**

方法：设置 2 个 ControlNet，第一个 ControlNet 通过 OpenPose 控制人物姿态，第二个 ControlNet 通过 Seg 或 Depth 控制背景构成。调整 ControlNet 权重，如 OpenPose 权重高于 Depth 权重，以确保人物姿态被正确识别，再通过提示词和风格模型进行内容和风格控制。

应用模型：OpenPose、Seg（自定义背景内容和结构）、Depth。

示例：

![](/images/image-20240705184138334.png)

**2. 三维重建**

方法：通过 Depth 深度检测和 Normalbae 法线贴图模型，识别三维目标。再配合提示词和风格模型，重新构建出三维物体和场景。

应用模型：Depth、Normalbae。

示例：

![](/images/image-20240705184150207.png)

**3. 更精准的图片风格化**

方法：在 img2img 图生图中，通过叠加 Lineart 和 Depth 模型，可以更加精准的提取图像结构，最大程度保留原图细节，再配合提示词和风格模型重新生成图像。

应用模型：Lineart、Depth。

示例：

![](/images/image-20240705184204506.png)

**4. 更精准的图片局部重绘**

方法：在 img2img 图生图的局部重绘中，通过叠加 Canny 和 Inpaint 模型，可以更加精准的对图像进行局部重绘。

应用模型：Canny、Inpaint。

示例：

![](/images/image-20240705184218789.png)
