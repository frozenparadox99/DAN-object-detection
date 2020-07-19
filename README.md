# Deep Affinity Network

## Testing on MOT-17

![image](https://media.giphy.com/media/htX1vSaY4syvdT39Ys/giphy.gif)

## DAN Summary

- The paper mainly deals with the problem of object association in multi object tracking and utilizes the pre-existing state-of-the-art methods for object detection.
- The proposed Deep Affinity Model learns target object appearances and their affinities in a pair of video frames in an end-to-end fashion.
- The appearance modelling is responsible for hierarchical feature detection of objects while the Deep Affinity Network (DAN) estimates the affinities between the objects from in a pair of frames which can be spaced ‘n’ units ie. they can be non-successive
- Object appearance ---> 2 stream convolutional network with shared parameters
- Object affinities ---> Later layers with the hierarchical features gathered from the conv layers.

## Model Specifications

Video frames, ‘n’ timestamps apart, are sent into the network along with the centres of pre-detected objects. The initial part of the network consists of 2 VGG-like networks having shared parameters. The number of feature maps are reduced using 9 convolution layers(1 x 1 convolution kernels). The features extracted from these are then concatenated to form 2 matrices of Nm(no. Of detected objects) x 520 dimensions to ensure a 520-dimensional feature vector of each object. A tensor containing all permutations of these feature matrices is formed. The tensor is then mapped to a matrix using 5 convolutional layers. Using this matrix, 2 more matrices are generated by appending a column and a row respectively. Row and column wise softmax is performed over both these matrices and their row trimmed and column trimmed versions are computed as well. Finally, all 4 of these matrices are used in the loss computation network along with the ground truth data association matrix

## Deep Track Association

While training, the network was a 2 stream network but while deploying, it is used as a single stream network. This is possible because of the shared parameters in the 2 streams of the convolutional network. Feature matrices of frames are stored with timestamps to allow association of objects in the current frame with any possible frame. The computed affinity matrices are used to update trajectory sets by looking back deep into previous frames.
Algorithm for deep track association :

1. Initialize the track set with as many trajectories as the number of objects detected.
2. The trajectory at the tth time frame is computed using the Hungarian Algorithm
