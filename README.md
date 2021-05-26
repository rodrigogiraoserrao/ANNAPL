# ANNAPL

Artificial Neural Networks in APL.


## How To Use

Clone this repository to a directory, say `c:\tmp`, then fire up your interpreter and create a link with the
folder as a namespace

```
      ]create ANNAPL c:\tmp\ANNAPL
Linked: #.ANNAPL ←→ c:\tmp\ANNAPL
```

After the link is successfully created, go ahead and run one of the examples:

```
      ANNAPL.examples.corner ⍬
```


## Examples

Take a look at the [examples](./examples) to see
how to use the networks.
In particular, the [corner.aplf](./examples/corner.aplf)
is a simple tradfn with a couple of lines exhibiting
how to do the simplest things.


## Components Available

To build a neural network you will need:
 - an array of layers;
   - each of which needs an activation function; and
   - a shape.
 - a loss function; and
 - a learning rate.

Use `Layer` to build layers and `Net` to build a network.

Use `_FeedForward` to feed inputs to the network
and use `_Train` to train the network on inputs and
expected targets.


### Activation Functions

Here are the activation functions implemented
in [`ActivationFns`](ActivationFns.apln):

 - `Id` – identity function
 - `ReLU` – rectified linear unit
 - `LeakyReLU` – leaky rectified linear unit (parametrised)
 - `ELU` – exponential linear unit
 - `Sigmoid`
 - `Tanh` – Hyperbolic tangent
 - `ArcTan` – Arc Tangent


### Loss Functions

Here are the loss functions implemented
in [`LossFns`](LossFns.apln):

 - `BCELoss` – binary cross entropy loss
 - `CrossEntropyLoss`
 - `L1Loss`
 - `MSELoss` – mean squared error loss
