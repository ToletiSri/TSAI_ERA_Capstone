# The School of AI - ERA(Extensive & Reimagined AI Program) - CAPSTONE PROJECT!!

This folder consists of the capstone project -  the shows topper to the ERA V1 course offered by - TSAI(The school of AI). 
Follow https://theschoolof.ai/ for more updates on TSAI and further courses

## CAPSTONE
You are going to make a multi-modal LLM that can take these inputs:
-- Text
-- Image
-- Audio 
The output remains text for us. 

#### Training Guidelines:
Here are the guidelines:

##### Image:
Use the original Instruct 150k dataset, and use CLIP to get the image embeddings. Either run it in real-time (need more GPU), or store the embeddings (basically preprocess and store)
Add your projection layer from this CLIP embeddings to something that can be fed to your Phi Model (do not apply QLoRa to this payer)
Add an adapter that you'll train (QLoRa) on the instruct 150k dataset

##### Audio:
You need to use Whisper to perform ASR. Add a projection layer for whisper output (which is text only)
This audio part "should" not require any training, but a pipeline from your end to link it properly to your model

##### Text:
You need to select a model that is less than 3B parameters (can be Microsoft's Phi 2 as well, but with random weights, hence training logs are MUST)
###### Data:
-- It would be close to impossible to collect ALL the datasets required to train your model. Hence:
-- You are going to use Microsoft's Phi-2 or any other model and generate data. Recommend you generate this data in parallel, don't generate and store everything as that would be a very very large dataset
-- You are going to collect "some" clean data (100MB when zipped). This data CAN be generated from Phi-2 and stored.
###### Training
-- You are going to use the same tokenizer and other data structures to keep your life simple
-- You are going to use AWS (or an equvalent system) where you are going to train YOUR model. 
-- You are going to train YOUR model (let's say that starts at 10). Train it somehow to reach the "initial loss - 1" value. Compare it with the final Microsoft's Phi 2's value and see how much more you have to train!!! You can stop your training now (but submitting this result is super important, else your Capstone project gets 0)
-- This is proof that if you were to spend more money you'll be able to train it. We'll stop here
-- Then you're going to take the default Phi-2 model (Microsoft's version and move to the next step)

#### Deployment:
Your deployment page should look like ChatGPT only, where you can send in images, text, or upload audio (live recording or file)
And you're done! :D

### Implementation and Results:
The Capstone is project is divided into 2 parts - Phase 1 and Phase 2
##### Phase 1 - 
We focus in training phi-2 model from scratch. This is as per the training guidelines for text stated above.
-- We use an untrained phi2 model (random weights), and we train this from scratch using clean dataset - SlimPajama dataset with 6B parameters.
-- File: Phase1/Phi2TrainSlimPajamaFromScratch.ipnyb
-- Special mention - Used this tutorial for training from scratch - https://huggingface.co/learn/nlp-course/chapter7/6?fw=pt

###### Results:
The loss for the corresponding iteraion number is given below. We see that the loss is reduced by more than 1 point when compared to the inital loss

-----------------------------------------------------------
    Iteration #    | Loss
-------------------------------------------------------------
    30             |    8.036800   
    60             |    7.641500     
    90             |    7.532500
    120	           |   7.351900
    150	           |    7.205000
    180	           |    7.090300
    210	           |    7.081100
    240	           |    6.975600
    270	           |    6.979500
    300	           |    6.805700
    330            |	6.810200
    360            |	6.797300
    390	           |   6.701500
    420	           |   6.678600
    450	           |   6.654300
-------------------------------------------------------------

###### Todo:
Use custom generated data as well to train

##### Phase 2 - 
As part of phase 2, we proceed with our training in the following steps:
-- Step 1:
Generate image embeddings for the images from Coco 2017 dataset. (This is because the original llava 150k dataset has referred to Coco dataset). We use a CLip model to generate these embeddings.
File: 

-- Step 2:
We use the image embeddings obtained from step 1 to train our image projection layer from scratch, as mentioned in the guidelines above for image
File: Phase2/ProjectionAndPhi2FineTuned.ipnyb

Special mention - Image projection layer code referenced from open source code:
https://github.com/sshh12/multi_token/blob/81ee75cd4435ebd5c7c7c3cf42c136c4053320fb/multi_token/modalities/projectors.py

-- Step 3:
We use the now trained projection model and fine tune it along woth phi-2 model, as mentioned in training guidelines for image.
File: Phase2/ProjectionLayerFromScratch.ipnyb

That's it, we now use these models to implement our Hugging face spaces app

Implemented a simple gradio interface on huggingface: 
HF Link: https://huggingface.co/spaces/ToletiSri/Capstone







