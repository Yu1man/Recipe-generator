# Recipe Generator

A system that ables users to scan the ingredients that they have, and generate some recommend dishes with instructions, ingredients needed, and estimated cook time.

When the user scan the ingredients that they have and press the scan button, the system will pass that frame into an ingredient classifier built by PyTorch, and the results will be stored in an array. After pressing the generate key, the ingredients in the stored array will be passed to the fine-tuned phi3_mini_4k_instruct_4bit model, which able to generate a reader-friendly recipe with the above requirement. The user can also input special requirement (e.g. There's no oven in my house) before generate.

The ingredient classifier can only classify carrot, chicken, corn, egg, garlic, lettuce, mushroom, onion, potato, salmon, shrimp, tomato.

## File Architecture
The main program is "food-recipe.ipynb". User have to download the  phi3_mini_4k_instruct_4bit model, fine-tune it with the intructions below, then run all to activate the software.

The program under the "Ingredient-classifier" are the training and testing program for the PyTorch Classifier.

The program under the "output" is the fine-tuned model, by using the jsonl under the "data".

## Instructions
``` 
conda create -n recipe-generator python=3.13
conda activate recipe-generator
cd recipe-generator
pip install -r requirements.txt
```

## Model Fine-tune
The phi3_mini_4k_instruct_4bit is fine tuned by using the following command in terminal. 

Fine-tune
```
python -m mlx_lm lora -c lora_config.yaml
```

Fuse the fine-tune parameter with the model
```
python -m mlx_lm fuse \
    --model models/phi3_mini_4k_instruct_4bit \
    --adapter-path adapters \
    --save-path output/phi3_finetuned_fused
```
Test the model
```
python -m mlx_lm generate \
    --model output/phi3_finetuned_fused \
    --prompt "I have potato, salmon, onion. Suggest 2 dishes, with ingredient needed, instructions and their estimated time. keep it easy to read" \
    --max-tokens 5000 \
    --temp 0.7
```

## Functions

Press p: Scan the ingredient

Press r: Clear stored ingredients

Press g: generate recipes with the stored ingredient

When the user run the "food-recipe.ipynb", there will be a window showing the frame captured from your webcam. The user can press the set button to scan and generate.

## Future Improvement

A YOLO model will be better as an ingredient classifier