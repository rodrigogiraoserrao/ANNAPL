# ANNAPL

Artificial Neural Networks in APL.

## How To Use

Clone this repository to the directory `dir`, then fire up your interpreter and create a link with the `ANNAPL` namespace:

```
      ]create ANNAPL c:\tmp\ANNAPL
Linked: #.ANNAPL ←→ c:\tmp\ANNAPL
```

After the link is successfully created, go ahead and run one of the examples:

```
      ANNAPL.examples.xor
```

## Components Available

To build a neural network you will want to create a copy of the `ANN` namespace and then
set its learning rate `lr`, `LossFn` namespace and `layers` array.

`layers` is an array of `Layer` namespaces, where each `Layer` has a `shape`
(number of inputs and number of outputs), a matrix of `weights`, a column vector of `bias`,
and an activation function namespace `ActivationFn`.
A `Layer` should be created with the `Layer.Init` dfn, which expects the layer dimensions and
returns a `Layer` namespace.
After initialising the layer, the activation function can be defined.

Finally, most activation functions are simple namespaces with two dfns.
Parametric activation functions can also be customised, e.g., the `LeakyReLU`.

### Activation Functions

Here are the activation functions implemented:

 1. ReLU
 2. LeakyReLU (parametrised)
 3. Sigmoid

### Loss Functions

Here are the loss functions implemented:

 1. MSELoss

## Example network

Let us build a network with three layers, all using different activation functions.

```apl
l1 ← ANNAPL.Layer.Init 10 20
l1.ActivationFn ← ⎕NS ANNAPL.ActivationFns.ReLU

fn ← ⎕NS ANNAPL.ActivationFns.LeakyReLU
fn.leaky ← 0.1                          ⍝ Customise the parameter of the Leaky ReLU.
l2 ← ANNAPL.Layer.Init 20 5
l2.ActivationFn ← fn

l3 ← ANNAPL.Layer.Init 5 1
l3.ActivationFn ← ⎕NS ANNAPL.ActivationFns.Sigmoid

net ← ⎕NS ANNAPL.ANN
net.layers ← l1 l2 l3                    ⍝ Assign the layers.
net.lr ← 0.03                            ⍝ Set learning rate.
net.LossFn ← ⎕NS ANNAPL.LossFns.MSELoss ⍝ Set loss function namespace.

⍝ Feed something to the network (as a column vector) and get its last output:
⊃⌽net.Feed ⍪⍳10
```
