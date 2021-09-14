# HyperGCN
This repository aims to reproduce **HyperGCN** proposed in the paper entitled "HyperGCN: A New Method of Training Graph Convolutional Networks on Hypergraphs (NeurIPS 2019)". 
 We refer to [the original repository](https://github.com/malllabiisc/HyperGCN) of **HyperGCN** to implement this repository. 

## Dependencies
 We use the following Python packages to implement this. Install them in your environment using `pip install -r requirements.txt`. 

 * tqdm == 4.62.1  
 * torch == 1.9.0  
 * scipy == 1.7.1  
 * numpy == 1.21.2  
 * ConfigArgParse == 1.5.2  

 ## Experimental setting

 ### Datasets
 We use the following datasets for the experiments. 
 * coauthorship : dblp, cora  
 * cocitation : citeseer, cora, pubmed

 ### Hyperparameters
 We use the following hyperparameters as described in **[Kipf and Welling](https://github.com/tkipf/gcn)**. 
 * hidden layer size : 32  
 * dropout rate : 0.5  
 * learning rate : 0.01  
 * weight decay : 0.0005  
 * training epochs : 200  
 * lamda for explicit Laplacian regularisation : 0.001    

## Training
To train the model, type the following command: 
 ```bash
 python main.py --mediators True --split 1 --data coauthorship -- dataset dblp
 ```

 It will train the model for the node classification task in the given hypergraph. 

 In this project, you can control the value of each hyperparameter in `config.py`. 
 The details of the hyperparameters are in the following table.

 Option | Description | Default
 ------- | ---------- | --------
 data | oauthorship/cocitation | coauthorship
 dataset | cora/dblp for coauthorship, cora/citeseer/pubmed for cocitation | dblp
 mediators | use of Laplacian | False
 fast | use of FastHyperGCN | False
 split | train-test split dataset | 1
 gpu | gpu number to use | 3
 cuda | True or False | True
 seed | and integer for random | 5
 depth | number of hidden layers | 2
 dropout | dropout probability for hidden layers | 0.5
 epochs | number of training epochs | 200
 rate | learning rate | 0.01
 decay | weight decay | 0.0005
 model | model for hypergraph | HyperGCN

## Difference between this repository and the original repository

I report `mean test error ± standard deviation` (lower is better) over train-test splits.  
`Accuracy = 100(%) - Error`

### Paper's result

 | Method | DBLP <br> co-authorship | Pubmed <br> co-citation | Cora <br> co-authorship | Cora <br> co-citation | Citeseer <br> co-citation
 ------- | ------ | ---- | ---- | ----- |------
 1-HyperGCN | 33.87 ± 2.4 | 30.08 ± 1.5 | 36.22 ± 2.2 | 34.45 ± 2.1 | 38.87 ± 1.9
 FastHyperGCN | 27.34 ± 2.1| 29.48 ± 1.6 | 32.54 ± 1.8 | 32.43 ± 1.8 | 37.42 ± 1.7
 HyperGCN | 24.09 ± 2.0 | 25.56 ± 1.6 | 30.08 ± 1.8 | 32.37 ± 1.7 | 37.35 ± 1.6

### My Experiment's result

| Method | DBLP <br> co-authorship | Pubmed <br> co-citation | Cora <br> co-authorship | Cora <br> co-citation | Citeseer <br> co-citation
 ------- | ------ | ---- | ---- | ----- |------
 1-HyperGCN | 32.64 ± 9.3 | 35.54 ± 15.4 | 48.98 ± 10.1 | 52.02 ± 12.2 | 44.65 ± 10.2
 FastHyperGCN | 30.20 ± 12.6| 44.71 ± 13.3 | 49.82 ± 9 | 48.66 ± 15.6 | 50.11 ± 9.8
 HyperGCN | 27.28 ± 8.1 | 37.36 ± 16.8 | 52.19 ± 14.4 | 43.33 ± 12.2 | 51.77 ± 9.1

In default hyperparameters in paper, these models do not train well.  
- Check **[issue](https://github.com/malllabiisc/HyperGCN/issues/1)** at here

So we need to find proper hyperparameter -> `Grid experiments` (about learning rate, epoch in each split without validation part)

## Experiment's parameters of coauthorship/dblp

> Split = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]  
> Learning rate = [0.001, 0.005, 0.01, 0.05, 0.1]  
> Epoch = [100, 200, 500, 1000, 2000]  

**The highest accuracy result**


  Task | Method | Learning rate | Epochs | Error ± std
 ----- | ------ | ------ | ------- | -------
 DBLP <br> (coauthorship) | 1-HyperGCN | 0.1 | 2000 | 15.45 ± 0.5
 DBLP <br> (coauthorship) | HyperGCN | 0.1 | 2000 | 15.49 ± 0.3
 DBLP <br> (coauthorship) | FastHyperGCN | 0.1 | 2000 | 15.49 ± 0.3
 Pubmed <br> (co_citation) | 1-HyperGCN | 0.1 | 1500 |21.0 ± 1.2
 Pubmed <br> (co_citation) | HyperGCN | 0.1 | 1500 |22.1 ± 1.4
 Pubmed <br> (co_citation) | FastHyperGCN | 0.1 | 1500 |20.62 ± 1.0
 Cora <br> (coauthorship) | 1-HyperGCN | 0.1 | 1000| 34.69 ± 2.4
 Cora <br> (coauthorship) | HyperGCN | 0.1 | 1000| 35.37 ± 1.8
 Cora <br> (coauthorship) | FastHyperGCN | 0.1 | 1000| 33.21 ± 2.8
 Cora <br> (co-citation) | 1-HyperGCN | 0.1 | 1500 | 34.69 ± 2.4
 Cora <br> (co-citation) | HyperGCN | 0.1 | 1500 | 35.47 ± 2.2
 Cora <br> (co-citation) | FastHyperGCN | 0.1 | 1500 | 35.77 ± 3.3
 Citeseer <br> (co-citation) | 1-HyeprGCN | 0.1 | 2000 | 39.59 ± 2.8
 Citeseer <br> (co-citation) | HyeprGCN | 0.1 | 2000 | 38.62 ± 1.5
 Citeseer <br> (co-citation) | FastHyeprGCN | 0.1 | 2000 | 40.83 ± 3.1

- `mean of accuracy` is 0.849550073302664 ~ 0.85  
- {Learning rate , Epoch} = [(0.05,500), (0.05,2000), (0.1,1000)] has similiar result as above
