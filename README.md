# LINE: Large-scale information network embedding

## Introduction

This is the LINE toolkit developed for embedding very large-scale information networks. It is suitable to a variety of networks including directed, undirected, binary or weighted edges. The LINE model is quite efficient, which is able to embed a network with millions of vertices and billions of edges on a single machine within a few hours.
```
Contact: Jian Tang, tangjianpku@gmail.com
Project page: https://sites.google.com/site/pkujiantang/line
This work was done when the author was working at Microsoft Research
```
## Usage

We provide both the Windows and LINUX versions. To compile the souce codes, some external packages are required, which are used to generate random numbers for the edge-sampling algorithm in the LINE model. 
- [The BOOST package](http://www.boost.org/) for the Windows version
- [the GSL package]( http://www.gnu.org/software/gsl/) for the LINUX version

## Network Input

The input of a network consists of the edges in the network. Each line of the input file represents a DIRECTED edge in the network, which is specified as the format "source_node target_node weight" (can be either separated by blank or tab). For each undirected edge, users must use TWO DIRECTED edges to represent it. Here is an input example of an undirected word co-occurrence network:
```
good the 3
the good 3
good bad 1
bad good 1
bad of 4
of bad 4
```

## Run 

A typical command to run the model can be:
```bash
./line -train network_file -output embedding_file -binary 1 -size 200 -order 2 -negative 5 -samples 100 -rho 0.025 -threads 20
```
- `-train`: the input file of a network;
- `-output`: the output file of the embedding;
- `-binary`: whether saving the output file in binary mode; the default is 0 (off);
- `-size`: the dimension of the embedding; the default is 100;
- `-order`: the order of the proximity used; 1 for first order, 2 for second order; the default is 2;
- `-negative`: the number of negative samples used in negative sampling; the deault is 5;
- `-samples`: the total number of training samples (in *millions*);
- `-rho`: the starting value of the learning rate; the default is 0.025;
- `-threads`: the total number of threads used; the default is 1.

## Files in the folder
- `line.cpp`: the souce code of the LINE;
- `reconstruct.cpp`: the code used for reconstructing the sparse networks into dense ones, which is described in Section 4.3;
- `normalize.cpp`: the code for normalizing the embeddings (l2 normalization);
- `concatenate.cpp`: the code for concatenating the embeddings with 1st-order and 2nd-order;

## Examples

We provide an example running script for the Youtube data set (available [here](http://socialnetworks.mpi-sws.mpg.de/data/youtube-links.txt.gz)). The script will automatically download the data, run LINE to learn network embeddings and finally it will evaluate the learned embeddings on the node classification task.

To run the script, users first need to compile the evaluation codes by running `make.sh` in the folder `evaluate`. Afterwards, we can run `train_youtube.sh` (or `train_youtube.bat` on Windows)to run the whole pipeline. 

More explicitly:
```bash
cd evaluate
bash make.sh
cd ..
bash train_youtube.sh
```
A separate script has been provided for systems where the `gsl` library is not  static and hence paths to its folders must be provided. The last line then becomes:
```bash
bash train_youtube_dyngsl.sh -i path/to/gsl/include -l path/to/gsl/lib
```


**Citation**

```
@inproceedings{tang2015line,
  title={LINE: Large-scale Information Network Embedding.},
  author={Tang, Jian and Qu, Meng and Wang, Mingzhe and Zhang, Ming and Yan, Jun and Mei, Qiaozhu},
  booktitle={WWW},
  year={2015},
  organization={ACM}
}
```
