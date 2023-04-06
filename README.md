How to run:
Get these files into a google drive directory titled Neural Networks Project:

ConvolutionTry.ipynb

RawPercentageConvolution.ipynb

input _data.csv

output_data.csv

input _data_conv.csv

output_data_conv.csv

Convolution_best_model_conv.pth

Convolution_best_model.pth

To get the output for one validation sample for which ever ipynb file you want to run, run all cells before the cell titled “The main training and evaluation code starts here” and the one after. The one after will give you an output from a validation sample and its ground truth value

	

The original intent of my project was to try and predict the relative rates of return of asset prices (bonds, stocks, real estate, bank account)
based on time series data. To do this I first tried building a RNN. I tried changing the number of layers, embedding space, etc. However, my network
learned to always predict the same value regardless of what the input was. I decided to switch to a CNN because it can also process sequence data. 
I ran into the same issue with this network. I tried many things to get this to work. I will explain the architecture of my last try at getting a 
network to predict relative price movements. This is in the file ConvolutionTry.ipynb. I used 5 1d convolutional layers (by recommendation of 
Professor Czajka) followed by 3 fully connected layers. After the convolutional layers I used maxpooling to reduce the dimensionality. The convolution
layers used ReLU activation function, the first two fully connected layers used a tanh activation function and on the last layer I used sigmoid 
to push the values between 0 and 1. I used no normalization or dropout as I was just trying to see if there was any signal the network could learn 
at all. The loss function I used was mean squared error because this was a regression problem not a classification problem. The optimizer I used
was Adam because we were shown in class that it will likely work better than something like SGD. 

Since this isn’t a classification task there is no accuracy, but the testing average loss was .2142 while the validation average loss was .2040. 
Since the network always predicts the same things, I think it just so happens that the validation data tends to be closer to that one output.

I now want to discuss all the things I tried to get it to work. I tried adding dropout and normalization. I tried reducing the number of convolution
layers. I altered the convolution kernel sizes and number of channels. I had my network go through the normalization algorithm (min max) in the forward
method. I tried normalizing the input sequences. I tried both min max and also dividing by the largest value in the sequence (this would keep the 
percentage change between time steps the same which is often considered important information). I tried different normalizations on the intended 
output (min max and divide by largest).  I tried adding and removing input sequences. I tried only predicting the relative changes of stocks and 
bonds. I tried changing the learning rate from very low ~.0000000001 to 1. I tried changing the activation functions of the fully connected layers.
I tried reducing the number of fully connected layers.

One thing that I was able to get work is predicting the actual percent changes of assets. I made a network that would make a prediction of what the 
percent change in stocks and bonds were. The architecture of this network is just a result of tinkering to get something that didn’t suffer from the
issue of the same output. I used 3 convolution layers followed by dropout and maxpool. I tried dropout because I asked chatgpt ways to help with my 
problem and it gave me that as a possibility. I used a dropout rate of .3 because that was on the lower end of what chatgpt said might be a good range
for dropout. I then followed this up with one fully connected layer. I tried to keep the number of weights in the network low because I only have around
4000 data points. This network did make predictions based on the input data. This is the network created in the RawPercentageConvolution.ipynb file.
I had the network predict the percentage like 8 not .8 and my average training loss was 11.17  while my validation average loss was 15.82 The validation
loss was higher as it was obviously predicting things it hadn’t seen before, but the average training loss and prediction loss were not that far off from
each other.The loss of 13.90 is not very good. When you generate responses it almost seems like they are just random. When you set the learning rate high 
on this model, the same thing happens as the other models and everything is predicted to be the same.

WIth either of these models I will not be trying to get the losses closer together and to generalize better because I will be changing my project to
music genre classification. I don’t think that there is any signal in the data for the network to pick up on. Part of the reason for this lack of generalization
might also be because of the lack of data

I am a team of one so I did all the work.
