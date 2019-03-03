# Convolutional Layer
When dealing with high-dimensional inputs such as images, as we saw above it is impractical to connect neurons to all neurons in the previous volume. Instead, we will connect each neuron to only a local region of the input volume. The spatial extent of this connectivity is a hyperparameter called the receptive field of the neuron (equivalently this is the filter size). The extent of the connectivity along the depth axis is always equal to the depth of the input volume. It is important to emphasize again this asymmetry in how we treat the spatial dimensions (width and height) and the depth dimension: The connections are local in space (along width and height), but always full along the entire depth of the input volume.

Parameter Sharing. Parameter sharing scheme is used in Convolutional Layers to control the number of parameters. Using the real-world example above, we see that there are 55*55*96 = 290,400 neurons in the first Conv Layer, and each has 11*11*3 = 363 weights and 1 bias. Together, this adds up to 290400 * 364 = 105,705,600 parameters on the first layer of the ConvNet alone. Clearly, this number is very high.

It turns out that we can dramatically reduce the number of parameters by making one reasonable assumption: That if one feature is useful to compute at some spatial position (x,y), then it should also be useful to compute at a different position (x2,y2). In other words, denoting a single 2-dimensional slice of depth as a depth slice (e.g. a volume of size [55x55x96] has 96 depth slices, each of size [55x55]), we are going to constrain the neurons in each depth slice to use the same weights and bias. With this parameter sharing scheme, the first Conv Layer in our example would now have only 96 unique set of weights (one for each depth slice), for a total of 96*11*11*3 = 34,848 unique weights, or 34,944 parameters (+96 biases). Alternatively, all 55*55 neurons in each depth slice will now be using the same parameters. In practice during backpropagation, every neuron in the volume will compute the gradient for its weights, but these gradients will be added up across each depth slice and only update a single set of weights per slice.

Notice that if all neurons in a single depth slice are using the same weight vector, then the forward pass of the CONV layer can in each depth slice be computed as a convolution of the neuron’s weights with the input volume (Hence the name: Convolutional Layer). This is why it is common to refer to the sets of weights as a filter (or a kernel), that is convolved with the input.

**Notice that the parameter sharing assumption is relatively reasonable: If detecting a horizontal edge is important at some location in the image, it should intuitively be useful at some other location as well due to the translationally-invariant structure of images. There is therefore no need to relearn to detect a horizontal edge at every one of the 55*55 distinct locations in the Conv layer output volume.**

Note that sometimes the parameter sharing assumption may not make sense. This is especially the case when the input images to a ConvNet have some specific centered structure, where we should expect, for example, that completely different features should be learned on one side of the image than another. One practical example is when the input are faces that have been centered in the image. You might expect that different eye-specific or hair-specific features could (and should) be learned in different spatial locations. In that case it is common to relax the parameter sharing scheme, and instead simply call the layer a Locally-Connected Layer.

**Summary**. To summarize, the Conv Layer:

-   Accepts a volume of size  W1×H1×D1
-   Requires four hyperparameters:
    -   Number of filters  K,
    -   their spatial extent  F,
    -   the stride  S,
    -   the amount of zero padding  P.
-   Produces a volume of size  W2×H2×D2  where:
    -   W2=(W1−F+2P)/S+1
    -   H2=(H1−F+2P)/S+1  (i.e. width and height are computed equally by symmetry)
    -   D2=K
-   With parameter sharing, it introduces  F⋅F⋅D1  weights per filter, for a total of  (F⋅F⋅D1)⋅K  weights and  K  biases.
-   In the output volume, the  dd-th depth slice (of size  W2×H2) is the result of performing a valid convolution of the  d-th filter over the input volume with a stride of  S, and then offset by  d-th bias.

# Pooling Layer

It is common to periodically insert a Pooling layer in-between successive Conv layers in a ConvNet architecture. Its function is to progressively reduce the spatial size of the representation to reduce the amount of parameters and computation in the network, and hence to also control overfitting. The Pooling Layer operates independently on every depth slice of the input and resizes it spatially, using the MAX operation. The most common form is a pooling layer with filters of size 2x2 applied with a stride of 2 downsamples every depth slice in the input by 2 along both width and height, discarding 75% of the activations. Every MAX operation would in this case be taking a max over 4 numbers (little 2x2 region in some depth slice). The depth dimension remains unchanged. More generally, the pooling layer:

-   Accepts a volume of size  W1×H1×D1
-   Requires two hyperparameters:
    -   their spatial extent  F,
    -   the stride  S,
-   Produces a volume of size  W2×H2×D2 where:
    -   W2=(W1−F)/S+1
    -   H2=(H1−F)/S+1
    -   D2=D1
-   Introduces zero parameters since it computes a fixed function of the input
-   For Pooling layers, it is not common to pad the input using zero-padding.

# Normalization Layer

Many types of normalization layers have been proposed for use in ConvNet architectures, sometimes with the intentions of implementing inhibition schemes observed in the biological brain. However, these layers have since fallen out of favor because in practice their contribution has been shown to be minimal, if any. For various types of normalizations, see the discussion in Alex Krizhevsky’s  [cuda-convnet library API](http://code.google.com/p/cuda-convnet/wiki/LayerParams#Local_response_normalization_layer_(same_map)).

# Fully-connected layer

Neurons in a fully connected layer have full connections to all activations in the previous layer, as seen in regular Neural Networks. Their activations can hence be computed with a matrix multiplication followed by a bias offset.
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTMwNDQ1MzgxMywxNDQ1MTc2NzU0LDYyMj
I4NzQ5NSwtNDM2ODg3NzE5LC0xMzE3MTExMjEyLC0zNjg4ODQ3
MzIsLTE4NTI4Mjg4ODVdfQ==
-->