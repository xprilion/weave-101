# Weave 101: Tracing Gen AI with Weave

[![Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1Elm8me1v6qSjHGtLpEwJmHvMC_MQpYjv)

This repository demonstrates a minimal example of using **[Weave](https://wandb.ai/weave)** to trace and log LLM API interactions, including text completions and image generation. It was originally developed for the **AI Converge BLR - May 2025** meetup.

## 🔍 What’s Inside

* ✅ Setup for OpenAI + Weave in a Colab Notebook
* 🧐 Pirate-talking coding assistant using `gpt-4o`
* 🎨 Image generation with `gpt-image-1`
* 📈 Automatic tracing via `@weave.op`

## 🚀 Getting Started

To run the notebook on [Google Colab](https://colab.research.google.com):

1. Clone this repo or open the notebook directly [here](https://colab.research.google.com/drive/1Elm8me1v6qSjHGtLpEwJmHvMC_MQpYjv).

2. Make sure to install dependencies:

   ```python
   !uv pip install weave
   !uv pip install openai
   ```

3. Set your environment variables (via Colab's `userdata`):

   ```python
   os.environ["WANDB_API_KEY"] = userdata.get("WANDB_API_KEY")
   os.environ["OPENAI_API_KEY"] = userdata.get("OPENAI_TEMP_KEY")
   ```

4. Initialize Weave:

   ```python
   weave.init("ai-converge-blr-may-2025")
   ```

## 🧪 Example Usage

### 1. Pirate Coding Assistant (Text Completion)

```python
response = client.responses.create(
    model="gpt-4o",
    instructions="You are a coding assistant that talks like a pirate.",
    input="How do I check if a Python object is an instance of a class?",
)
print(response.output_text)
```

### 2. Image Generation (With Weave Tracing)

```python
@weave.op
def generate_image(prompt: str) -> Image:
    result = client.images.generate(
        model="gpt-image-1",
        prompt=prompt
    )
    image_base64 = result.data[0].b64_json
    image_bytes = base64.b64decode(image_base64)
    return image_bytes

Image(generate_image("A children's book drawing of a veterinarian using a stethoscope to listen to the heartbeat of a baby otter."))
```

## 📊 Weave Tracing

By decorating `generate_image` with `@weave.op`, the image generation call is automatically traced and can be visualized in the [Weave app](https://wandb.ai/weave).

## 🧰 Requirements

* Python 3.7+
* OpenAI SDK (`openai`)
* Weave (`weave`)
* WandB API Key
* Colab `userdata` access

## 📌 License

MIT License

---

Created with ❤️ by [@xprilion](https://github.com/xprilion) for AI Converge BLR
