in ldm/data/personalized.py d33p1na to something unique

```
training_templates_smallest = [
  'd33p1na {}',
]
```

# Setup
Run the following commands to setup the environment:

```
pip install omegaconf
pip install einops
pip install pytorch-lightning==1.6.5
pip install test-tube
pip install transformers
pip install kornia
pip install -e git+https://github.com/CompVis/taming-transformers.git@master#egg=taming-transformers
pip install -e git+https://github.com/openai/CLIP.git@main#egg=clip
pip install setuptools==59.5.0
pip install pillow==9.0.1
pip install torchmetrics==0.6.0
pip install -e .
pip install protobuf==3.20.1
pip install gdown
pip install pydrive
pip install -qq diffusers["training"]==0.3.0 transformers ftfy
pip install -qq "ipywidgets>=7,<8"
pip install huggingface_hub
pip install ipywidgets==7.7.1
git clone https://github.com/panchaldeep009/Dreambooth-Stable-Diffusion.git
cd Dreambooth-Stable-Diffusion
git clone https://github.com/djbielejeski/Stable-Diffusion-Regularization-Images-person_ddim.git
mv Stable-Diffusion-Regularization-Images-person_ddim/* .
```

Create huggingface account and download stable diffusion model
```
wget --http-user=<username> --http-password=<password> https://huggingface.co/CompVis/stable-diffusion-v-1-4-original/resolve/main/sd-v1-4-full-ema.ckpt
```

Add your photos to "Dreambooth-Stable-Diffusion/me" folder
Start training

```
python "main.py" \
 --base configs/stable-diffusion/v1-finetune_unfrozen.yaml \
 -t \
 --actual_resume "/workspace/Dreambooth-Stable-Diffusion/sd-v1-4-full-ema.ckpt" \
 --reg_data_root "/workspace/Dreambooth-Stable-Diffusion/person_ddim" \
 -n myself \
 --gpus 0, \
 --data_root "/workspace/Dreambooth-Stable-Diffusion/me" \
 --max_training_steps 2020 \
 --class_word "person" \
 --no-test
```

`--actual_resume` is the path to the checkpoint file
`--data_root` is the path to the folder containing the images to train on
`--reg_data_root` is the path to the folder containing person images

```mv logs/<checkpoint folder name>/checkpoints/last.ckpt ./```

// To compress
```python prune_ckpt.py --ckpt last.ckpt```

download the checkpoint modal from ```Dreambooth-Stable-Diffusion/last-pruned.ckpt```

// To download via ssh
```scp -P 15597 root@ssh4.vast.ai:/workspace/Dreambooth-Stable-Diffusion/last-pruned.ckpt ~/```
```scp -P <ssh connection port number> root@ssh4.vast.ai:/workspace/Dreambooth-Stable-Diffusion/last-pruned.ckpt ~/```