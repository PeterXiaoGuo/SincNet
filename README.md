# SincNet
SincNet is a neural architecture for processing **raw audio samples**. It is a novel Convolutional Neural Network (CNN) that encourages the first convolutional layer to discover more **meaningful filters**. SincNet is based on parametrized sinc functions, which implement band-pass filters.

In contrast to standard CNNs, that learn all elements of each filter, only low and high cutoff frequencies are directly learned from data with the proposed method. This offers a very compact and efficient way to derive a **customized filter bank** specifically tuned for the desired application. 

This project releases a collection of codes and utilities to perform speaker identification with SincNet.
An example of speaker identification with the TIMIT database is provided. 

<img src="https://github.com/mravanelli/SincNet/blob/master/SincNet.png" width="400" img align="right">

## Cite us
If you use this codes or part of it, please cite us!

*Mirco Ravanelli, Yoshua Bengio, “Speaker Recognition from raw waveform with SincNet” Arxiv*


## Prerequisites
- Linux
- Python 3.6/2.7
- pytorch 0.4.0
- pysoundfile (``` conda install -c conda-forge pysoundfile```)
- We also suggest to use the anaconda environment.


## How to run a TIMIT experiment
Even though the code can be easily adapted to any speech dataset, in the following part of the documentation we provide an example based on the popular TIMIT dataset.

**1. Run TIMIT data preparation.**

This step is necessary to store a version of TIMIT in which start and end silences are removed and the amplitute of each speech utterance is normalized. To do it, run the following code:

``
python TIMIT_preparation.py $TIMIT_FOLDER $OUTPUT_FOLDER data_lists/TIMIT_all.scp
``

where:
- *$TIMIT_FOLDER* is the folder where the original TIMIT corpus is stored
- *$OUTPUT_FOLDER* is the folder where the normalized TIMIT will be stored
- *data_lists/TIMIT_all.scp* is the list of the TIMIT files used for training/test the speaker id system.

**2. Run the speaker id experiment.**

- Modify the *[data]* section of *cfg/SincNet_TIMIT.cfg* file according to your paths. In particular, modify the *data_folder* with the *$OUTPUT_FOLDER* specified during the TIMIT preparation. The other parameters of the config file belong to the following sections:
 1. *[windowing]*, that defines how each sentence is splitted into smaller chunks.
 2. *[cnn]*,  that specifies the characteristics of the CNN architecture.
 3. *[dnn]*,  that specifies the characteristics of the fully-connected DNN architecture following the CNN layers.
 4. *[class]*, that specify the softmax classification part.
 5. *[optimization]*, that reports the main hyperparameters used to train the architecture.

- Once setup the cfg file, you can run the speaker id experiments using the following command:

``
python speaker_id.py --cfg=cfg/SincNet_TIMIT.cfg
``

The network might take a number of hours depending on the speed of your GPU card to converge. In our case, using an *nvidia TITAN X*, the full training took about 24 hours. If you use the code within a cluster is crucial to copy the normalized dataset into the local node, since the current version of the code requires frequent accesses to the stored wav files. Note that several possible optimizations to improve the code speed are not implemented in this version, since are out of the scope of this work.

**3. Results.**

The results are saved into the *output_folder* specified in the cfg file. In this folder, you can find a file (*res.res*) summarizing training and test error rates. The model *model_raw.pkl* is model saved during the last iteration. 
Using the cfg file specified above we obtain the following results:

## Where SincNet is implemented?
To take a look into the SincNet implementation you should open the file *dnn_models.py* and read the classes *SincNet*, *sinc_conv* and the function *sinc*.

## References

[1] Mirco Ravanelli, Yoshua Bengio, “Speaker Recognition from raw waveform with SincNet” Arxiv
