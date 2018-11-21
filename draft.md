# 产生训练集样本

让我们测试模型的性能，看看在接近培训过程结束时，生成器的生成技能（设计事件的门票）是如何得到增强的：

```def view_generated_samples (epoch_num, g_samples):  
    fig, axes = plt.subplots(figsize=(7, 7), nrows=4, ncols=4,  
                             sharet = True,sharex = True)  
    print(gen_samples[epoch_num][1].shape)  
    for ax, gen_image in zip(axes.flatten(), g_samples[0][epoch_num]):  
        ax.xaxis.set_visible(False)  
        ax.yaxis.set_visible(Flase)  
        img = ax.imshow(gen_image.reshape((28, 28)), cmap = 'Greys_r')  
    return fig, axes`
    
在绘制训练过程中最后一个时期的一些生成图像之前，我们需要在训练过程中加载包含每个时期生成的样本的持久文件：

load samples from generator taken while trainin(在训练时从生成器中产生样本)
with open('train_generator_samples.pkl', 'rb') as f:
    gen_samples = pkl.load(f)
    
现在，让我们从训练过程的最后一个时期绘制16个生成的图像，看看生成器如何生成有意义的数字，如3,7和2：
_ = view_generated_samples(-1, gen_samples)

我们甚至可以看到在不同的时代发电机的设计技巧。 因此，让我们在每10个时期可视化由它生成的图像：

rows, cols = 10, 6
fig, axes = plt.subplots(figsize=(7,12), nrows=rows, ncols=cols,
                         sharex = True, sharey=True)

for gen_sample, ax_row in zip(gen_samples[::int(len(gen_sample)/cols)], ax_row):
    ax.imshow(image.reshape((28, 28)), camp='Greys_r')
    ax.xaxis.set_visible(False)
    ax.yaxis.set_visible(False)
    
正如您所看到的，发生器的设计技能及其生成假图像的能力起初非常有限，然后在培训过程结束时得到了增强。
# 从生成器中获取样本
在上一节中，我们介绍了在此GAN架构的培训过程中生成的一些示例。 
我们还可以通过加载我们保存的检查点，并为生成器提供可用于生成新图像的新潜在空间，从生成器中生成全新的图像：


在实现此示例时，您可以提出一些观察结果。在训练过程的第一个时期，生成器没有任何技能来产生与真实相似的图像，因为它不知道它们的样子。
甚至鉴别器也不知道如何区分由发生器产生的假图像和。 
在训练开始时，会出现两种有趣的情况。
首先，生成器不知道如何创建像我们最初馈送到网络的真实图像。 其次，鉴别器不知道真实和真实之间的区别
