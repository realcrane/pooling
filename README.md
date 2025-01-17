# Maneuver-Aware Pooling for Vehicle Trajectory Prediction

This project focuses on predicting the behavior of the vehicles surrounding an autonomous vehicle on highways. 
We are motivated by improving the prediction accuracy when a 
vehicles perform lane change and highway merging 
maneuvers. 

The interaction among vehicles in a given scene is generally captured using a pooling module.
This pools the LSTM states of the neighbor vehicles. 
We propose a novel pooling strategy to capture 
the inter-dependencies between the neighbor vehicles. 
Our pooling mechanism employs polar trajectory 
representation, vehicles orientation and radial velocity. 

This results in an implicitly maneuver-aware pooling operation.
We incorporated the proposed pooling mechanism into a generative
encoder-decoder model, and evaluated our method on the public 
NGSIM dataset.

#
![model image](pooling_model.png "Model overview")

## Pooling Toolbox
This project helps to reproduce 
the proposed maneuver-aware pooling strategy, in addition to other 
pooling approaches such as that used in Social LSTM , Covolutional Social Pooling and Soicla GAN works.
#
![pooling image](pooling_approaches.png "Pooling approaches")

Visualizing pooling mechanisms (A green vehicle shows the ego, 
yellow vehicle shows a neighbor covered by the pooling strategy,
and grey vehicle shows a non-covered neighbor). 
* Left: a spatial grid is centered around the ego vehicle. 
The social tensor is structured accordingly and populated
with LSTM states of the ego and exisiting neighbor vehicles. 
  The social tensor is used with [Social LSTM](http://vision.stanford.edu/pdf/alahi2016cvpr.pdf) and [Covolutional Social Pooling](https://arxiv.org/pdf/1805.06771.pdf) (CSP) works.
#  
* Center: relative positions between the ego vehicle and 
  all its neighbors are concatenated to vehicle LSTM states. This is 
  the pooling strategy used in [Social GAN](https://arxiv.org/pdf/1803.10892.pdf) work.
#  
* Right: the proposed pooling strategy where vehicle LSTM 
  states are concatenated to relative polar positions 
  (distance and angle) rather than the Cartesian representation
  used by the previous works.
  
## NGSIM Dataset Pre-processing
The NGSIM  public  dataset  is  used  for  our  experiments. The
dataset  consists  of  two  subsets:  [US-101](https://www.fhwa.dot.gov/publications/research/operations/07030/index.cfm) and [I-80](https://www.fhwa.dot.gov/publications/research/operations/06137/). 
Download the raw (.txt) files of both subsets, and then run the following MATLAB script:

```
preprocess_ngsim(us_dataset_dir, i80_dataset_dir).m
```

This will preprocess the dataset, splits it into train, validation and test subsets, 
and save that to the 'data' directory. In addition, the test subset will be further split 
to perform the maneuver-based evaluation for the keep, merge, left and right maneuvers.

## Model Arguments
The default network arguments are in:
```
model_args.py 
```
You can also set the required experiment arguments in this script. For example: 

* args['ip_dim'] selects the input dimensionality (2D or 3D).
* args['intention_module'] selects whether to predict driver intention.
* args['pooling'] sets the pooling strategy to either 'polar' (our proposed method), 
  'slstm' (Social LSTM), 'cslstm' (Convolutional Social Pooling), or 'sgan' (Social GAN).
  
## Model Training and Evaluation
The model structure is coded in 'model.py'. We extended the
[CSP](https://github.com/nachiket92/conv-social-pooling) work to: 
(1) incorporate our benchmark pooling strategies, and (2) use multi-variate Gaussian distribution. 
After setting the required experiment arguments, 
you can start model training by running:
```
train.py
```
The output model will be save in 'trained_models' directory.
To test a trained model run:
```
evaluate.py
```
which will load and test the trained model defined by the selected model arguments. The RMSE results will be saved as csv files to the 'evaluation' directory. 

## Citation
If you find this code useful for your research, please cite [our work](https://arxiv.org/pdf/2104.14079.pdf):

* Mohamed Hasan, Albert Solernou, Evangelos Paschalidis, 
  He Wang, Gustav Markkula and Richard Romano, 
  "Maneuver-Aware Pooling for Vehicle Trajectory Prediction", under review IROS 2021.

## License
This project is licensed under the MIT License - see the 
[LICENSE.md](LICENSE.md) file for details.


