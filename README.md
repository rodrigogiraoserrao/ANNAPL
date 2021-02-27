# ANNAPL

Artificial Neural Networks in APL.


## How To Use

Clone this repository to a directory, say `c:\tmp`, then fire up your interpreter and create a link with the `ANNAPL` namespace:

```
      ]create ANNAPL c:\tmp\ANNAPL
Linked: #.ANNAPL ←→ c:\tmp\ANNAPL
```

After the link is successfully created, go ahead and run one of the examples:

```
      ANNAPL.examples.circle
```


## Example network

Let us build a network with two layers and train it over some sample data.
Notice that the input vectors to the network must be column vectors.

```apl
      l1 ← ANNAPL.Layer.Init 2 4 
      l1.ActivationFn ← ⎕NS ANNAPL.ActivationFns.LeakyReLU  
      l1.ActivationFn.leaky ← 0.1      ⍝ Customise the parameter of the function.

      l2 ← ANNAPL.Layer.Init 4 1
      l2.ActivationFn ← ⎕NS ANNAPL.ActivationFns.Sigmoid

      net ← ⎕NS ANNAPL.ANN
      net.layers ← l1 l2                        ⍝ Assign the layers.
      net.lr ← 0.03                             ⍝ Set the learning rate.
      net.LossFn ← ⎕NS ANNAPL.LossFns.MSELoss  ⍝ Set loss function namespace.

      data ← ?1000 2⍴0                          ⍝ Create some random 2d points inside a 1×1 square.
      3 2⍴data
0.6057999908 0.3874385354
0.6464077634 0.8646932264
0.2874541592 0.3140257194
      outs ← 1>+/data                           ⍝ Label with 1 those in the lower left half and with 0 the others.
      3⍴outs
1 0 1

      net.Feed⍉1↑data                           ⍝ Compute the forward pass of the network on the first data point...
┌────────────┬──────────────┬────────────┐
│0.6057999908│¯0.02990077416│0.3520214044│
│0.3874385354│¯0.04273491875│            │
│            │¯0.01708205292│            │
│            │¯0.06383148307│            │
└────────────┴──────────────┴────────────┘
      net.Output⍉1↑data                         ⍝ or simply compute the network output.
0.3520214044

      ⍝ To compute accuracy on the last 100 data points (test data).
      A ← {(⊢⌹=⍨)(900↓outs)=⌊0.5+∊ ⍵.Output∘⍪¨↓900↓data}

      A net    ⍝ Accuracy with no training is similar to random guessing.
0.52

      ⍝ Train once over first 900 data points (training data)
      {}(900↑outs) net.Train∘⍪¨ (↓900↑data)
      A net
0.71
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

 - `Id` – identity function
 - `ReLU` – rectified linear unit
 - `LeakyReLU` – leaky rectified linear unit (parametrised)
 - `ELU` – exponential linear unit
 - `Sigmoid`
 - `Tanh` – Hyperbolic tangent
 - `ArcTan` – Arc Tangent

### Loss Functions

Here are the loss functions implemented:

 - `BCELoss` – binary cross entropy loss
 - `CrossEntropyLoss`
 - `L1Loss`
 - `MSELoss` – mean squared error loss
