[数据集 CIFAR-10]
      |
      v
[DataLoader] --> 拿出 x_0 (原图)
      |
      | (前向过程: Diffusion.noise_images)
      v
[计算物理引擎] --> 查表 alpha_hat --> 混合 x_0 和 随机噪声 epsilon
      |
      |--> 产出 x_t (脏图)
      |--> 产出 t (时间)
      |--> 产出 y (标签, 10%概率丢弃)
      |
      v
[神经网络 UNet]
      |--> 1. Embedding层: 把 t 和 y 变成向量并相加
      |--> 2. Encoder: 下采样 + 存快照 (Skip Connection) + 注入 t
      |--> 3. Bottleneck: 深层处理
      |--> 4. Decoder: 上采样 + 拼快照 + 注入 t
      |
      v
[输出 pred_noise]
      |
      | (计算 Loss)
      v
[MSE Loss] <--- 对比 ---> [真噪声 epsilon]
      |
      v
[Optimizer 更新权重] --> [EMA 更新影子权重]