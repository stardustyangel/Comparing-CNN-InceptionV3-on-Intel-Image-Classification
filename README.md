# Comparing-CNN-InceptionV3-on-Intel-Image-Classification

![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/629a85b2-382e-4adf-82b9-eb8072e1ce42)


This repo is a comparative study of performance between CNN from scratch and using transfer learning : 


## Data Table Summary 

| Properties       | Details                                                                         |
| ---------------- | ------------------------------------------------------------------------------- |
| Name             | Intel Image Classificaiton                                                      |
| Source           | [Kaggle](https://www.kaggle.com/datasets/puneet6060/intel-image-classification) |
| Author           | PUNEET BANSAL                                                                   |
| Data Type        | Images                                                                         |
| Problem Type        | Image Classificaiton                                                                          |
| Files            | 3  Folders (14K Training , 3K Testing , 7K Prediction)                        |
| Classes            | 6  Classes (Buildings, Forest, Glacier, Mountain, Sea, Street)                        |


## Preprocessing Steps : 
- **Image Resizing :** Image has been resized to `(150,150)`
- **Image Normalisation :** Normalising pixel values from `0-255` to between `0-1`
- **Encoding Image Labels :** Transforming labels from categorical format to numerical using `Label Encoder`

## Modelling : 
In this part we will go through the different CNN version that we created from scratch and using transfer learning with different versions compare the results including fine-tuning :

- **Simple CNN :**
    - 1 convolutional layer `(32 Units)`
    - 1 MaxPooling layer
    - 2 Dense Layers `( 128 Units , 6 )`
    - 10 Epochs
- **Deep CNN :**
    - 4 convolutional layers `(2 * 32 Units + 64 Units + 128 Units)`
    - 4 Maxpooling Layers
    - 3 Dense Layers `(128 Units + 64 Units , 6 )`
    - 10 Epochs
- **Deep CNN + Fine-tuning :**
    - Same previous Architecure
    - Change optimizer to `Adamax`
    - Added Callbacks (`early stopping` + `LR Scheduler`)
    - 20 Epochs
- **InceptionV3 I :**
    - Excluding Original Classification layers (`include_top = False`)
    - Using ImageNet Weights (`weights='imagenet'`)
    - Unfreezing the first 25 layers (`inception1.layers[-25:]`)
    - Adding Costume classificaiton head :
        - Dense layer `(1024 units)`
        - Dropout layer `Dropping 20% of units (0.2)`
        - Classification layer (last dense layer) `(6)`
    - Optimizer `Adamax` with starting learning rate of `0.0001`
    - Callbacks (`early stopping +  retreive best weights`)
    - 20 Epochs 
- **InceptionV3 II :**
    - Same Previous Architecture
    - Unfreezing the last 25 layers instead (`inception2.layers[25:]`)
    - 20 Epochs 
- **InceptionV3 III :**
    -  Using the full architecture (`48 Layers`)
    -  Different Costume classificaiton head :
        - Batch Normalization Layer
        - 2 Dense Layers (`256 Units + 128 Units`)
        - 1 Classification layer (`6`)
    - Same Callbacks
    - 20 Epochs
 
## Results : 
### I - Simple CNN :
- **Training :** The Simple CNN did a horrible job with evaluation of `68%` Accuracy & `140%` Loss 

![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/d8622ade-00b4-4e33-b6e4-01abb2a6c538)

- **Prediciton :** The bad training explains the bad predictions done by the simple CNN failing to classify a lot of image

![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/c08f8d87-da3f-4001-8a8c-7ef85a42e725)

### II - Deep CNN :
- **Training :** The Deep CNN did a better job than the normal CNN with evaluation of `81%` Accuracy but still having errors with `82%` Loss

![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/f2161c7c-ce8c-4526-966c-0e045de79e9e)


- **Prediciton :** The defective training explains the wrong predictions done by the Deep CNN failing to classify some images

![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/e57f84dc-65dc-4d59-a519-604ef31ccbc5)

### III - Deep CNN + Fine-tuning  :

- **Training :** The Fine-tubned Deep CNN did a better than the previous models with evaluation of `82%` Accuracy but still having errors with `49%` Loss (still high loss)

![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/3511a263-8b81-4f00-9caa-29972607daa5)


- **Prediciton :** The improvement in the training results can be seen in the predictions done by the fine-tuned CNN

![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/ef3581c7-cdfe-4bb4-b114-1b11a5486df7)



### IV - InceptionV3 (ver 1)  :

- **Training :** We can already see the supremacy of transfer learning here with evaluation of `90%` Accuracy but still having errors with `27%` Loss
  
![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/274f9271-7cd4-469d-a7d5-b01397aeda08)

- **Prediciton :** The difference cannot be really seen here , but overall it's doing way better than previous models
  
![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/6dc0cd05-7554-4cd5-adb0-ad319048b22e)

### IV - InceptionV3 (ver 2)  :

- **Training :** Freezing the first 25 layers insead showed difference with `91%` Accuracy but still having errors with `25%` Loss

![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/db996005-a636-410a-92d0-500c66eda73f)

- **Prediciton :** The difference cannot be really seen here , but overall it's doing way better than previous models

![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/573aff49-48fc-469f-a308-b891d51e4dca)


### IV - InceptionV3 (ver 3)  :

- **Training :** Using the whole model made the performance inferrior than previous versions of inception with evaluation of `89%` Accuracy but having more errors with `30%` Loss

![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/e58cbf03-08b7-45dd-9bad-4ccee3a52f96)


- **Prediciton :** The difference cannot be really seen here , but overall it's doing a bit worse than the other inception versions

![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/3b98b7f3-f017-4adb-96bf-bb411aa0645b)

## Comparing Model Performance : 
- The Transfer learning models are `doing exponentionally better` than the best from-scratch CNN model ( the fine-tuned version)

![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/c2e2e77d-3ceb-4b95-b540-440e791bcdaf)



## Bonus : 
- This is the confusion matrices of each models giving better context at the overall performances

![image](https://github.com/stardustyangel/Comparing-CNN-InceptionV3-on-Intel-Image-Classification/assets/89689459/0bbcf5de-2483-44ab-b6c9-aa77ba5e5aae)
  


