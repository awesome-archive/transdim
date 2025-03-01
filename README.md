***transdim***
--------------

>**Trans**portation **d**ata **im**putation (***transdim***).

Contents
--------

-   [Strategic aim](#strategic-aim)
-   [Tasks and challenges](#tasks-and-challenges)

  	[[Missing data imputation](#missing-data-imputation)] [[Rolling traffic prediction](#rolling-traffic-prediction)]
-   [What we do just now!](#what-we-do-just-now)
-   [What we care about!](#what-we-care-about)
-   [Overview](#overview)
-   [Selected References](#selected-references):

	[[Spatio-temporal forecasting](#spatio-temporal-forecasting)] [[Principal component analysis](#principal-component-analysis)] [[Guassian process](#gaussian-process)] [[Matrix factorization](#matrix-factorization)] [[Bayesian matrix and tensor factorization](#bayesian-matrix-and-tensor-factorization)] [[Low-rank tensor completion](#low-rank-tensor-completion)] [[Generative Adversarial Nets](#generative-adversarial-nets)] [[Variational Autoencoder](#variational-autoencoder)] [[Tensor regression](#tensor-regression)] [[Poisson matrix factorization](#poisson-matrix-factorization)] [[Graph signal processing](#graph-signal-processing)] [[Graph neural network](#graph-neural-network)] [[Missing data imputation](#missing-data-imputation)]
-   [Our Publications](#our-publications)
-   [License](#license)

Strategic aim
--------------

>Creating accurate and efficient solutions for the spatio-temporal traffic data imputation and prediction tasks.

Tasks and challenges
--------------

- ### **Missing data imputation**

  - **Random missing**: Each sensor lost their observations at completely random. (★★★)
  - **Non-random missing**: Each sensor lost their observations during several days. (★★★★)

- ### **Rolling traffic prediction**

  - Forecasting **without missing values**. (★★★)
  - Forecasting **with incomplete observations**. (★★★★★)

What we do just now!
--------------

- add a **framework** indicating overall studies;

![framework](https://github.com/xinychen/transdim/blob/master/images/framework.png)

> *Framework*: Tensor completion task and its framework including **data organization** and **tensor completion**, in which traffic measurements are partially observed.

- define the **problems** clearly;

    - Example: Traffic forecasting using matrix factorization models.

		![example](https://github.com/xinychen/transdim/blob/master/images/rolling_prediction_strategy.png)

> *Real experiment setting*: Observations with **0%, 20% and 40% fiber missing rates** during first **56 days are treated as stationary inputs**. Meanwhile, there are some rolling inputs for **forecasting traffic speed during last 5 days (from Monday to Friday)** in a rolling manner.

- describe the **core challenges** intuitively;
- list **main contributions** of these studies.

What we care about!
--------------

- Best algebraic structure for data imputation.
- The context of urban transportation (e.g., biases).
- Data noise avoidance.
- Competitive imputation and prediction performance.
- Capable of various missing data scenarios.

Overview
--------------

   >With the development and application of intelligent transportation systems, large quantities of urban traffic data are collected on a continuous basis from various sources, such as loop detectors, cameras, and floating vehicles. These data sets capture the underlying states and dynamics of transportation networks and the whole system and become beneficial to many traffic operation and management applications, including routing, signal control, travel time prediction, and so on. However, the missing data problem is inevitable when collecting traffic data from intelligent transportation systems.

### [Urban traffic speed data set of Guangzhou, China](https://doi.org/10.5281/zenodo.1205228)

  >**Publicly available at our Zenodo repository!**


![example](https://github.com/xinychen/transdim/blob/master/images/estimated_series1.png)
  *(a) Time series of actual and estimated speed within two weeks from August 1 to 14.*

![example](https://github.com/xinychen/transdim/blob/master/images/estimated_series2.png)
  *(b) Time series of actual and estimated speed within two weeks from September 12 to 25.*

> *The imputation performance of BGCP (CP rank r=15 and missing rate α=30%) under the fiber missing scenario with third-order tensor representation, where the estimated result of road segment #1 is selected as an example. In the both two panels, red rectangles represent fiber missing (i.e., speed observations are lost in a whole day).*

### Machine learning models

  - **Missing data imputation**

  > **Urban traffic speed data set** (i.e., [Guangzhou-data-set(Gdata)](https://github.com/xinychen/transdim/tree/master/Guangzhou-data-set)) registered traffic speed data from 214 road segments over two months (61 days from August 1 to September 30 in 2016) in Guangzhou, China. We organize the raw data into a time series matrix of (214, 8784). For tensor-based models, we use a third-order tensor (214, 61, 144) as input. Matrix based models are tested with the time series matrix (214, 8784).

  > We consider two common missing data scenarios (i.e., **random missing (RM)** and **non-random missing (NM))**. For **RM**, we simply remove certain amount of observed entries in the matrix randomly and use these entries as ground truth to evaluate RMSE. For **NM**, we apply correlated fiber missing experiment by randomly choosing certain amount (e.g., 40%) (location, day) combinations and removing the whole time series in each combination.

|  Missingness | [BGCP](https://nbviewer.jupyter.org/github/xinychen/transdim/blob/master/Imputation-BGCP-Gdata.ipynb) | [BPMF](https://nbviewer.jupyter.org/github/xinychen/transdim/blob/master/Imputation-BPMF-Gdata.ipynb) | [PMF](https://nbviewer.jupyter.org/github/xinychen/transdim/blob/master/Imputation-PMF-Gdata.ipynb) | [GAIN](https://nbviewer.jupyter.org/github/xinychen/transdim/blob/master/Imputation-GAIN-Gdata.ipynb) |
|---     |---    |---    |---    |---    |
| 20%, RM | **3.5762** | 4.0403 | 4.0909 | 4.6718 |
| 40%, RM | **3.5969** | 4.1578	| 4.2280 | 5.1776 |
| 20%, NM | 4.4136 | 4.3828 | **4.3575** | 6.5500 |
| 40%, NM | 4.6791 | 4.5586	| **4.4866** | 6.9947	|


- ***BGCP***: Bayesian Gaussian CP decomposition (by [Chen et al., 2019](https://doi.org/10.1016/j.trc.2018.11.003)). [[Imputation example - Jupyter Notebook](http://nbviewer.jupyter.org/github/xinychen/transdim/blob/master/BGCP_example.ipynb)] [[Matlab code is also available!](https://github.com/lijunsun/bgcp_imputation)]

- ***BPMF***: Bayesian probabilistic matrix factorization (by [Salakhutdinov et al., 2008](https://www.cs.toronto.edu/~amnih/papers/bpmf.pdf)).

- ***PMF***: Probabilistic matrix factorization (by [Salakhutdinov et al., 2007](http://59.80.44.99/papers.nips.cc/paper/3208-probabilistic-matrix-factorization.pdf)).
  - The [code1](https://github.com/adamzjw) and [code2](https://github.com/fuhailin/Probabilistic-Matrix-Factorization) have been adapted for our experiments.

- ***GAIN***: Generative Adversarial Imputation Nets (by [Yoon et al., 2018](http://proceedings.mlr.press/v80/yoon18a/yoon18a.pdf)).
  - The [code](https://github.com/jsyoon0823/GAIN) has been adapted for our implementation.

- ***TRMF***: Temporal regularized matrix factorization. [[Matlab code is also available!](https://github.com/rofuyu/exp-trmf-nips16)]

- ***HaLRTC***: High accuracy low rank tensor completion.



Selected references
--------------

- ### **Spatio-temporal forecasting**

  - San Gultekin, John Paisley, 2019. [*Online Forecasting Matrix Factorization*](https://ieeexplore.ieee.org/document/8590686/). IEEE Transactions on Signal Processing, 67(5): 1223-1236. [[Python code](https://github.com/chloemnge/online_learning)]

  - Zheyi Pan, Yuxuan Liang, Junbo Zhang, Xiuwen Yi, Yong Yu, Yu Zheng, 2018. [*HyperST-Net: hypernetworks for spatio-temporal forecasting*](https://arxiv.org/pdf/1809.10889.pdf). arXiv.

  - Truc Viet Le, Richard Oentaryo, Siyuan Liu, Hoong Chuin Lau, 2017. [*Local Gaussian processes for efficient fine-grained traffic speed prediction*](https://arxiv.org/pdf/1708.08079.pdf). arXiv.

  - Yaguang Li, Cyrus Shahabi, 2018. [*A brief overview of machine learning methods for short-term traffic forecasting and future directions*](https://doi.org/10.1145/3231541.3231544). ACM SIGSPATIAL, 10(1): 3-9.

  - Bing Yu, Haoteng Yin, Zhanxing Zhu, 2017. [*Spatio-temporal graph convolutional networks: a deep learning framework for traffic forecasting*](https://arxiv.org/pdf/1709.04875.pdf). arXiv. ([appear in IJCAI 2018](https://www.ijcai.org/proceedings/2018/0505.pdf))

  - Feras A. Saad, Vikash K. Mansinghka, 2018. [*Temporally-reweighted Chinese Restaurant Process mixtures for clustering, imputing, and forecasting multivariate time series*](http://proceedings.mlr.press/v84/saad18a/saad18a.pdf). Proceedings of the 21st International Conference on Artificial Intelligence and Statistics (*AISTATS 2018*), Lanzarote, Spain. PMLR: Volume 84.

  - Zhengping Che, Sanjay Purushotham, Kyunghyun Cho, David Sontag, Yan Liu, 2018. [*Recurrent neural networks for multivariate time series with missing values*](https://doi.org/10.1038/s41598-018-24271-9). Scientific Reports, 8(6085).

  - Zhengping Che, Sanjay Purushotham, Guangyu Li, Bo Jiang, Yan Liu, 2018. [*Hierarchical deep generative models for multi-rate multivariate time series*](http://proceedings.mlr.press/v80/che18a/che18a.pdf). Proceedings of the 35th International Conference on Machine Learning (*ICML 2018*), PMLR 80:784-793, 2018.

  - Chuxu Zhang, Dongjin Song, Yuncong Chen, Xinyang Feng, Cristian Lumezanu, Wei Cheng, Jingchao Ni, Bo Zong, Haifeng Chen, Nitesh V. Chawla, 2018. [*A deep neural network for unsupervised anomaly detection and diagnosis in multivariate time series data*](https://arxiv.org/abs/1811.08055). arXiv.
  - Wang, X., Chen, C., Min, Y., He, J., Yang, B., Zhang, Y., 2018. [*Efficient metropolitan traffic prediction based on graph recurrent neural network*](https://arxiv.org/pdf/1811.00740.pdf). arXiv.

  - Peiguang Jing, Yuting Su, Xiao Jin, Chengqian Zhang, 2018. [*High-order temporal correlation model learning for time-series prediction*](https://doi.org/10.1109/TCYB.2018.2832085). IEEE Transactions on Cybernetics, early access.

  - Oren Anava, Elad Hazan, Assaf Zeevi, 2015. [*Online time series prediction with missing data*](http://proceedings.mlr.press/v37/anava15.pdf). Proceedings of the 32nd International Conference on Machine Learning (*ICML 2015*), 37: 2191-2199.

  - Shanshan Feng, Gao Cong, Bo An, Yeow Meng Chee, 2017. [*POI2Vec: Geographical latent representation for predicting future visitors*](https://aaai.org/ocs/index.php/AAAI/AAAI17/paper/view/14902/13749). Proceedings of the Thirty-First AAAI Conference on Artificial Intelligence (*AAAI 2017*).

  - Yasuko Matsubara, Yasushi Sakurai, Christos Faloutsos, Tomoharu Iwata, Masatoshi Yoshikawa, 2012. [*Fast mining and forecasting of complex time-stamped events*](http://www.cs.kumamoto-u.ac.jp/~yasuko/PUBLICATIONS/kdd12-trimine.pdf). Proceedings of the 18th ACM SIGKDD international conference on Knowledge discovery and data mining (*KDD 2012*).

  - Yasuko Matsubara, Yasushi Sakurai, Willem G. van Panhuis, Christos Faloutsos, 2014. [*FUNNEL: automatic mining of spatially coevolving epidemics*](http://www.cs.cmu.edu/~christos/PUBLICATIONS/14-kdd-funnel.pdf). Proceedings of the 20th ACM SIGKDD international conference on Knowledge discovery and data mining (*KDD 2014*).

  - Koh Takeuchi, Hisashi Kashima, Naonori Ueda, 2017. [*Autoregressive tensor factorization for spatio-temporal predictions*](https://doi.org/10.1109/ICDM.2017.146). 2017 IEEE International Conference on Data Mining (*ICDM 2017*).

  - Shun-Yao Shih, Fan-Keng Sun, Hung-yi Lee, 2018. [*Temporal pattern attention for multivariate time series forecasting*](https://arxiv.org/pdf/1809.04206v2.pdf). arXiv.

- ### **Principal component analysis**

  - Shigeyuki Oba, Masa-aki Sato, Ichiro Takemasa, Morito Monden, Ken-ichi Matsubara, Shin Ishii, 2003. [*A Bayesian missing value estimation method for gene expression profile data*](https://doi.org/10.1093/bioinformatics/btg287). Bioinformatics, 19: 2088-2096. [[Matlab code](http://ishiilab.jp/member/oba/tools/BPCAFill.html)]

  - Li Qu, Li Li, Yi Zhang, Jianming Hu, 2009. [*PPCA-based missing data imputation for traffic flow volume: a systematical approach*](https://doi.org/10.1109/TITS.2009.2026312). IEEE Transactions on Intelligent Transportation Systems, 10(3): 512-522.

  - Li Li, Yuebiao Li, Zhiheng Li, 2013. [*Efficient missing data imputing for traffic flow by considering temporal and spatial dependence*](https://doi.org/10.1016/j.trc.2013.05.008). Transportation Research Part C: Emerging Technologies, 34: 108-120.

- ### **Guassian process**

  - Michalis K. Titsias, Magnus Rattray, Neil D. Lawrence, 2009. [*Markov chain Monte Carlo algorithms for Gaussian processes*](http://www2.aueb.gr/users/mtitsias/papers/ILDMChapter09.pdf), Chapter.

  - Filipe Rodrigues, Kristian Henrickson, Francisco C. Pereira, 2018. [*Multi-output Gaussian processes for crowdsourced traffic data imputation*](https://doi.org/10.1109/TITS.2018.2817879). IEEE Transactions on Intelligent Transportation Systems, early access. [[Matlab code](http://fprodrigues.com/publications/multi-output-gaussian-processes-for-crowdsourced-traffic-data-imputation/)]

  - Nicolo Fusi, Rishit Sheth, Huseyn Melih Elibol, 2017. [*Probabilistic matrix factorization for automated machine learning*](https://arxiv.org/pdf/1705.05355.pdf). arXiv. [[Python code](https://github.com/elibol/amle)]

  - Tinghui Zhou, Hanhuai Shan, Arindam Banerjee, Guillermo Sapiro, 2012. [*Kernelized probabilistic matrix factorization: exploiting graphs and side information*](http://www.cs.cmu.edu/~tinghuiz/papers/sdm12_kpmf.pdf). [[slide](http://people.ee.duke.edu/~lcarin/Jorge6.4.2012.pdf)]

  - John Bradshaw, Alexander G. de G. Matthews, Zoubin Ghahramani, 2017. [*Adversarial examples, uncertainty, and transfer testing robustness in Gaussian process hybrid deep networks*](https://arxiv.org/pdf/1707.02476.pdf). arXiv.

- ### **Matrix factorization**

  - Nikhil Rao, Hsiangfu Yu, Pradeep Ravikumar, Inderjit S Dhillon, 2015. [*Collaborative filtering with graph information: Consistency and scalable methods*](http://www.cs.utexas.edu/~rofuyu/papers/grmf-nips.pdf). Neural Information Processing Systems (*NIPS 2015*). [[Matlab code](http://bigdata.ices.utexas.edu/publication/collaborative-filtering-with-graph-information-consistency-and-scalable-methods/)]

  - Hsiang-Fu Yu, Nikhil Rao, Inderjit S. Dhillon, 2016. [*Temporal regularized matrix factorization for high-dimensional time series prediction*](http://www.cs.utexas.edu/~rofuyu/papers/tr-mf-nips.pdf). 30th Conference on Neural Information Processing Systems (*NIPS 2016*), Barcelona, Spain. [[Matlab code](https://github.com/rofuyu/exp-trmf-nips16)]

  - Yongshun Gong, Zhibin Li, Jian Zhang, Wei Liu, Yu Zheng, Christina Kirsch, 2018. [*Network-wide crowd flow prediction of Sydney trains via customized online non-negative matrix factorization*](http://urban-computing.com/pdf/CIKM18-1121-Camera%20Ready.pdf). In The 27th ACM International Conference on Information and Knowledge Management (*CIKM 2018*), Torino, Italy.

- ### **Bayesian matrix and tensor factorization**

  - Ruslan Salakhutdinov, Andriy Mnih, 2008. [*Bayesian probabilistic matrix factorization using Markov chain Monte Carlo*](https://www.cs.toronto.edu/~amnih/papers/bpmf.pdf). Proceedings of the 25th International Conference on Machine Learning (*ICML 2008*), Helsinki, Finland. [[Matlab code (official)](https://www.cs.toronto.edu/~rsalakhu/BPMF.html)] [[Python code](https://github.com/LoryPack/BPMF)] [[Julia and C++ code](https://github.com/ExaScience/bpmf)] [[Julia code](https://github.com/RottenFruits/BPMF.jl)]

  - Ilya Sutskever, Ruslan Salakhutdinov, Joshua B. Tenenbaum, 2009. [*Modelling relational data using Bayesian clustered tensor factorization*](https://ece.duke.edu/~lcarin/pmfcrp.pdf). NIPS 2009.

  - kan Saha, Vikas Sindhwani, 2012. [*Learning evolving and emerging topics in social media: A dynamic NMF approach with temporal regularization*](http://people.cs.uchicago.edu/~ankans/Papers/wsdm227-saha.pdf). WSDM 2012. (★★★★)

  - Nicolo Fusi, Rishit Sheth, Melih Huseyn Elibol, 2017. [*Probabilistic matrix factorization for automated machine learning*](https://arxiv.org/pdf/1705.05355.pdf). arXiv.

  - Liang Xiong, Xi Chen, Tzu-Kuo Huang, Jeff Schneider, Jaime G. Carbonell, 2010. [*Temporal collaborative filtering with Bayesian probabilistic tensor factorization*](https://www.cs.cmu.edu/~jgc/publication/PublicationPDF/Temporal_Collaborative_Filtering_With_Bayesian_Probabilidtic_Tensor_Factorization.pdf). Proceedings of the 2010 SIAM International Conference on Data Mining. SIAM, pp. 211-222.

  - Qibin Zhao, Liqing Zhang, Andrzej Cichocki, 2015. [*Bayesian CP factorization of incomplete tensors with automatic rank determination*](https://doi.org/10.1109/TPAMI.2015.2392756). IEEE Transactions on Pattern Analysis and Machine Intelligence, 37(9): 1751-1763.

  - Qibin Zhao, Liqing Zhang, Andrzej Cichocki, 2015. [*Bayesian sparse Tucker models for dimension reduction and tensor completion*](https://arxiv.org/pdf/1505.02343.pdf). arXiv.

  - Piyush Rai, Yingjian Wang, Shengbo Guo, Gary Chen, David B. Dunsun,	Lawrence Carin, 2014. [*Scalable Bayesian low-rank decomposition of incomplete multiway tensors*](http://people.ee.duke.edu/~lcarin/mpgcp.pdf). Proceedings of the 31st International Conference on Machine Learning (*ICML 2014*), Beijing, China.

- ### **Low-rank tensor completion**

  - Ji Liu, Przemyslaw Musialski, Peter Wonka, Jieping Ye, 2013. [*Tensor completion for estimating missing values in visual data*](https://doi.org/10.1109/TPAMI.2012.39). IEEE Transactions on Pattern Analysis and Machine Intelligence, 35(1): 208-220.

  - Bin Ran, Huachun Tan, Yuankai Wu, Peter J. Jin, 2016. [*Tensor based missing traffic data completion with spatial–temporal correlation*](https://doi.org/10.1016/j.physa.2015.09.105). Physica A: Statistical Mechanics and its Applications, 446: 54-63.

- ### **Generative Adversarial Nets**

  - Brandon Amos, 2016. [*Image completion with deep learning in TensorFlow*](http://bamos.github.io/2016/08/09/deep-completion/). blog post. [[github](https://github.com/bamos/dcgan-completion.tensorflow)]

  - Jinsun Yoon, James Jordon, Mihaela van der Schaar, 2018. [*GAIN: missing data imputation using Generative Adversarial Nets*](http://proceedings.mlr.press/v80/yoon18a/yoon18a.pdf). Proceedings of the 35th International Conference on Machine Learning (*ICML 2018*), Stockholm, Sweden. [[supplementary materials](http://medianetlab.ee.ucla.edu/papers/ICML_GAIN_Supp.pdf)] [[Python code](https://github.com/jsyoon0823/GAIN)]

  - Ian Goodfellow, 2016. [*NIPS 2016 tutorial: Generative Adversarial Networks*](https://arxiv.org/abs/1701.00160).

  - Thomas Schlegl, Philipp Seeböck, Sebastian M. Waldstein, Ursula Schmidt-Erfurth, Georg Langs, 2017. [*Unsupervised anomaly detection with generative adversarial networks to guide marker discovery*](https://arxiv.org/abs/1703.05921). arXiv.

  - Yonghong Luo, Xiangrui Cai, Ying Zhang, Jun Xu, Xiaojie Yuan, 2018. [*Multivariate time series imputation with generative adversarial networks*](https://papers.nips.cc/paper/7432-multivariate-time-series-imputation-with-generative-adversarial-networks). 32nd Conference on Neural Information Processing Systems (*NeurIPS 2018*), Montréal, Canada. [[Python code](https://github.com/Luoyonghong/Multivariate-Time-Series-Imputation-with-Generative-Adversarial-Networks)]

- ### **Variational Autoencoder**

  - Zhiwei Deng, Rajitha Navarathna, Peter Carr, Stephan Mandt, Yisong Yue, 2017. [*Factorized variational autoencoders for modeling audience reactions to movies*](http://openaccess.thecvf.com/content_cvpr_2017/papers/Deng_Factorized_Variational_Autoencoders_CVPR_2017_paper.pdf). 2017 IEEE Conference on Computer Vision and Pattern Recognition (*CVPR 2017*), Honolulu, HI, USA.

  - Vassilis Kalofolias, Xavier Bresson, Michael Bronstein, Pierre Vandergheynst, 2014. [*Matrix completion on graphs*](https://arxiv.org/abs/1408.1717). arXiv. (appear in NIPS 2014)

  - Rianne van den Berg, Thomas N. Kipf, Max Welling, 2018. [*Graph convolutional matrix completion*](https://www.kdd.org/kdd2018/files/deep-learning-day/DLDay18_paper_32.pdf). Proceedings of the 24th ACM SIGKDD International Conference on Knowledge Discovery and Data Mining (*KDD 2018*), London, UK.

  - [*Graph autoencoder - GitHub*](https://github.com/tkipf/gae).

  - Haowen Xu, Wenxiao Chen, Nengwen Zhao, Zeyan Li, Jiahao Bu, Zhihan Li, Ying Liu, Youjian Zhao, Dan Pei, Yang Feng, Jie Chen, Zhaogang Wang, Honglin Qiao, 2018. [*Unsupervised anomaly detection via variational auto-encoder for seasonal KPIs in web applications*](https://arxiv.org/pdf/1802.03903.pdf). *WWW 2018*.

  - John T. McCoy, Steve Kroon, Lidia Auret, 2018. [*Variational Autoencoders for missing data imputation with application to a simulated milling circuit*](https://doi.org/10.1016/j.ifacol.2018.09.406). IFAC-PapersOnLine, 51(21): 141-146. [[Python code](https://github.com/ProcessMonitoringStellenboschUniversity/IFAC-VAE-Imputation)] [[VAE demo](https://github.com/oduerr/dl_tutorial/blob/master/tensorflow/vae/vae_demo-2D.ipynb)]

  - Pierre-Alexandre Mattei, Jes Frellsen, 2018. [missingIWAE: Deep generative modelling and imputation of incomplete data](http://bayesiandeeplearning.org/2018/papers/100.pdf). Third workshop on Bayesian Deep Learning (*NeurIPS 2018*), Montréal, Canada. [related slide](https://ai.ku.dk/ai-seminar-series/ai-seminar_jes-frellsen.pdf)

- ### **Tensor regression**

  - Guillaume Rabusseau, Hachem Kadri, 2016. [*Low-rank regression with tensor responses*](https://papers.nips.cc/paper/6302-low-rank-regression-with-tensor-responses.pdf). 30th Conference on Neural Information Processing Systems (*NIPS 2016*), Barcelona, Spain.

  - Rose Yu, Yan Liu, 2016. [*Learning from multiway data: simple and efficient tensor regression*](http://proceedings.mlr.press/v48/yu16.pdf). Proceedings of the 33rd International Conference on Machine Learning (*ICML 2016*), New York, NY, USA.

  - Masaaki Imaizumi, Kohei Hayashi, 2016. [*Doubly decomposing nonparametric tensor regression*](http://proceedings.mlr.press/v48/imaizumi16.pdf). Proceedings of the 33 rd International Conference on Machine Learning (*ICML 2016*), New York, NY, USA.

  - Rose Yu, Guangyu Li, Yan Liu, 2018. [*Tensor regression meets Gaussian processes*](http://proceedings.mlr.press/v84/yu18a/yu18a.pdf). Proceedings of the 21st International Conference on Artificial Intelligence and Statistics (*AISTATS 2018*), Lanzarote, Spain. [[Matlab code](https://github.com/yuqirose/MultilinearGP)]

  - Lifang He, Kun Chen, Wanwan Xu, Jiayu Zhou, Fei Wang, 2018. [*Boosted sparse and low-rank tensor regression*](https://papers.nips.cc/paper/7379-boosted-sparse-and-low-rank-tensor-regression.pdf). 32nd Conference on Neural Information Processing Systems (*NeurIPS 2018*), Montréal, Canada.

- ### **Poisson matrix factorization**

  - Liangjie Hong, 2015. [*Poisson matrix factorization*](http://www.hongliangjie.com/2015/08/17/poisson-matrix-factorization/). blog post.

  - Ali Taylan Cemgil, 2009. [*Bayesian inference for nonnegative matrix factorisation models*](http://downloads.hindawi.com/journals/cin/2009/785152.pdf). Computational intelligence and neuroscience.

  - Prem Gopalan, Jake M. Hofman, David M. Blei, 2015. [*Scalable recommendation with hierarchical poisson factorization*](http://www.cs.columbia.edu/~blei/papers/GopalanHofmanBlei2015.pdf). In UAI, 326-335. [[C++ code](https://github.com/premgopalan/hgaprec)]

  - Laurent Charlin, Rajesh Ranganath, James Mclnerney, 2015. [*Dynamic Poisson factorization*](http://www.cs.toronto.edu/~lcharlin/papers/2015_CharlinRanganathMcInerneyBlei.pdf). Proceedings of the 9th ACM Conference on Recommender Systems (*RecSys 2015*), Vienna, Italy. [[C++ code](https://github.com/blei-lab/DynamicPoissonFactorization)]

  - Seyed Abbas Hosseini, Keivan Alizadeh, Ali Khodadadi, Ali Arabzadeh, Mehrdad Farajtabar, Hongyuan Zha, Hamid R. Rabiee, 2017. [*Recurrent Poisson factorization for temporal recommendation*](https://dl.acm.org/citation.cfm?doid=3097983.3098197). Proceedings of the 23rd ACM SIGKDD International Conference on Knowledge Discovery and Data Mining (*KDD 2017*), Halifax, Nova Scotia Canada. [[Matlab code](https://github.com/AHosseini/RPF)]

- ### **Graph signal processing**

  - Arman Hasanzadeh, Xi Liu, Nick Duffield, Krishna R. Narayanan, Byron Chigoy, 2017. [*A graph signal processing approach for real-time traffic prediction in transportation networks*](https://arxiv.org/pdf/1711.06954.pdf). arXiv.

  - Antonio Ortega, Pascal Frossard, Jelena Kovačević, José M. F. Moura, Pierre Vandergheynst, 2018. [*Graph signal processing: overview, challenges, and applications*](https://doi.org/10.1109/JPROC.2018.2820126). Proceedings of the IEEE, 106(5): 808-828. [[slide](https://www.seas.upenn.edu/~gsp16/ortega.pdf)]

- ### **Graph neural network**

  - [*How to do Deep Learning on Graphs with Graph Convolutional Networks (Part 1: A High-Level Introduction to Graph Convolutional Networks)*](https://towardsdatascience.com/how-to-do-deep-learning-on-graphs-with-graph-convolutional-networks-7d2250723780). blog post.

  - [*Structured deep models: Deep learning on graphs and beyond*](https://tkipf.github.io/misc/SlidesCambridge.pdf). slide.

  - [*gcn: Implementation of Graph Convolutional Networks in TensorFlow*](https://github.com/tkipf/gcn). GitHub project.

  - [*gated-graph-neural-network-samples: Sample Code for Gated Graph Neural Networks*](https://github.com/Microsoft/gated-graph-neural-network-samples). GitHub project.

  - Xu Geng, Yaguang Li, Leye Wang, Lingyu Zhang, Qiang Yang, Jieping Ye, Yan Liu, 2019. [*Spatiotemporal multi-graph convolution network for ride-hailing demand forecasting*](http://www-scf.usc.edu/~yaguang/papers/aaai19_multi_graph_convolution.pdf). *AAAI 2019*.

  - Menglin Wang, Baisheng Lai, Zhongming Jin, Yufeng Lin, Xiaojia Gong, Jiangqiang Huang, Xiansheng Hua, 2018. [*Dynamic spatio-temporal graph-based CNNs for traffic prediction*](https://arxiv.org/pdf/1812.02019.pdf). arXiv.

- ### **Missing data imputation**

  -  Daniel J. Stekhoven, Peter Bühlmann, 2012. [*MissForest—non-parametric missing value imputation for mixed-type data*](https://doi.org/10.1093/bioinformatics/btr597). Bioinformatics, 28(1): 112–118. [[missingpy - PyPI](https://pypi.org/project/missingpy/)] or [[missingpy - GitHub](https://github.com/epsilon-machine/missingpy)]

  - [fancyimpute](https://github.com/iskandr/fancyimpute): A variety of matrix completion and imputation algorithms implemented in Python. [[homepage](https://pypi.org/project/fancyimpute/)]

  - Dimitris Bertsimas, Colin Pawlowski, Ying Daisy Zhuo, 2018. [*From predictive methods to missing data imputation: An optimization approach*](http://jmlr.org/papers/v18/17-073.html). Journal of Machine Learning Research, 18(196): 1-39.

  - Wei Cao, Dong Wang, Jian Li, Hao Zhou, Yitan Li, Lei Li, 2018. [*BRITS: Bidirectional Recurrent Imputation for Time Series*](https://papers.nips.cc/paper/7911-brits-bidirectional-recurrent-imputation-for-time-series.pdf). 32nd Conference on Neural Information Processing Systems (NeurIPS 2018), Montréal, Canada. [Python code](https://github.com/caow13/BRITS)

Our publications
--------------

- **Xinyu Chen**, Zhaocheng He, Yixian Chen, Yuhuan Lu, Jiawei Wang (2019). **Missing traffic data imputation and pattern discovery with a Bayesian augmented tensor factorization model**. Transportation Research Part C: Emerging Technologies, 104: 66-77. [[preprint](https://xinychen.github.io/paper/BATF.pdf)] [[slide](https://doi.org/10.5281/zenodo.2632552)] [[data](http://doi.org/10.5281/zenodo.1205229)] [[Matlab code](https://github.com/sysuits/BATF)]

- **Xinyu Chen**, Zhaocheng He, Lijun Sun (2019). **A Bayesian tensor decomposition approach for spatiotemporal traffic data imputation**. Transportation Research Part C: Emerging Technologies, 98: 73-84. [[preprint](https://www.researchgate.net/publication/329177786_A_Bayesian_tensor_decomposition_approach_for_spatiotemporal_traffic_data_imputation)] [[doi](https://doi.org/10.1016/j.trc.2018.11.003)] [[data](http://doi.org/10.5281/zenodo.1205229)] [[Matlab code](https://github.com/lijunsun/bgcp_imputation)] [[Imputation example in Jupyter notebook (Matlab)](https://nbviewer.jupyter.org/github/xinychen/transdim/blob/master/BGCP_example.ipynb)] [[Jupyter notebook (Python)](https://nbviewer.jupyter.org/github/xinychen/transdim/blob/master/Imputation-BGCP-Gdata.ipynb)]

- **Xinyu Chen**, Zhaocheng He, Jiawei Wang (2018). **Spatial-temporal traffic speed patterns discovery and incomplete data recovery via SVD-combined tensor decomposition**. Transportation Research Part C: Emerging Technologies, 86: 59-77. [[doi](http://doi.org/10.1016/j.trc.2017.10.023)] [[data](http://doi.org/10.5281/zenodo.1205229)]

  >Please consider citing our papers if they help your research.

Our blog posts (in Chinese)
--------------

  - [贝叶斯泊松分解变分推断笔记](https://yxnchen.github.io/research/%E8%B4%9D%E5%8F%B6%E6%96%AF%E6%B3%8A%E6%9D%BE%E5%88%86%E8%A7%A3%E5%8F%98%E5%88%86%E6%8E%A8%E6%96%AD%E7%AC%94%E8%AE%B0/), by Yixian Chen (陈一贤).

  - [变分贝叶斯推断笔记](https://yxnchen.github.io/research/%E5%8F%98%E5%88%86%E8%B4%9D%E5%8F%B6%E6%96%AF%E6%8E%A8%E6%96%AD%E7%AC%94%E8%AE%B0/), by Yixian Chen (陈一贤).

  - [贝叶斯高斯张量分解](https://zhuanlan.zhihu.com/p/47049414), by Xinyu Chen (陈新宇).

  - [贝叶斯矩阵分解](https://zhuanlan.zhihu.com/p/26067454), by Xinyu Chen (陈新宇).

  - [时序矩阵分解 | 时序数据修补与预测](https://zhuanlan.zhihu.com/p/56105537), by Jamie Yang (杨津铭).

  - [如何用贝叶斯概率矩阵分解修复缺失数据？(Jupyter notebook - Python)](https://zhuanlan.zhihu.com/p/63351454), by Xinyu Chen (陈新宇).

  - [如何用贝叶斯高斯张量分解修复缺失数据？(Jupyter notebook - Python)](https://zhuanlan.zhihu.com/p/59196946), by Xinyu Chen (陈新宇).

License
--------------

This work is released under the MIT license.
