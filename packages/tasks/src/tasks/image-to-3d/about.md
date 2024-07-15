## Use Cases

Image-to-3D models can be used in a wide variety applications that require 3D, such as games, animation, design, architecture, engineering, marketing, and more.

### Meshes

Meshes are the standard representation of 3D in industry.

Meshes can be in `.obj`, `.glb`, `.stl`, or `.gltf` format. Other formats are allowed, but won't be rendered in the gradio [Model3D](https://www.gradio.app/docs/gradio/model3d) component.

### Splats

[Gaussian Splatting](https://huggingface.co/blog/gaussian-splatting) is a differentiable rasterization technique that represents 3D as fuzzy points.

Splats can be in `.ply` or `.splat` format. They can be rendered in the gradio [Model3D](https://www.gradio.app/docs/gradio/model3d) component using the [gsplat.js](https://github.com/huggingface/gsplat.js) library.

### Inference

You can use custom diffusion pipelines to infer with `image-to-3D` models.

These are unstandardized and depend on the model.

```python
import torch
import requests
import numpy as np
from io import BytesIO
from diffusers import DiffusionPipeline
from PIL import Image

pipeline = DiffusionPipeline.from_pretrained(
    "dylanebert/LGM-full",
    custom_pipeline="dylanebert/LGM-full",
    torch_dtype=torch.float16,
    trust_remote_code=True,
).to("cuda")

input_url = "https://huggingface.co/datasets/dylanebert/iso3d/resolve/main/jpg@512/a_cat_statue.jpg"
input_image = Image.open(BytesIO(requests.get(input_url).content))
input_image = np.array(input_image, dtype=np.float32) / 255.0
result = pipeline("", input_image)
result_path = "/tmp/output.ply"
pipeline.save_ply(result, result_path)
print(f"Output saved to {result_path}")
```

## Useful Resources

- [ML for 3D Course](https://huggingface.co/learn/ml-for-3d-course)
- [3D Arena Leaderboard](https://huggingface.co/spaces/dylanebert/3d-arena)
