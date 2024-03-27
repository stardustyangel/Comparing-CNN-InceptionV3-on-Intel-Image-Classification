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

# Modelling : 
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
