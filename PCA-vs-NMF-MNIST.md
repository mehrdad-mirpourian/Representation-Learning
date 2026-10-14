# PCA vs. NMF on handwritten digits

A comparison of two ways to find a low-dimensional basis for a set of images:
principal component analysis, which gives an orthogonal basis with coefficients
of either sign, and non-negative matrix factorization, where the basis and the
coefficients are both constrained to be non-negative.

The question I wanted to answer is whether that non-negativity constraint costs
anything. If an image can only be built by adding parts together and never by
subtracting them, does the representation still separate the digit classes as
well as PCA does, and are the resulting components any easier to interpret?

## What's here

`PCA_vs_NMF_MNIST.ipynb` works through it on a balanced subset of MNIST, 100
training images per digit. It computes the PCA basis by SVD, fits NMF for
r = 3, 6 and 10 using the Lee & Seung multiplicative updates (maximizing a
Poisson log-likelihood, since pixel values are non-negative), classifies
held-out images by nearest neighbour in each latent space, and compares the two
sets of coefficients directly.

Both factorizations are written out by hand rather than imported from
scikit-learn, so the update rules are visible.

## Results

Nearest neighbour in the NMF coefficient space comes close to the PCA baseline
and needs fewer dimensions to get there, so the constraint doesn't destroy the
class information. The bases look completely different, though. The principal
directions are signed and global, each one mixing several digits at once, while
the NMF components come out as strokes and loops that get added together. The
coefficients follow the same pattern: about half the PCA coefficients are
negative, while the NMF ones are non-negative and mostly near zero, with only a
few components active for any given digit.

The test set is small, so the accuracy numbers should be read as "comparable"
rather than as a ranking.

## Running it

Needs `mnist_all.mat` in the same directory, plus NumPy, SciPy and Matplotlib.
In Colab, upload the .mat file to `/content` and run top to bottom.
