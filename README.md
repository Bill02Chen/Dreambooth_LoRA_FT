# DreamBooth LoRA fine-tuning of Stable Diffusion v1.5

[Portfolio](https://bill02chen.github.io/chenyanrui-site/) · [GitHub](https://github.com/Bill02Chen)

Personalise Stable Diffusion v1.5 to a single game character from three images,
and generate that character in new environments, attributes and artistic styles.

**Key result:** the LoRA-tuned model reproduces the character in new contexts,
with an LPIPS diversity score of **0.6747** over four samples for a fixed prompt.
Face fidelity and accessory composition remain limited; the limitations are
reported below.

## Problem

Text-to-image models do not know a specific game character. Fine-tuning the full
model on three images would overfit and destroy prior knowledge. DreamBooth with
LoRA is a way to add a subject with a small adapter while keeping the base model
intact.

The questions were:

- can three images be enough to learn a usable subject?
- how well does the subject transfer to new environments and styles?
- where does it fail?

## Approach

### Dataset and token

- Subject: a game character, three images covering different clothing styles.
- Unique identifier: `a photo of hbr mari_satsuki`.

![Dataset example](images/dataset.png)

### Training

- Base model: Stable Diffusion v1.5.
- Method: DreamBooth LoRA with the Hugging Face `diffusers` scripts.
- The text encoder is also trained, to improve facial detail.

| Hyperparameter | Value |
| --- | --- |
| Batch size | 2 |
| Learning rate | 5e-5 |
| Max train steps | 500 |

## Key results

The tuned model places the subject into:

- **new environments** — different scenes and backgrounds;
- **modified attributes or accessories** — clothing and objects;
- **artistic styles** — illustration and painterly treatments.

| Result | Evidence |
| --- | --- |
| Different environments | `images/o1.png` |
| Modified attributes / accessories | `images/o2.png` |
| Artistic styles | `images/o3.png` |
| Diversity: LPIPS 0.6747 (4 samples, fixed prompt) | `images/diversity.png` |
| Class-prompt-only prior preservation | `images/subject.png` |

What works and what does not:

- **Fidelity:** overall subject fidelity is acceptable, but the face is not
  reproduced perfectly.
- **Prompt fidelity:** environments and styles follow the prompt well; some
  accessory objects do not fit the subject correctly.
- **Fitting:** the model underfits human faces. With three images, the face is the
  hardest part.
- **Diversity:** LPIPS 0.6747 across four generations for the prompt
  `a photo of hbr mari_satsuki wearing a tiny wizard hat, holding a wooden staff,
  magical forest background`; higher means more variation.
- **Prior preservation:** class-prompt-only generations vary the environment but
  tend to keep the same clothing.

## Evidence

- [Dreambooth_LoRA.ipynb](Dreambooth_LoRA.ipynb) — training and inference
  notebook.
- [images/dataset.png](images/dataset.png) — the three input images.
- [images/o1.png](images/o1.png) — new environments.
- [images/o2.png](images/o2.png) — modified attributes / accessories.
- [images/o3.png](images/o3.png) — artistic styles.
- [images/diversity.png](images/diversity.png) — LPIPS diversity samples.
- [images/subject.png](images/subject.png) — class-prompt-only generations.

The trained LoRA weights are not committed; the notebook reproduces the run.

## Reproduce

1. Open `Dreambooth_LoRA.ipynb` in a GPU environment.
2. Prepare the three subject images and the instance prompt.
3. Run the DreamBooth LoRA training cells (batch 2, LR 5e-5, 500 steps).
4. Run the inference cells with the prompts in the results section.

The notebook follows the Hugging Face `diffusers` DreamBooth example. Memory use
depends on the script flags, resolution and batch size; the notebook records the
settings used for this run.

## Limitations

- Three images are not enough for a stable face; the model underfits.
- LPIPS over four samples is a small sample; treat it as a signal, not a
  benchmark.
- The results are qualitative; there is no FID or human preference study.
- The subject is a game character; check the rights before any commercial use of
  the generated images.
