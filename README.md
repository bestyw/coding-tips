# python-tips

### 调用self()

在深度学习pytorch框架中，class中定义了forward()函数，则在本类中，其他函数中，调用self(x)，即调用forward函数


# 深度学习训练tips
### pytorch lightning 中文教程
> https://zhuanlan.zhihu.com/p/319810661  
> https://zhuanlan.zhihu.com/p/353985363

### 一种较好的config读入方式
default yaml文件 + 其他 yaml 文件  
argparse 读入用户自定义的config文件，再通过定义```load_config()```函数，读入 default config 的yaml文件，结合```parser.parse_known_args()``` 和 ```update_config(config, unknown)```函数，载入所有的yaml文件
示例
```pyrhon
# Arguments
parser = argparse.ArgumentParser(
    description='Train a GAN with different regularization strategies.'
)
parser.add_argument('--config', type=str, help='Path to config file.')

args, unknown = parser.parse_known_args() 
config = load_config(args.config, 'configs/default.yaml')
config = update_config(config, unknown)
```
# vscode tips

### debug 相关

#### accelerate launch 的 launch.json文件编写
与使用python命令进行launch的区别在于，去掉```"program": "${file}",```，转而使用```"module": "accelerate.commands.launch",```，要**注意**的是，```"args"```也要做修改，第一项须为待调试的python文件名称  
示例  
```python 
// lanch json for diffusers textural inversion
"configurations": [
    {
        "name": "Python: 当前文件",
        "python": "/opt/anaconda3/envs/wj-ldm/bin/python",
        "type": "python",
        "module": "accelerate.commands.launch",
        "request": "launch",
        // "program": "${file}",
        "console": "integratedTerminal",
        "justMyCode": true,
        "cwd": "/data1/Documents/weijia/diffusers/examples/textual_inversion",
        "args": [
            "textual_inversion.py",
            "--pretrained_model_name_or_path=/data1/Documents/lioo/stable-diffusion-finetune/diffusers/examples/textual_inversion/textual_inversion_bosideng",
            "--train_data_dir=/data1/Documents/weijia/data/few_downcoat",
            "--learnable_property='subject'",
            "--placeholder_token='<bosideng2022-52>'",
            "--initializer_token='coat'",
            "--resolution=512",   
            "--train_batch_size=1",  
            "--max_train_steps=1000",   
            "--learning_rate=5.0e-04", 
            "--scale_lr",   
            "--lr_scheduler='polynomial'",
            "--lr_warmup_steps=0",   
            "--output_dir='/data1/Documents/weijia/diffusers/examples/textual_inversion/textual_inversion_bosideng'", 
            "--mixed_precision=fp16",
            " --use_8bit_adam",
        ],
        "env": {"CUDA_VISIBLE_DEVICES": "0, 1, 2, 3"}
    }
]
```
