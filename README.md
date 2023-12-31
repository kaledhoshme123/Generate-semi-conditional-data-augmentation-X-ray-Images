# Generate semi-conditional data augmentation X-ray Images
- The following study, through which we can generate X-ray images of the chest region in a semi-conditional manner, by taking advantage of the probability distributions.
The study relies on borrowing a probability distribution of the images included in the dataset, and making the generator generate the medical images in a manner commensurate with the probability distribution that the discerning trains on.
- Thus, the process operates unsupervised, with the aim of generating medical images with specific specifications. In the end, we try as much as possible to adjust the generation process according to that assumed probability distribution, and the basic way to control this is to work on choosing the probability distribution (continuous or discontinuous) so that it expresses a specific goal and assuming that probability distribution on the data contained in the dataset.
- As I mentioned, the study includes at the beginning making the discriminator trained to sort the data and images contained in the dataset into specific probability distributions, and then the generator is trained to carry out the generation process in accordance with that probability distribution.
- The study is based on the use of the tensorflow probabilistic library.
- The study does not work on studying whether specific samples belong to a probability distribution (normal, uniform, or Bernoulli,....) but it works to make the generator commit that the generation is appropriate for the samples that are taken from the probability distribution.
- The goal is to make the generator generate samples from chest x-ray images, taking into account the samples taken from the probability distributions. The following study focuses on 4 cases that are adhered to during the obstetric process, and they are as follows:
- The use of the discontinuous Bernoulli distribution in order to predict whether the patient can live or not, and thus the Bernoulli distribution is appropriate for this case of diagnosis and the imposition of probability distributions.
- Using the continuous exponential distribution, with the aim of studying and determining the duration required to treat a patient infected with Coronavirus, or pneumonia. Thus, the generator is committed to generating samples for infected people who can be treated within a specified period. (The exponential distribution is suitable for studying a period of time to do a specific thing).
- Using the continuous uniform distribution: with the aim of generating people with the same disease, but the disease is presented in a specific way (I think here that this case works to transfer the details of the disease from one lung to the other). That is, it helps in re-reviewing the same disease, but in a different way.
Using a continuous regular distribution again: with the aim of extracting other features through which the disease can be presented.
- The basic idea, as mentioned above, is to assume a probabilistic distribution of the data contained in the dataset, and to study the belonging of the images included in the dataset to samples from that probability distribution.
- In the medical field, the use of a one-dimensional probability distribution is not capable of generating images with new features, because a specific change in one of the regions can lead to a complete change in the disease. That is not enough, so I moved to the use of multi-dimensional probability distributions (use 5 random variables for each probability distribution) in order to intertwine the basic features that each change and reshaping of the image can lead to according to specific samples.
- Finally, instead of using conditional entropy to compare whether the generated images are identical to the samples taken from the multivariate probability distributions, the mse loss function was used, by which the samples that the generator must adhere to during generation can be compared with the samples that the discriminator sorted through the generator.
- In order to ensure that the process and methodology used were successful, a structure for a convolutional neural network was proposed and many images were generated from the generator, and the convolutional neural network was trained on those images that were generated, and then the trained neural network was tested on the basic images included in the dataset, and got high results, and this indicates that the process is done correctly, and the semi-conditional generation operations were rather accurate.
- For training on other medical images, we need a large number of medical images, and we must know that the X-ray images are simple in terms of the information they provide compared to MRI and CT scans, which we can find difficult to generate conditionals for (since it is assumed that there are a large number of images included in the dataset).
- The multidimensional probability distributions used were not independent
## Multidimensional probability distributions
![image](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/9d5f5430-e21f-4dc1-a4a5-8c3875498452)
## Dataset Samples:
![111](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/77e3c1f9-a671-4aac-ad4e-b67d59be9000)
## Neural Network Architecture:
| #Generator    | #Discriminator    |
| :---: | :---: |
|  ![22](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/cc9c0107-d8d9-4372-977e-bfefb9078117)|  ![333](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/1c753b1a-b3fb-44ec-bfa5-2c4a72a6728d)|

### Results of Generation:
The first row (covid-19) represents the same samples, but with a change in characteristics
The second row (pneumonia) represents the same samples but with a change in features
c1: Bernoulli distribution    | c2: Exponential distribution    | c3: Uniform distribution| c4: Uniform distribution|
| :---: | :---: | :---: | :---: |
|  ![CO B](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/67ce2167-31e6-4123-8d1a-3fafbfbe46c3)|  ![CO EX](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/e73c7e54-595d-45fd-a6df-cf7781ebcd11)|  ![CO UNI1](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/be3f500c-4bde-4aa9-941d-6218a45700d5)| ![CO UNI2](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/04495a54-2127-40e8-912f-72b5994d10c9)|
| ![VI B](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/622badf8-ffe1-4267-9868-35541a52598f)| ![VI EX](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/d49a0358-83d1-4657-a97f-7170e927ec9b)|  ![VI UNI1](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/bd762484-ed54-4cf9-b93d-be9b474d0af4)| ![VI UNI2](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/e8ce27d6-40b2-49d3-be57-f57def5b9be4)|

## Classification neural network:
A neural network is trained on a large number of images that have been generated through the generator, and then that neural network is tested on the original images included in the dataset.
| Accuracy, Precision, Recall    | Classification Report    | confusion_matrix |
| :---: | :---: | :---: |
| ![image](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/27b9b6ea-2f60-4502-81c7-c1385501bd59)| ![image](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/58bb27a3-ff11-435d-8d8c-8334875aae44)| ![image](https://github.com/kaledhoshme123/Generate-semi-conditional-data-augmentation-X-ray-Images/assets/108609519/3f20a727-952f-4123-a965-0fac5e907efb)|

Dataset used: 
https://www.kaggle.com/datasets/tawsifurrahman/covid19-radiography-database
