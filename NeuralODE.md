
#SelfMadeNotes
#FullMethod
![[ResNet.PNG]]
That's a ResNet (from which NeuralODEs have been inspired) hidden layer formula, on a discrete timescale. f can be thought of as the "delta", the difference between each hidden layers.
If we suppose that we can make the timescale continous and the distance between t and t+1 as small as possible, then this would be equivalent to :
![[ResNet_continous_layers.png]]
Which means that the derivative of the hidden state is parametrized by a neural network.
The ResNet can be represented by this ODENet where each intermediate layer can be mimic by calling a kind of shared layer 