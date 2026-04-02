# Dreambooth_LoRA_FT
LoRA FT on diffusion V1.5

1. Subject and Dataset

The object is a game character. The dataset includes 3 images of the distinct object, covering three different clothing styles while preserving facial features and artistic styles.
The unique identifier token associated with this subject is “a photo of hbr mari_satsuki”.
Fig. 1 Dataset example

2. Model and Training Setup

The model for training is Stable Diffusion v1.5.
Using Hugging Face diffusers DreamBooth scripts, the training is LoRA-based fine-tuning. To further enhance fine-tuning performance in facial, the text encoder is trained. Other main training configurations are listed:
- batch size: 2
- learning rate: 5e-5
- max train steps: 500
  
3. Representative Results
By placing the object into different new contexts or styles, such as different environments, modified attributes, added accessories, and artistic styles, the fine-tuned model outputs some representative results.
    
  3.1 Different Environments
    
  3.2 Modified Attributes or Added Accessories

  3.3 Artistic Styles

5. Observations and Analysis
   
  4.1 Fidelity

The overall subject fidelity is acceptable, however, the fine-tuned model does not perfectly reproduce the face.
The prompt fidelity varies in different contexts. For most cases, the environment and style can be well generated. But in some cases, for instance, the accessory object can not be generated correctly to fit with the subject together.

  4.2 Fitting

Since the subject is a character, and the output results show some inconsistency in the human face, the fine-tuned model's performance is underfitting in human face generation.

  4.3 Diversity

Using LPIPS to do evaluation with a certain prompt “a photo of hbr mari_satsuki wearing a tiny wizard hat, holding a wooden staff, magical forest background”, the average LPIPS among 4 generated images is 0.6747, which shows a good performance in diversity based on the given prompt.

Fig. 2 Diversity example

  4.4 Prior Preservation

The observation shows that the model can generate images with different environments using the class prompt only. However, the clothing of the character tends to be the same.

Fig. 3 Class prompt only example
