# IMAGE DENOISING EXAMPLE
### Directory Structure:
1. examples directory contains all the notebooks, training code,transformer code and pipeline.ipynb files.
2. pn2v contains the utility functions.
3. unet contains the network which we are using to train the model.

## How to Train the Example in DKube.

## Step1: Create a Code
 1. Click *Repos* side menu option.
 2. Click *+Code* button.
 3. Select Code source as Git.
 4. Enter a unique name say *Img-DN*
 5. Paste link *[https://github.com/riteshkarvaloc/dkube-demo/tree/main/image-denoising/examples
 ](https://github.com/riteshkarvaloc/dkube-demo/tree/main/image-denoising/examples)* in the URL text box.
 6. Click *Add Code* button.
 7. Code will be created and imported in Dkube. Progress of import can be seen.
 8. Please wait till status turns to *ready*.

## Step2: Create a dataset
 1. Click *Datasets* side menu option.
 2. Click *+Dataset* button.
 3. Select *Others* option.
 4. Enter a unique name say *Img-DN*
 5. Enter URL as https://storage.googleapis.com/image-denoising/Convallaria_diaphragm/20190520_tl_25um_50msec_05pc_488_130EM_Conv.tif
 6. Click *Add Dataset* button.
 7. Dataset will be created and imported in Dkube. Progress of import can be seen.
 8. Please wait till status turns to *ready*.

## Step3: Create a model
 1. Click *Models* side menu option.
 2. Click *+Model* button.
 3. Enter a unique name say *Img-DN*.
 4. Select Versioning as DVS. 
 5. Select Model store as default.
 6. Select Model Source as None.
 7. Click the Add Model button.
 8. Model will be created on Dkube.
 9. Please wait till status turns to ready.


## Step4: Start a training job
 1. Click *Runs* side menu option.
 2. Click *+Runs* and select Training button.
 3. Fill the fields in Run form and click *Submit* button. Toggle *Expand All* button to auto expand the form. See below for sample values to be given in the form, for advanced usage please refer to **Dkube User Guide**.
    - **Basic Tab** :
	  - Enter a unique name say *Img-DN*
 	  - **Container** section
		- Framework - pytorch.
		- Code section - Please select the workspace *image-dn* created in **Step1**.
		- Start-up script -`python model-care.py`
    - **Repos Tab**
	    - Dataset section - Under Inputs section,select the dataset *Img-DN* created in **Step2**. Mount point: /opt/dkube/input .
	    - Model section   - Under Outputs section,select the model *Img-DN* under Outputs created in **Step3**. Mount point: /opt/dkube/output .
4. Click *Submit* button.
5. A new entry with name *Img-DN* will be created in *Runs* table.
6. Check the *Status* field for lifecycle of job, wait till it shows *complete*.

## Data Scientist Workflow :
### Steps for running the training program in IDE
1. Create a IDE with pytorch framework and version 1.6.
2. Select the Code Img-DN.
3. Under Inputs section, in Repos Tab select dataset Img-DN and enter mount path /opt/dkube/input.
4. Create a new notebook inside workspace/Img-DN/image-denoising/examples/
   - In first cell type:
     - %mkdir -p /opt/dkube/output
     - %rm -rf /opt/dkube/output/*
   - In 2nd cell type %load model-care.py in a notebook cell and then run.
5. Note for running the training more than once, please run the cell 1 again.

### How to Run Notebooks
1. Create a IDE with pytorch framework and version 1.6.
2. Select the Code Img-DN.
3. Under Inputs section, in Repos Tab select dataset Img-DN and enter mount path /opt/dkube/input.
4. Inside the directory workspace/Img-DN/image-denoising/examples/ ,Run all the cells of 1_CareTraining.ipynb and then 
run 2_CarePrediction.ipynb for predictions.


## MLE Workflow:
### Hyperparameter Tuning - Optimization of the metrics
1. Hyperparameter tuning is useful to find the appropriate parameter space for DL training. Dkube will auto generate all the possible combinations of parameters specified and runs training for each of the combination till the goal specified or max count is reached.
2. Dkube plots the graphs for comparision and suggests a best run with hyperparameters used for the run.
3. Create a run same as explained above except that now a tuning file also needs to be uploaded in the configuration tab under Parameters of the Training Job form.
4. For this example, sample tuning file is present in the github at https://github.com/riteshkarvaloc/dkube-demo/tree/main/image-denoising/examples/tuning.json. This file can be modified according to the need.

## Pipeline
1. The pipeline for this example includes training and serving stages.
2. Training: The training stage takes the image dataset as  input, train the model using UNet and outputs the model.
3. Serving: The serving stage takes the generated model and serve it with a predict endpoint for inference.

### How to run Pipeline
1. Create Code with name Img-DN as explained in Step1 above.
2. Create Dataset with name Img-DN as explained in Step2 above.
3. Create model with name Img-DN as explained in Step3 above.
4. Download the notebook from https://github.com/riteshkarvaloc/dkube-demo/tree/main/image-denoising/examples/dkube-denoising-pipeline.ipynb and upload this in default DKube IDE under pipelines folder.
5. Run all the cells of dkube-denoising-pipeline.ipynb. This will create a pipeline,and  a run.
6. Links are displayed in the output cells wherever applicable.

## Production Workflow
### How to publish model.
1. Go to Repos, click on Models and click on the model (Img-DN).
2. Click on publish icon under actions for the model version you want.
3. Fill the below details in publish model form.

### Publish model Details with a unique name IMG-DN
1. Serving image : (use default one)
2. Transformer image : (use default)
3. Transformer Code (use default)
4. Check on the transformer and fill the transformer Script : `image-denoising/examples/transformer.py`
5. Select CPU and click submit.

### Deploy Model.
1. Go to the model catalog and open the model IMG-DN
2. Lick on deploy icon.
3. Provide a name and choose CPU
4. Submit
5. The inference can be seen running in model serving tab.


### How to Test serving in DKube Webapp
1. Download data sample from https://github.com/riteshkarvaloc/dkube-demo/tree/main/image-denoising/examples/img1.png
2. Open the URL https://:32222/inference.
3. Copy the serving endpoint from the model serving tab and paste it into the serving the URL field.
4. Copy token from developer settings and paste into token field.
5. Select model steel from the dropdown.
6. Upload the downloaded image and click predict.

### Custom Webapp
1. Go to webapp directory, and build a docker image with given **Dockerfile** by typing **docker build . -t ocdr/streamlit-webapp:img-dn**
2. Run command **docker run -p 8501:8501 ocdr/streamlit-webapp:img-dn**
3. Open http://localhost:8501/ in your browser and copy serving endpoint from the model serving tab and paste it into Dkube serving URL field , fill authentication token and upload image from (https://github.com/riteshkarvaloc/dkube-demo/tree/main/image-denoising/examples/img1.png) and click predict.
4. Denoised image will be returned as an output and will be displayed in the UI.
