# 高级 AI 下 Machine Learning

Suprervised learning - 监督性学习，本质分类器

Unsupervised learning - 无监督学习，本质聚类

Semi-supervised learning - 小样本学习

Reinforcement learning - 强化学习

## Markov Decision Process

马尔可夫链

![image.png](markov.png))

动作，改变环境状态，奖励，循环

Policy，策略

输入环境状态，输出所有动作的概率。

要考虑短期收益和长期收益

## Build Advanced Game AI

构建游戏的

- Observation，
- Action
- Reward
- NN design
- Training Strategy

States

- Map
- Rewards : 细致且高频的 reward 效果好

#### 神经网络

- Multi-Layer Perceptron : MLP
- 卷积神经网络 Convolutional Neural Network CNN
- Transformer
- Long-Short Tern Memory (LSTM) 记忆功![image.png](lstm.png))
- NN Architecture Selection

先用监督学习法，训练出还可以的 AI

强化学习，AI 互博

自己打自己会陷入局部最优解。

选择和：自己打，过去的自己打，其他分支的自己打

主分支，leg 完全和别的版本自己竞争的分支，专门找主分支弱点的分支

监督学习，快速收敛，能达到业余水平

如果游戏每一步奖励是足够明确的，用强化学习

探索型的游戏使用监督学习

可以考虑混合使用
