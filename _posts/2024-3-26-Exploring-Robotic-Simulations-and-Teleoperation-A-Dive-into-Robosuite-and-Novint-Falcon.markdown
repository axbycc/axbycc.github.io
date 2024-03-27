---
layout: post
title:  "Exploring Robotic Simulations and Teloperation: A Dive into Robosuite and Novint Falcon"
author: "Peter Liu"
date:   2023-12-28 17:25:38 -0500
categories: ml-project
---

## Introduction

The eventual goal for Axby Technologies is to use machine learning to train robots to perform ordinary tasks that a human could do, such as cooking a burger. Machine-learned behaviors offer versatility, robustness, and the ability to generalize to different environments that pre-recorded trajectories do not. Because of this, it is important that we are able to test the machine-learned policies in simulated environments.

<br>
## Robosuite and Robomimic: Powering Robot Simulations and Behavior Learning

### Robosuite: A Robot Simulation Framework

Robosuite stands out as a comprehensive robot simulation framework that is powered by Mujoco. This framework provides a platform for researchers and developers to simulate and test various robotic tasks and behaviors in a controlled environment.

### Robomimic: Learning Robot Behavior in Robosuite

Complementing Robosuite is Robomimic, a machine learning library specifically designed for learning robot behavior within the Robosuite framework. It offers a range of tools and algorithms to facilitate the training and optimization of robotic actions.

<br>
## Integrating xarm7 into Robosuite

The robot we plan to use is the xarm7, so we needed to import the xarm7 into Robosuite. Thanks to Robosuite’s flexibility, I was able to integrate the xarm7 robot by utilizing the meshes and URDF from the Mujoco-menagerie package. This integration allowed me to incorporate the xarm7 into Robosuite for further experimentation and testing.

<br>
## Novint Falcon: From Video Games to Robotic Teleoperation

### The Novint Falcon: A Versatile Device

Originally designed for video games, the Novint Falcon is a haptic device known for its precision and tactile feedback. Its versatility extends beyond gaming, making it an excellent tool for teleoperation in robotic applications. However, the Novint Falcon is no longer being manufactured, and we had to acquire a model on eBay. There are similar models that also provide torque control and feedback developed by Force Dimension. Thankfully, the Force Dimension SDK supports the Novint Falcon.

Other options to generate training data were to use the keyboard or a spacemouse. The keyboard isn’t that great because it’s janky and only goes at one speed. The spacemouse is a great device because it provides rotational input, but it doesn’t provide haptic feedback. Haptic feedback is important to us at Axby because we will add force feedback to our real teleoperated data.

### Utilizing Novint Falcon in Robosuite

In my experiments, I utilized the Novint Falcon for teleoperation within the Robosuite environment to create training data. While the Novint Falcon did offer great position control, it lacked rotational control.

<br>
## Generating Training Data: xarm7 Lifting a Red Block

To train and optimize the robotic behavior within Robosuite using Robomimic, it was essential to generate relevant training data. Using the Novint Falcon, I created training data of the xarm7 robot lifting a red block. This data serves as a foundational dataset for the subsequent machine learning processes.


<p align="center">
  <img src="/media/xarm7_training.gif" alt="xarm7_training GIF">
  <figcaption style="text-align:center;">Some Training Data generated with the Novint Falcon</figcaption>
</p>
<br>
## Hyperparameter Searches: Optimizing Learning Parameters

To achieve optimal performance and accuracy in the learning process, I conducted hyperparameter searches over various parameters:

- **Learning Rate**: Two learning rates for the MLP [1e-4, 1e-5]
- **MLP Inner Layer Size**: Configuring the size of the inner layers in the neural network [128 x 128, 256 x 256, 512 x 512, 1024 x 1024]
- **Training Sequence Length**: Setting the length of the state-action sequences for learning [1, 10]

Robomimic comes with a hyperparameter configuration script that makes scanning the hyperparameters easy and straightforward. The model trained the best with an MLP size of 256 x 256 and a learning rate of 1e-5. The training sequence length didn’t make much of a difference. Despite the grid search, we were only able to consistently achieve a success rate of around 0.6. (Though we did see the highest success rate at around .8) One way to up the success-rate is to add some sort of rotational control on the gripper.

One issue I ran into previously was that using a RNN model gave terrible performance (0% - 2% success). The RNN turned out to be learning some sort of jitter in my teleoperation that caused some walk-off.

<p align="center">
  <img src="/media/generated_data.gif" alt="generated_data GIF">
  <figcaption style="text-align:center;">Machine learned policy rollouts</figcaption>
</p>
<br>
## Conclusion

The combination of Robosuite, Robomimic, and the Novint Falcon offers a powerful toolkit for simulating, learning, and teleoperated robotic behaviors. Through the integration of these tools and methodologies, we can advance our understanding and capabilities in the field of robotics, paving the way for more sophisticated and intelligent robotic systems in the future.

Stay tuned!



