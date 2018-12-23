---


---

<h1 id="keras-retinanet">Keras-RetinaNet</h1>
<p>在自标数据集 <code>alidq</code> 上训练 detection model <code>RetinaNet</code></p>
<h2 id="模型部署与环境配置">模型部署与环境配置</h2>
<p>参考<a href="https://github.com/fizyr/keras-retinanet#installation">README</a></p>
<h2 id="数据预处理">数据预处理</h2>
<blockquote>
<p>数据统计信息：</p>
<ul>
<li>类别：gun1, gun2</li>
<li>有效数据量：23216</li>
<li>测试集大小：1000</li>
<li>训练验证集大小：22216</li>
</ul>
</blockquote>
<p>由于此次 detection 任务比较简单，为了实验 fine tuning 对模型的影响，我们将训练数据分为 3 个部分，这次实验在第 1 部分数据上完成。</p>
<blockquote>
<p>Part 1 训练数据统计量：</p>
<ul>
<li>gun1 数量：2826</li>
<li>gun2 数量：3170</li>
</ul>
</blockquote>
<ul>
<li>预处理需要将标注数据文件格式转换为固定格式的 <code>csv</code> 文件，schema 为：<br>
<code>path/to/imagefile,xmin,ymin,xmax,ymax,classname</code></li>
<li>我们标注的 Raw data 中包含的信息量是足够的，但需要一些针对模型的数据格式调整；</li>
<li>除了 Bounding Box 的坐标和类别名，我们还需要定义类别名到类别ID 映射（class name to class ID mapping），ID 从 0 开始。在这次的例子里很简单，在数据集目录新建一个 csv 文件，其内容为：<br>
<code>gun1,0</code><br>
<code>gun2,1</code></li>
<li>需要提出的一点是：如果没有特殊要求，我们交付的数据中，Bounding Box 的坐标最好按照普遍通用的顺序处理好，即<code>xmin,ymin,xmax,ymax</code>；</li>
<li>预处理完成后，可以使用 <code>keras-retinanet</code> 中的调试工具 <code>debug.py</code> 检查 csv 是否有效且观察标注框在图中的效果：<br>
<code>python keras-retinanet/bin/debug.py csv /path/to/annotations /path/to/class_label_mapping</code></li>
</ul>
<h2 id="训练">训练</h2>
<p>下载在 coco 数据集上训练好的模型 <a href="https://github.com/fizyr/keras-retinanet/releases/download/0.5.0/resnet50_coco_best_v2.1.0.h5">resnet50_coco_best_v2.1.0.h5</a> 到<code>snapshot/</code><br>
数据准备完成且确认无误后就可以开始训练了。<br>
此次训练起点是预训练过的 <code>resnet50_coco_best_v2.1.0.h5</code>，<code>steps=6000</code>，<code>epochs=5</code>，在 K80 显卡训练的时间大概是 5-6 小时。</p>
<pre><code>python keras-retinanet/bin/train.py --weight /snapshots/resnet50_coco_best_v2.1.0.h5 --gpu 0 csv /path/to/annotations /path/to/class_label_mapping --steps 6000 --epochs 5
</code></pre>
<h2 id="评价">评价</h2>
<p>训练完成后，我们需要用 1000 条测试数据对模型的 performance 做出评价。我在准备评价数据时，发现标注数据（我们标注的 Ground Truth）存在大约 5% 的错误分类。这些错误分类是我通过人工辨识 model 预测的结果与 GT 的差别而得到的。也就是说，刚刚训练好的 model 帮助我找到了很多标注数据的错误标注！</p>
<p>虽然这些错误标注在训练的时候不会产生太大影响（否则也不会帮我找错），但在做评价时会严重影响模型的 performance。可能需要借助刚训练好的 model 对 1000 个测试数据做清洗。</p>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

