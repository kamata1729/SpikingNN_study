# SpikingNN_study
Spiing Neural Network関連の読んだ論文をまとめていきます

参考：
脳とDeep Neural Networkの関係性に関する論文のまとめページ
https://github.com/takyamamoto/BNN-ANN-papers

# SuperSpike (Neural computation 2017)
https://arxiv.org/abs/1705.11146

spikepropだとニューロンが発火しているところしかback propが行われない問題を解決

正解のスパイク波形との差分を二重指数関数フィルターで畳み込みを行なって積分したものをlossにすることで、完全にスパイクが一致する時のみlossが0になるので、常にback propできる

back propするためにいくつか近似をしていて、

・出力スパイクを膜電位の高速シグモイドにしている

・膜電位のシナプス強度微分を、前のニューロンのスパイク（の畳み込み）に近似

# Equilibrium Propagation
https://arxiv.org/pdf/1602.05179.pdf

https://github.com/TMats/survey/issues/71

Hopfield energy + コスト関数をネットワークのエネルギーと捉えて、free phaseではHopfield energyのみを最小化、weakly clamped phaseではコスト関数を徐々に増やしていって最小化する。多層にしてMNIST 98%

# SNN survey (2018)
https://arxiv.org/pdf/1804.08150.pdf

STDP, WTA, surpervised training(Spikepropから始まっていろんな手法）, deep の方法, CNNなどいろいろ書いてある

# Spiking YOLO (AAAI 2020)
https://arxiv.org/abs/1903.06530

Tiny YOLOをSpiking YOLOに変換する手法

leaky ReLUを表現するためのimbalanced thresholdと、channel wise normalizationを使用することで、FLOPSと消費電力を1/100にしながら、Tiny YOLOから精度は1%減にとどめた

# Enabling Deep Spiking Neural Networks with Hybrid Conversion and Spike Timing Dependent Backpropagation (ICLR2020)
https://iclr.cc/virtual_2020/poster_B1xSperKvH.html  

学習済みのANNの重みを、SNNの初期値に用いてSTDBで学習すると精度が上がるという話

STDBは、最終層の積算膜電位を用いて予測 / 中間層のスパイクの膜電位での微分はスパイクタイミングに応じた指数関数で近似 する方法

入力time stepを10分の1に減らしながら精度がそこまで下がらない（といっても4%くらい下がってる）

それならANNとSNNで同時にlossとって最適化できそうな気もする（そう言う研究も誰かがやってそうなきがする）

# Direct Training for Spiking Neural Networks: Faster, Larger, Better (AAAI 2019)
https://arxiv.org/pdf/1809.05793.pdf 

STBPの改善手法
改善点は、

(1) NeuNormという、layer wise BN (bias項なし）をLIF neuronで実装

(2) LIFモデルを式変形してiterativeな形式に変え、さらに出力のtime windowを小さくすることで高速化

# RMP-SNN: Residual Membrane Potential Neuron for Enabling Deeper High-Accuracy and Low-Latency Spiking Neural Network (CVPR 2020)
https://arxiv.org/pdf/2003.01811.pdf

ANN to SNNの変換手法。既存手法のLIF neuronは閾値Vthを越えて発火すると静止膜電位に戻るが(hard reset)、発火した際に単純に膜電位からVthを引き算するようにする(soft reset)と、情報の損失がなくなり精度が上がると言う主張

# SpikeGrad (ICLR2020)
https://arxiv.org/abs/1906.00851

BackPropagationもSNNの枠組みの中で時間的に逐次的にスパイク列を用いて行う手法

ANNと等価なback propができると主張しており、GPUを使って学習する際はANN側で学習することも可能
著者にコードを公開してもらうようメール済み

# Going Deeper in Spiking Neural Networks: VGG and Residual Architectures
https://arxiv.org/pdf/1802.02627.pdf

ANN-SNN conversionで、thresholdをスパイクの最大入力値にするspike normの提案

resnetの場合は、最初のlayerだけthresholdが高くなり、最後のlayerはidentitymapにほぼ等しくなるから、次の制約をかけている

・skip connectのreluは、どちらも同じthresholdにする

・最初の7x7 convは3x3に分解

深い層になるほどspike countが減ることも確認されている


