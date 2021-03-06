---
title: "01: Practical Operation Extraction from Electromagnetic Leakage
for Side-Channel Analysis and Reverse Engineering by Piieter Robyns, Mariona Di Martino, Dennis Giese, Wim, Lamotte, Peter Quax, Guevara Noubir"
date: 2020-08-30
type: book
commentable: true

# Provide the name of the presenter
summary: "Presenter(s): Japhet Ye, Alvin Yang"

# Provide other tags that describe the paper
tags:
- teaching
- ee693e
---

***
## Paper Summary
In Reverse Engineering, the ability to determine the operation that is being executed on a black-box device is highly important to successfully determine the type of device it is. In this paper, the authors use a side-channel attack that analyzes the electromagnetic (EM) leakage of a processor and using a Machine Learning classification network to determine the operation that is being executed on the processor. They provided 66GB worth of complex traces of EM transmissions of operations that were executed on a Microcontroller with custom firmware. They propose a Convolutional Neural Network as the classifier, with an emphasis on the timing of the operation being executed. They compared this CNN with two other classification strategies to determine the effectiveness of their network. In the end, the have favorable results in a black-box scenario with the CNN models being 52.29% accurate.
***

## Presentation
{{< youtube w7Ft2ymGmfc >}}

[PROVIDE A VIDEO RECORD OF YOUR PRESENTATION]
***

## Review
### Strengths
- Greatly goes into the strengths and weaknesses of each of the strategies that they tested.
- Provides in-depth background of the methods used by their models in order.

### Weaknesses
- Paper heavily relies on the fact that adversaries would be able to gather data to train the networks, they themselves having a training dataset of 66GB for only a few operations.


### Detalied Comments
The authors go into great depth on the background information that they used to create the model in  their experiments, by providing links and also quoting the necessary details from the background models that they used, such as the convolution model that they based their convolutional layers on. They also explain clearly how their version is different and how they changed the base model to fit their specific needs through modification of the input, as well as some tweaks to the convolutional layers themselves. 
Their analysis of each of  the classification strategies goes in depth on why they believe the results are as they are. In particular, how much the noise they were able to correctly identify in the CNN models where the mean-squared approach completely failed in matching anything and flagging all the inputs as noise.
However, their model heavily relies on the adversary already having such models with a decent amount of data that will require expensive equipment to obtain. In their training data,  they used a software defined radio to capture EM traces of a microcontroller processor. The radio was placed only a few centimeters from the processor where their amplitude and frequency readings were had a decent resolution. They themselves also say that a good follow-up study was to see if a more powerful adversary would be capable of capturing EM leakage from significantly farther distance from very powerful sensors. So while the paper has proven that it is possible to reverse engineer a black-box hardware module via analyzing EM-leakage, the security concerns with a more powerful adversary was not discussed in this paper.

