# How Computers Got Good at Recognizing Images

A technical walkthrough of how deep convolutional neural networks work, anchored on the 2012 **AlexNet** breakthrough that kicked off the modern deep learning boom.

## The basics: neurons, backpropagation, MNIST

A neuron takes a weighted sum of inputs, adds a bias, and applies a non-linear activation function (e.g. sigmoid) — without that non-linearity, no stack of neurons could do more than a linear network can. **Backpropagation** (popularized by Hinton, Rumelhart, and Williams in a landmark 1986 paper) trains a network by computing, layer by layer working backward from the output, how much each weight contributed to the error, then nudging weights to reduce it — repeated over thousands to billions of examples.

A minimal illustration: a 2-layer, 25-neuron, ~12,000-parameter network can recognize handwritten digits from the MNIST dataset (60,000 labeled 28×28 images) at 95-98% accuracy, in about 74 lines of plain Python.

## AlexNet: the 2012 turning point

The annual **ImageNet competition** (1M+ images, ~1,000 categories) saw slow progress in 2010-2011 (28% then 25% top-5 error rate, both from non-neural-network approaches). In 2012, a University of Toronto team (Alex Krizhevsky, advised by Hinton) submitted a deep convolutional network — "AlexNet" — that scored **16% error**, blowing away the next-best 26%. It had 8 layers, 650,000 neurons, and 60 million parameters (vs. ~12,000 in the toy MNIST network above), trained across two GPUs over 5-6 days.

## Why convolutional: shared weights and feature detectors

A fully-connected network (like the MNIST example) doesn't generalize position — it has no efficient way to recognize the same pattern in a different part of an image. **Convolutional layers** fix this: a small "feature detector" (e.g. an 11×11-pixel receptive field in AlexNet's first layer) scans across the whole image using the *same* weights at every position — like sliding a stencil around and checking for a match everywhere, rather than training a separate detector per position. AlexNet's first layer had 96 such feature detectors, each producing its own feature map.

Layers stack in complexity: the first layer detects edges, gradients, and simple textures; the second combines those into shapes like circles; by the fifth layer, detectors respond to complex, semantically meaningful patterns — dog faces, car wheels, human silhouettes, corporate logos, even backgrounds like grass (useful for inferring "this is probably an animal photo" indirectly). The final layers are fully-connected again, synthesizing all the feature maps into a classification.

## Why this actually works — and its limits

Convolutional networks have no built-in understanding of geometry, rotation, scale, or lighting. They achieve translation-invariant recognition purely through brute-force exposure to enough labeled examples in enough positions — not unlike, the piece argues, the human visual system's own crude pattern-matching (illustrated by the classic upside-down-face illusion, where flipped eyes/mouth look normal upside down but "freakishly misshapen" right-side up). A 2015 Google experiment ("Inceptionism") that ran a trained network backward to generate images maximizing a "dumbbell" detector produced dumbbells *always attached to a muscular arm* — because the training images never showed a dumbbell without one, illustrating how these networks lean on contextual correlation, not object understanding.

## Why it needed so much compute

Training AlexNet-scale networks requires massively parallel matrix multiplication — exactly what GPUs (built for rendering video game graphics) are good at. This is the underlying reason Nvidia and AMD's fortunes rose with deep learning, and why Google (TPUs), Tesla, and Apple all built custom AI silicon in the years following AlexNet.

## Cross-reference

Part of the [[The Origins of Generative AI]] cluster. This is the technical companion to [[AlexNet and the Three Nonconformists Who Built the Deep Learning Boom]], which tells the human/historical side of the same AlexNet story — Fei-Fei Li's ImageNet dataset and Jensen Huang's CUDA platform being the two other ingredients (alongside Hinton's backpropagation) that made AlexNet possible. For where this sits in the wider chronology see [[Deep Learning Timeline (1982-2024)]]; for how the field moved from these narrow classifiers to generative models see [[From Data-Driven Software to Generative AI]].

## Sources
- [How computers got shockingly good at recognizing images](<../../source/How computers got shockingly good at recognizing images.md>)

#ai-techniques #deep-learning #computer-vision #neural-networks
