# Semi-Supervised Classification with Graph Convolutional Networks (ICLR, 2017)

[논문링크](https://arxiv.org/abs/1609.02907)

<p align="center">
    <img width="600" alt='fig1' src="../img/kipf2016semi.png?raw=true"></br>
    <em><font size=2>Overall Architecture</font></em>
</p>

## 연구목적
- 본 논문은 neural network model을 사용하여 graph 구조를 직접적으로 인코딩하는 graph convolutional network(GCN)을 제안함
> - Graph의 각 노드의 feature를 담고 있는 feature matrix $X$와 graph에 대한 adjacency matrix $A$를 입력으로 이용하는 네트워크를 제안함
- 구체적으로, 본 논문은 (i) graph 상에서 직접적으로 동작하는 neural network를 위한 layer-wise propagation rule을 제안하고, (ii) graph-based neural network가 빠르고 scalable한 semi-supervised node classification을 위해 사용될 수 있다는 것을 증명함

## 접근법
- GCN의 graph convolution은 graph에 spectral convolution을 적용하는 것
> - Graph에서의 spectral convolution은 Fourier transform을 통해 eigen-decomposition을 수행하는 것과 같음
> - 즉, graph signal(node feature)을 freqeuncy(node 간의 차이) 별로 분해
- Graph convolution을 딥러닝의 관점에서 일반화하면 다음과 같음:
> - $H^{{l+1}} = \sigma(AH^{(l)}W^{(l)}+b^{(l)})$
> - 이때 $H^{{l+1}}$과 $H^{{l}}$은 각각 $(l+1)$번째, $(l)$번째 layer의 hidden state이며, $W^{(l)}$과 $b^{(l)}$는 $(l)$번째 layer의 weight/bias 이고, $A$는 adjacency matrix, $\sigma$는 activation function임
> - $(l)$이 $0$일때 $H^{(l)}$은 $X$가 됨
> - Adjacency matrix를 곱해주는 이유는 각 node와 연결되어 있는 node들의 feature만을 고려하기 위해서임
- 이를 보다 구체적으로 표현함
> - 결과적으로, graph convolution의 output $Z$는 $Z = \text{softmax}(\hat{A}\cdot\text{ReLU}(\hat{A}XW^{(0)})W^{(1)})$로 계산됨
> - 이때 $W^{(0)}$와 $W^{(1)}$은 layer의 weight matrix이며, $X$는 input feature, $\hat{A}$는 normalized adjacency matrix임
> - Adjacency matrix의 normalization은 각 node에 연결된 edge의 개수에 따라 $A$를 normalize하여 각 node 별 edge의 개수에 관계 없이 모든 node들을 잘 학습하도록 함
- GCN은 이러한 graph convolution layer를 여러 개 쌓아서 구성됨
> - 초기 layer에서는 각 node의 인접한 node들만을 이용하여 hidden states를 계산함
> - Layer가 쌓일수록 더 멀리 떨어진 node들까지 고려하여 hidden states를 계산할 수 있음

## 실험결과
- Node classification task에서 전체 lables의 5%만 사용하여 학습하더라도 좋은 성능을 기록함

## 의견
- / 