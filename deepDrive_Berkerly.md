---


---

<blockquote>
<p>Berkeley 大学最近推出的针对自动驾驶的街景数据集，号称比 Cityscapes 数据量更大，可泛化性更好。</p>
</blockquote>
<h1 id="语义实例分割（semantic-instance-segmentation）">语义实例分割（Semantic Instance Segmentation）</h1>
<p>数据集一共有 40 种物体类别</p>
<h2 id="与-cityscapes-的对比">与 Cityscapes 的对比</h2>
<h3 id="街景数据来自-us-的城市">街景数据来自 US 的城市</h3>
<p>模型更熟悉美国的街景。</p>
<h3 id="图片标签">图片标签</h3>
<p>时间：daytime, nighttime, dawn/dusk;<br>
场景：Residential，High-way, City street, Parking lot, Gas station, Tunnel;<br>
天气：Clear, Partly cloudy, Over-case, Rainy, Snowy, Foggy;</p>
<h3 id="label-maps">Label Maps</h3>
<p>语义分割使用标签映射（Label Maps），不是训练索引（Training Indices）。</p>
<h3 id="更高的可泛化性">更高的可泛化性</h3>
<p>使用 Dilate Residual Network （Hyper parameter 相同）测试两个数据集时发现下表的关系：</p>

<table>
<thead>
<tr>
<th>Train</th>
<th>Test</th>
<th>Accuracy</th>
</tr>
</thead>
<tbody>
<tr>
<td>deepDriver</td>
<td>deepDriver</td>
<td>High</td>
</tr>
<tr>
<td>deepDriver</td>
<td>Cityscapes</td>
<td>Low</td>
</tr>
<tr>
<td>Cityscapes</td>
<td>deepDriver</td>
<td>Low</td>
</tr>
<tr>
<td>Cityscapes</td>
<td>Cityscapes</td>
<td>High</td>
</tr>
</tbody>
</table><p>在同样的数据集下训练结果都很好，但交叉使用不同测试集时精度下降显著。使用 deepDriver 训练的模型在 Cityscapes 测试集上的表现虽然较差，但有部分训练结果比在特定场景训练的结果要好。这意味着该数据集涵盖场景更多，训练出的模型的可泛化性会比较好。</p>
<p>以上参考：<a href="https://arxiv.org/abs/1805.04687">https://arxiv.org/abs/1805.04687</a></p>
<h2 id="数据集详情">数据集详情</h2>
<p>文件结构：</p>
<pre><code>bdd100k
|   seg
|    |  images 
|    |    |  train
|    |    |  val
|    |    |  test
|    |  color_labels
|    |    |  train
|    |    |  val
|    |  labels
|    |    |  train
|    |    |  val
</code></pre>
<p>检查数据集完整性的 <code>python3</code> 脚本</p>
<pre class=" language-python"><code class="prism  language-python"><span class="token keyword">import</span> os
<span class="token keyword">import</span> sys 

<span class="token keyword">if</span>  <span class="token builtin">len</span><span class="token punctuation">(</span>sys<span class="token punctuation">.</span>argv<span class="token punctuation">)</span> <span class="token operator">!=</span>  <span class="token number">2</span><span class="token punctuation">:</span>
	<span class="token keyword">print</span> <span class="token punctuation">(</span><span class="token string">'Usage: python checkdata.py &lt;train|val&gt;'</span><span class="token punctuation">)</span>
	exit<span class="token punctuation">(</span><span class="token operator">-</span><span class="token number">1</span><span class="token punctuation">)</span>

dataset_category <span class="token operator">=</span> sys<span class="token punctuation">.</span>argv<span class="token punctuation">[</span><span class="token number">1</span><span class="token punctuation">]</span>
<span class="token keyword">if</span> dataset_category <span class="token operator">not</span>  <span class="token keyword">in</span> <span class="token punctuation">{</span><span class="token string">'train'</span><span class="token punctuation">,</span> <span class="token string">'val'</span><span class="token punctuation">}</span><span class="token punctuation">:</span>
	<span class="token keyword">print</span> <span class="token punctuation">(</span>f<span class="token string">'Invalid argument "{dataset_category}"'</span><span class="token punctuation">)</span>
	exit<span class="token punctuation">(</span><span class="token operator">-</span><span class="token number">2</span><span class="token punctuation">)</span>

data_size <span class="token operator">=</span> <span class="token number">7000</span> <span class="token keyword">if</span> dataset_category <span class="token operator">==</span> <span class="token string">'train'</span> <span class="token keyword">else</span> <span class="token number">1000</span>

