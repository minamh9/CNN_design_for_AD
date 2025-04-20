# Alzheimer’s Disease Detection with CNNs

This project reproduces the paper:
**On the Design of Convolutional Neural Networks for Automatic Detection of Alzheimer’s Disease**
[Link](https://proceedings.mlr.press/v116/liu20a.html) to original paper


## 📁 Project Overview

This repository contains code and instructions for:

*	Preparing the ADNI dataset.
*	Preprocessing imaging data.
*	Training and evaluating 3D CNN models for Alzheimer’s Disease classification.
* instance normalization outperforms batch normalization  
 

## 🧠 Data Preparation

**1. Register for the ADNI Dataset**

To access the ADNI dataset:

* Register at the LONI Image & Data Archive (IDA).
* Submit a request for access via the ADNI application form.

**2. Download Image and Clinical Data**

Follow the instructions from Clinica’s ADNI2BIDS guide and refer to the original GitHub instructions for dataset format requirements.

**3. Install Required Software**

Install the necessary Python packages:

```
pip install -r requirements.txt
```
Install Miniconda, Clinica, and dcm2niix:

```
curl https://repo.anaconda.com/miniconda/Miniconda3-latest-MacOSX-x86_64.sh -o /tmp/miniconda-installer.sh
bash /tmp/miniconda-installer.sh
conda create --name clinicaEnv python=3.10
conda activate clinicaEnv
pip install clinica
pip install dcm2niix
```
Install SPM and MATLAB

Note: Clinica v0.9.3 has issues with SPM standalone. We recommend using [SPM12](https://aramislab.paris.inria.fr/clinica/docs/public/dev/Software/Third-party/#spm12) and [Matlab](https://aramislab.paris.inria.fr/clinica/docs/public/dev/Software/Third-party/#matlab), or [SPM standalone](https://aramislab.paris.inria.fr/clinica/docs/public/dev/Software/Third-party/#spm12-standalone) .


**4. Convert Data to BIDS Format**
```
conda activate clinicaEnv
clinica -v convert adni-to-bids MRI_IMAGE_DIR CLINICAL_DATA_DIR CONVERTED_OUTPUT_DIR -m T1
```
More details: [Clinica ADNI2BIDS Conversion](https://aramislab.paris.inria.fr/clinica/docs/public/dev/Converters/ADNI2BIDS/)

**5. Preprocess Data**

Use the generate_split_all function in preprocess/scripts/adni_meta_parse.py to generate train/val/test TSV files, then run:

```
export SPM_HOME="SPM_INSTALLED_PATH"
export PATH=/Applications/MATLAB_R2019a.app/bin:$PATH
clinica run t1-volume CONVERTED_OUTPUT_DIR ./ADNI_processed TRAIN -tsv ./ADNI_converted_meta_all/sample_30/Train_ADNI.tsv -wd './WD_train' -np 1
clinica run t1-volume-existing-template CONVERTED_OUTPUT_DIR ./ADNI_processed TRAIN -tsv ./ADNI_converted_meta_all/sample_30/Val_ADNI.tsv -wd './WD_val' -np 1
clinica run t1-volume-existing-template CONVERTED_OUTPUT_DIR ./ADNI_processed TRAIN -tsv ./ADNI_converted_meta_all/sample_30/Test_ADNI.tsv -wd './WD_Test' -np 1
```
## 🏋️ Model Training & Evaluation

  1.	Upload the preprocessed BIDS data and metadata (TSV files) to your Google Drive.
	2.	Open [train-test.ipynb](https://github.com/minamh9/CNN_design_for_AD/blob/main/train-test.ipynb) in Google Colab.
	3.	Mount your Google Drive and navigate to the project directory.
	4.	Run the !pip install -r requirements.txt cell to install dependencies.
	5.	Follow the Model Training section to train the models using the desired configuration.
	6.	In the Model Evaluation section, test and visualize model performance with ROC curves and accuracy metrics.

## 🔍 Notes

* The models are built using PyTorch and designed for 3D input volumes.
* Preprocessing steps and training pipelines are aligned with the methodology in the original paper.
* Results may vary depending on the ADNI subset and preprocessing quality.

 ## 📌 Acknowledgments
 
This codebase is adapted from:
https://github.com/NYUMedML/CNN_design_for_AD
with modifications for easier reproducibility and improved documentation.

