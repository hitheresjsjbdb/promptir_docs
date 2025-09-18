# promptir 配置环境和使用方法

## 1.配置虚拟环境（已经完成,此处只做记录，正常使用时跳过这些步骤）
由于cuda版本不相同，无法直接使用文件夹中的environment.yml,这里使用了简化版的yml文件创建conda虚拟环境：
```bash
conda env create -f environment_cuda101.yml -p /data1/user23004/promptir
conda activate /data1/user23004/promptir
```
在创建并激活虚拟环境之后使用pip安装剩余的依赖：
```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --default-timeout=600 --retries 10 -r requirements_pip.txt
```
这里为了防止超时使用了清华源并提高了超时时间

等待安装完成之后运行
```bash
python -c "import torch, timm, pytorch_lightning as pl; print(torch.__version__, timm.__version__, pl.__version__)"
```
如果得到
```bash
2.4.1+cu121 0.6.13 1.5.10
```
说明没有问题，环境已经配置完成

## 2.使用方法（正常使用从此开始看）
### (1)使用vscode连接服务器
参考这篇文章：https://blog.csdn.net/zhaxun/article/details/120568402

其中，输入的远程服务器账号和ip为
```bash
ssh user23004@172.25.4.41
```
密码为：11223344。
### (2)运行单图推理
连接成功后打开/data1/user23004/PromptIR_os文件夹，左上方新建终端，在终端中激活虚拟环境：
```bash
conda activate /data1/user23004/promptir
```
成功激活后运行test.py:
```bash
python test.py --ckpt_name epoch=139-step=431060-v1.ckpt --input_image Derain/Rain100L/rainy/rain-013.png --output_dir output/single/
```
其中，--ckpt_name 后面是模型路径；--input_image 后是输入图片，可以根据实际情况进行修改；--output_dir 后是输出图片路径，可以根据实际情况进行修改。即可完成单图推理。

## 3.端侧使用方法（简写）
首先进入系统后，进入路径并激活虚拟环境：
```bash
/home/nvidia/PromptIR_os&&conda activate promptir_env
```

由于SciPy需要的包的路径问题，需要提前配置环境变量：
```bash
export LD_LIBRARY_PATH="$CONDA_PREFIX/lib:${LD_LIBRARY_PATH}"
export LD_PRELOAD="$CONDA_PREFIX/lib/libstdc++.so.6:${LD_PRELOAD}"
```
之后使用上面的命令进行推理即可
## git简要说明

```bash
git status #查看修改了哪些文件
git add . #将修改文件添加到暂存区
git add 文件名 #或者只添加某个文件
git commit -m "简要说明你做了什么修改" #提交修改
git push #推送到github
```