### Implementation
The authors provide their [source code](https://github.com/rpp0/em-operation-extraction) to us. The language of choice for the analysis is Python and in the code repository, have provided the code for their zero-mean normalized cross-correlation and mean squared error analysis, as well as the code for their 1-D, 2-D, and 1-D with classification Convolutional Neural Networks along with their training algorithm for all the networks. They relied on TensorFlow for the backend of their CNN's by creating a TensorFlow implementation of another paper's CNN called [WaveNet] (https://deepmind.com/blog/article/wavenet-generative-model-raw-audio) whose model theirs are based on. 

The authors also provided their [training and testing datasets](http://wisecdata.ccs.neu.edu/) that they used. The datasets are compromised of EM traces during the execution of their chosen operations. An example trace can be seen below.

{{ < figure src="https://github.com/gustybear-teaching/course_ee693e_2020_fall/raw/main/week_08/images/emtraceex.png" title="Example EM Trace" width="300> }}

The architecture of their CNN can be found in the figure below. They feed the multi-dimensional data through 8 convolutional layers to extract different features, mainly instantaneous amplitude and the Short Time Fourier Transform (STFT), to be filtered down to the fully connected network that will then classify the frame. When designing the CNN, they had a few challenges they needed to overcome. Firstly, due to the variability of the operation executions, not all of the traces are of the same length. The authors decided to make the input of fixed length to accommodate the biggest trace size. Because of this, each of the convolutional layers have reduced filters to make the computation more tractable. In compensation, the inputs are widened and pooling layers added so that there is at best equal loss to the base model.

{{ < figure src="https://github.com/gustybear-teaching/course_ee693e_2020_fall/raw/main/week_08/images/cnnarch.png" title="CNN architecture" width="300> }}

The authors also describe two other CNN's for testing, a 2D CNN and another 1D CNN that also extracts extra features. The purpose of the 2D CNN is to determine whether or not the STFT is a viable input feature to a CNN. To accomplish this, the input is given a second dimension by overlapping each of the traces by a 512-point STFT with the overlap being 50%, resulting in a 512x512 vector as an input. The purpose of the third CNN was to see if they can reasonably identify sub-operations in the trace. While the entire trace may be classified as a SHA1 operation, there are sub-operations within the SHA1 trace that the network does not pick out as features. This CNN's goal is to also classify sub-operations and boxing in the sub-operation in the trace.

### Experimentation
[PROVIDE FIGURES/TABLE/WRITTEN-PROOF FROM THE PAPER]

The first thing that the authors did was collect EM traces from a NodeMCU Amica microcontroller using a USRP B210. The Microcontroller has modified firmware that contains the target operations that the researchers are targeting. The EM receiver was then placed near the microcontroller processor and an operation was executed in a time frame. The EM receiver took two measurements simultaneously. An instataneous amplitude reading and a EM spectrum reading, as shown in the figure below. 

{{< figure src="https://github.com/gustybear-teaching/course_ee693e_2020_fall/raw/main/week_08/images/exampledata.png" title="Example EM Trace" width="300" >}}

These traces are then divided into two datasets, the training dataset and the test dataset. Each of the models are trained with the training dataset and then tested to see if the accuracy of the labeling of the network. There were two tupes of test conducted, one with a grey-box adersarial model, where the adversary had some idea of the operations being executed, and one with a black-box adversarial model, where the adversary had no idea of the operations being executed. The grey box model is assumed when they are testing the accuracy of their CNN's to classical template matching strategies. 

The authors have provided the confusion matrixes for each of the models as shown in the figures below.

{{< figure src="https://github.com/gustybear-teaching/course_ee693e_2020_fall/raw/main/week_08/images/zeromeancon.png" title="Zero-Mean Cross-Correlation Confusion Matrix" width="300" >}}
{{< figure src="https://github.com/gustybear-teaching/course_ee693e_2020_fall/raw/main/week_08/images/msecon.png" title="Mean Squared Error Confusion Matrix" width="300" >}}
{{< figure src="https://github.com/gustybear-teaching/course_ee693e_2020_fall/raw/main/week_08/images/1dcon.png" title="1D CNN Confusion Matrix" width="300" >}}
{{< figure src="https://github.com/gustybear-teaching/course_ee693e_2020_fall/raw/main/week_08/images/2dcon.png" title="2D CNN Confusion Matrix" width="300" >}}
{{< figure src="https://github.com/gustybear-teaching/course_ee693e_2020_fall/raw/main/week_08/images/1dwboxcon.png" title="1D CNN with bounding boxes Confusion Matrix" width="300" >}}
{{< figure src="https://github.com/gustybear-teaching/course_ee693e_2020_fall/raw/main/week_08/images/2dwboxcon.png" title="2D CNN with bounding boxes Confusion Matrix" width="300" >}}

The authors then tested how well the CNN's would do in a black-box adversary model. To do this, they injected snippets of a WiFi connection EM trace from the training dataset in between random snippets of the test dataset to rule out the possibility of the models relying on knowing the timing to classify operations as in the grey box model.

The cumulated results from the paper results in the CNN's fairing 96.47% accuracy in the grey-box scenario, and 55.29% accuracy in the black-box scenario. These results are rather excellent as it proves that their models can help reverse engineering and possibly other adversaries in distinguishing operations.

### Questions From the Audience