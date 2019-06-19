# Deep-Learning-Contrastive-Center-Loss-Transfer-Learning-Food-Classification-
This repository contains the ipnyb for a project on deep learning visual classification of food categories.

## 1.0 General Overview
The Food59 dataset contains approximately 30,000 images from 59 classes, and the goal was to design an approach to improve the performance of the baseline model trained using ResNet18. Based on the results obtained from the base model, the top 1 validation accuracy was at 0.889, while the top 5 validation accuracy was at 0.983.

## 2.0 Technical Novelties
### 2.1 Fine-Tuning Model on External Food101 Dataset
According to Cui et al [1], although large scale image datasets such as ImageNet contains a large amount of data, these images are typically generic and "contain objects in the center with similar scale and simple backgrounds". Additionally, the authors of the paper discovered that datasets are often biased in terms of their content and style, which might affect the performance of transfer learning on another dataset that contains images that differs significantly. Consequently, the authors managed to achieved better transfer learning by fine-tuning the networks trained on selected subsets of data based on the paper's proposed measure to estimate domain similarity.

Motivated by the proposed methods in the paper, an attempt was made to fine-tune the model based on a dataset similar to Food59 prior to training end-to-end on the Food59 dataset. Based on the paper by Bossard, L. et al [2], a challenging dataset with 101 food categories with a total of 101,000 images, where each class has 750 training images and 250 test images, was introduced. Similar to the Food59 dataset, the images in the Food101 dataset contained noises in the data that is common across food images.

Therefore, based on the discussion above, our model was first fine-tuned on the Food101 dataset prior to conducting end-to-end training on the Food59 dataset in hope of achieving better results.

### 2.2 Contrastive-Center Loss
One of the challenges in visual classification and recognition task is to obtain more discriminative features to improve the performance of the deep neural networks. One strategy implemented by various papers are the introduction of contrastive loss [3], which is a distance-based loss function that penalises on the distance between semantically similar examples, as well as the introduction of center loss [4], which learns a center for the deep features of each class and penalizes the distances between the deep features and their corresponding class centers.

Following the introduction of the two losses above, according to Qi, C. & Su, F. [5], the proposed contrastive-center loss was proposed to "consider intra-class compactness and inter-class separability, by penalizing the contrastive values between: (1) the distances of training samples to their corresponding class centers, and (2) the sum of the distances of training samples to their non-corresponding class centers." Discriminative features within the same class should therefore have low separability while discriminative features of different classes should have high separability.

In this case, the contrastive-center loss was proposed to not only consider inter-class variability, but also to consider intra-class variability.

Formally, the formula of the contrastive-center loss can be seen below:
![A2-1](https://user-images.githubusercontent.com/50171205/59734591-9d113600-9284-11e9-8d91-d5945ff18910.png)

Where Lct−c denotes the contrastive-center loss. m denotes the number of training samples in a min-batch. xi ∈ Rd denotes the ith training sample with feature dimension d. yi denotes the label of xi. cyi ∈ Rd denotes the yith class center of deep features with dimension d. k denotes the number of class. δ is a constant used for preventing the denominator equal to 0.

## 3.0 Results
With the implementation of the above novelties, we were able to achieve the best top 1 validation accuracy of 0.891. Compared with the base model with its best top 1 validation accuracy at 0.889, this is 0.02 higher.
![A2-2](https://user-images.githubusercontent.com/50171205/59734630-cb8f1100-9284-11e9-944d-344fa767c863.png)

While there was a slight improvement, it did not exceed expectation. The following paragraphs will delineate the possible reasons for the slight improvement, as well as possible justifications on why the results did not exceed expectations based on the proposed novelties.

## 4.0 Discussion on Results
### 4.1 Fine-Tuning on External Food101 Dataset
Even though the model was fine-tuned on a dataset of a similar domain - food, there could be other factors that prevented the fine-tuned model to generalise well over the Food59 dataset. One possible reason is that the Food59 dataset contains many local food categories that are not common across the Food101 dataset. In particular, the Food59 dataset contains images of food from a fusion of multiple types of cuisine, such as Western and Asian. On the other hand, while the Food101 dataset does contain several classes of food from both Western and Asian cuisine, a much larger proportion of them are Western food categories.

Additionally, the Food59 dataset contains drinks, such as americano and instant coffee as food classes, while the Food101 dataset does not contain drinks.

However, for future work, it could be possible to tap onto other well-balanced dataset that contains food classes that are similar to that of Food59 for fine-tuning. For example, while searching for datasets that are of a similar domain with Food59, there was another food dataset called the ChineseFoodNet dataset. However, the data was unavailable for download. By integrating several food dataset where the model can be pre-trained or fine-tuned on, it could possibly lead to a better classification result.

### 4.2 Application of Contrastive Center Loss
The Contrastive Center Loss was a proposed loss to improve upon other training strategy to obtain more discriminative features. Not only does it consider intra-class compactness, but it also considers inter-class separability. Theoretically, this loss function would be ideal in performing image classification where there are low variability of examples across classes and / or high variability of examples within a class.

In this case, within the Food59 dataset, there are several confusing inter-classes of food. For example, some images of instant coffee looks very similar to images of kopi-o. Furthermore, some images within a class appear to be different as well. For example, within the food class of braised pig skin, there are several food images that do not look like braised pig skin.

As a result, this allows the model to perform better when using contrastive center loss than if only cross-entropy loss is used.

## References
[1] Cui, Yin & Song, Yang & Sun, Chen & Howard, Andrew & Belongie, Serge. (2018). Large Scale Fine-Grained Categorization and Domain-Specific Transfer Learning. 4109-4118. 10.1109/CVPR.2018.00432.

[2] Bossard, L., Guillaumin, M., & Gool, L.V. (2014). Food-101 - Mining Discriminative Components with Random Forests. ECCV.

[3] Chopra, S., Hadsell. R. & LeCun, Y. (2005) Learning a Similarity Metric Discriminatively, With Application to Face Verification," 2005 IEEE Computer Society Conference on Computer Vision and Pattern Recognition (CVPR'05), San Diego, CA, USA, 2005, pp. 539-546 vol. 1. 10.1109/CVPR.2005.202

[4] Wen, Yandong & Zhang, Kaipeng & Li, Zhifeng & Qiao, Yu. (2016). A Discriminative Feature Learning Approach for Deep Face Recognition. LNCS. 9911. 499-515. 10.1007/978-3-319-46478-7_31.

[5] Qi, C. & Su, F. (2017) Contrastive-Center Loss For Deep Neural Networks. 2017 IEEE International Conference on Image Processing (ICIP), Beijing, 2017, pp. 2851-2855. 10.1109/ICIP.2017.8296803