dir_root <span class="token operator">=</span>  <span class="token string">'.'</span>
dir_color <span class="token operator">=</span> os<span class="token punctuation">.</span>path<span class="token punctuation">.</span>join<span class="token punctuation">(</span>dir_root<span class="token punctuation">,</span> <span class="token string">'color_labels'</span><span class="token punctuation">,</span> dataset_category<span class="token punctuation">)</span>
dir_imgs <span class="token operator">=</span> os<span class="token punctuation">.</span>path<span class="token punctuation">.</span>join<span class="token punctuation">(</span>dir_root<span class="token punctuation">,</span> <span class="token string">'images'</span><span class="token punctuation">,</span> dataset_category<span class="token punctuation">)</span>
dir_label <span class="token operator">=</span> os<span class="token punctuation">.</span>path<span class="token punctuation">.</span>join<span class="token punctuation">(</span>dir_root<span class="token punctuation">,</span> <span class="token string">'labels'</span><span class="token punctuation">,</span> dataset_category<span class="token punctuation">)</span>

color_names <span class="token operator">=</span> os<span class="token punctuation">.</span>listdir<span class="token punctuation">(</span>dir_color<span class="token punctuation">)</span>
img_names <span class="token operator">=</span> os<span class="token punctuation">.</span>listdir<span class="token punctuation">(</span>dir_imgs<span class="token punctuation">)</span>
label_names <span class="token operator">=</span> os<span class="token punctuation">.</span>listdir<span class="token punctuation">(</span>dir_label<span class="token punctuation">)</span>

<span class="token keyword">assert</span> <span class="token builtin">len</span><span class="token punctuation">(</span>color_names<span class="token punctuation">)</span> <span class="token operator">==</span>  <span class="token builtin">len</span><span class="token punctuation">(</span>img_names<span class="token punctuation">)</span> <span class="token operator">==</span>  <span class="token builtin">len</span><span class="token punctuation">(</span>label_names<span class="token punctuation">)</span> <span class="token operator">==</span> data_size

<span class="token keyword">for</span> i <span class="token keyword">in</span> <span class="token builtin">range</span><span class="token punctuation">(</span><span class="token builtin">len</span><span class="token punctuation">(</span>color_names<span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">:</span>
	prefix_color <span class="token operator">=</span> color_names<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">.</span>split<span class="token punctuation">(</span><span class="token string">'_'</span><span class="token punctuation">)</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span>
	prefix_img <span class="token operator">=</span> img_names<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">.</span>split<span class="token punctuation">(</span><span class="token string">'.'</span><span class="token punctuation">)</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span>
	prefix_label <span class="token operator">=</span> label_names<span class="token punctuation">[</span>i<span class="token punctuation">]</span><span class="token punctuation">.</span>split<span class="token punctuation">(</span><span class="token string">'_'</span><span class="token punctuation">)</span><span class="token punctuation">[</span><span class="token number">0</span><span class="token punctuation">]</span>
	<span class="token keyword">assert</span> prefix_color <span class="token operator">==</span> prefix_img <span class="token operator">==</span> prefix_label<span class="token punctuation">,</span> f<span class="token string">'{prefix_color}, {prefix_img}, {prefix_label}'</span>

<span class="token keyword">print</span> <span class="token punctuation">(</span><span class="token string">'All Good!'</span><span class="token punctuation">)</span>
</code></pre>
<p>包含分割多边形信息的 <code>Json</code> 文件目前还没有公开，因此只能做segmentation，不能做 detection + segmentation。但是单纯的 detection 数据文件已经是提供好的，可以使用<a href="https://github.com/ucbdrive/bdd-data/blob/master/bdd_data/show_labels.py">查看工具</a>查看标注矩形框和三种图片标签（时间、场景、天气）<br>
<img src="https://img2018.cnblogs.com/blog/1457052/201809/1457052-20180916003545872-1733776961.png" alt=""></p>
<h3 id="官方代码目前的坑">官方代码目前的坑</h3>
<p><a href="https://github.com/ucbdrive/bdd-data/issues/17">https://github.com/ucbdrive/bdd-data/issues/17</a><br>
<a href="https://github.com/ucbdrive/bdd-data/issues/5">https://github.com/ucbdrive/bdd-data/issues/5</a><br>
<a href="https://github.com/ucbdrive/bdd-data/issues/15">https://github.com/ucbdrive/bdd-data/issues/15</a><br>
其中，#15 issue 目前还未解决。</p>
<hr>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>

