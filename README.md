## EXP-06 : Prototype Development for Image Captioning Using the BLIP Model and Gradio Framework
### NAME : HARI PRIYA M.
### REGISTER NO : 212224240047

### AIM:
To design and deploy a prototype application for image captioning by utilizing the BLIP image-captioning model and integrating it with the Gradio UI framework for user interaction and evaluation.

### PROBLEM STATEMENT:
Manual annotation of images for descriptions is time-consuming and inconsistent across large datasets. This experiment addresses the problem by developing an automated image captioning system using the BLIP (Bootstrapped Language-Image Pretraining) model. The system integrates with a user-friendly Gradio interface, allowing users to upload images and instantly generate meaningful captions, thus improving efficiency in applications like accessibility tools, content tagging, and data labeling.

### DESIGN STEPS:

<b>STEP 1:</b> Import the necessary libraries such as Gradio, Requests, and other utility modules for handling images and API communication.

<b>STEP 2:</b> Define a function get_completion() to send image data to the BLIP API endpoint and receive the generated caption response.

<b>STEP 3:</b> Create a helper function image_to_base64_str() to convert uploaded images into base64 strings compatible with the API request format.

<b>STEP 4:</b> Develop a main function captioner(image) that processes the image through the BLIP model and retrieves the generated caption.

<b>STEP 5:</b> Build a Gradio interface with gr.Interface(), allowing users to upload an image, display captions, and visualize example outputs.

### PROGRAM:

```python
import os
import io
import IPython.display
from PIL import Image
import base64 
from dotenv import load_dotenv, find_dotenv
_ = load_dotenv(find_dotenv()) # read local .env file
hf_api_key = os.environ['HF_API_KEY']
```

```python
import requests, json

#Image-to-text endpoint
def get_completion(inputs, parameters=None, ENDPOINT_URL=os.environ['HF_API_ITT_BASE']):
    headers = {
      "Authorization": f"Bearer {hf_api_key}",
      "Content-Type": "application/json"
    }
    data = { "inputs": inputs }
    if parameters is not None:
        data.update({"parameters": parameters})
    response = requests.request("POST",
                                ENDPOINT_URL,
                                headers=headers,
                                data=json.dumps(data))
    return json.loads(response.content.decode("utf-8"))
```

```python
import gradio as gr 

def image_to_base64_str(pil_image):
    byte_arr = io.BytesIO()
    pil_image.save(byte_arr, format='PNG')
    byte_arr = byte_arr.getvalue()
    return str(base64.b64encode(byte_arr).decode('utf-8'))

def captioner(image):
    base64_image = image_to_base64_str(image)
    result = get_completion(base64_image)
    return result[0]['generated_text']

gr.close_all()
demo = gr.Interface(fn=captioner,
                    inputs=[gr.Image(label="Upload image", type="pil")],
                    outputs=[gr.Textbox(label="Caption")],
                    title="Image Captioning with BLIP",
                    description="Caption any image using the BLIP model",
                    allow_flagging="never",
                    examples=["christmas_dog.jpeg", "bird_flight.jpeg", "cow.jpeg"])

demo.launch(share=True, server_port=int(os.environ['PORT1']))
```

```python
gr.close_all()
```

### OUTPUT:
<img width="900" height="500" alt="Screenshot 2025-11-13 071513" src="https://github.com/user-attachments/assets/1924dfb9-2895-4ca8-a730-0157b86581c3" />

### RESULT:
Thus, the image captioning application using the BLIP model was successfully developed and deployed with Gradio. The system efficiently generates descriptive captions for images, demonstrating the practical use of multimodal AI in automation and accessibility.
