---


---

<h1 id="mapreduce----word-count-example">MapReduce – Word Count Example</h1>
<h2 id="mapper">Mapper</h2>
<blockquote>
<p>Mapper maps input (key, value) pairs from HDFS into intermediate (key, value) pairs and save them into local file system.<br>
E.g.<br>
Input: (docID, doc)<br>
Output: (term, 1)</p>
</blockquote>
<h3 id="mapper-class-prototype">Mapper Class Prototype:</h3>
<pre class=" language-java"><code class="prism  language-java">Mapper<span class="token operator">&lt;</span>Object<span class="token punctuation">,</span> Text<span class="token punctuation">,</span> Text<span class="token punctuation">,</span> IntWritable<span class="token operator">&gt;</span> 
<span class="token comment">// Object:: INPUT_KEY</span>
<span class="token comment">// Text:: INPUT_VALUE</span>
<span class="token comment">// Text:: OUTPUT_KEY</span>
<span class="token comment">// IntWritable:: OUTPUT_VALUE</span>
</code></pre>
<h3 id="special-data-types-in-hadoop">Special Data Types in Hadoop</h3>
<h4 id="intwritable">IntWritable</h4>
<p>A <strong>serializable</strong> and <strong>comparable</strong> object for <strong>integer</strong>.<br>
Example:</p>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">private</span> <span class="token keyword">final</span> <span class="token keyword">static</span> IntWritable one <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">IntWritable</span><span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h4 id="text">Text</h4>
<p>A <strong>serializable</strong>, <strong>deserializable</strong> and <strong>comparable</strong> object for <strong>string</strong> at <strong>byte</strong> level. It stores text in UTF-8 encoding.<br>
Example:</p>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">private</span> Text word <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Text</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<blockquote>
<p>Hadoop defines its own classes for general data types.<br>
– All “values” must have Writable interface;<br>
– All “keys”  must have WritableComparable interface;</p>
</blockquote>
<h3 id="map-method-for-mapper">Map Method for Mapper</h3>
<h4 id="method-header">Method header</h4>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">map</span><span class="token punctuation">(</span>Object key<span class="token punctuation">,</span> Text value<span class="token punctuation">,</span> Context context
               <span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> InterruptedException
<span class="token comment">// Object key:: Declare data type of input key;</span>
<span class="token comment">// Text value:: Declare data type of input value;</span>
<span class="token comment">// Context context:: Declare data type of output. Context is often used for output data collection.</span>
</code></pre>
<h4 id="tokenization">Tokenization</h4>
<pre class=" language-java"><code class="prism  language-java"><span class="token comment">// Use Java built-in StringTokenizer to split input value (document) into words:</span>
StringTokenizer itr <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">StringTokenizer</span><span class="token punctuation">(</span>value<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h4 id="building-key-value-pairs">Building (key, value) pairs</h4>
<pre class=" language-java"><code class="prism  language-java"><span class="token comment">// Loop over all words:</span>
<span class="token keyword">while</span> <span class="token punctuation">(</span>itr<span class="token punctuation">.</span><span class="token function">hasMoreTokens</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
  <span class="token comment">// convert built-in String back to Text:</span>
  word<span class="token punctuation">.</span><span class="token function">set</span><span class="token punctuation">(</span>itr<span class="token punctuation">.</span><span class="token function">nextToken</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token comment">// build (key, value) pairs into Context and emit:</span>
  context<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>word<span class="token punctuation">,</span> one<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<h3 id="map-method-summary">Map Method Summary</h3>
<blockquote>
<p>Mapper class produces Mapper.Context object, which comprise a series of (key, value) pairs</p>
</blockquote>
<pre class=" language-java"><code class="prism  language-java">  <span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">map</span><span class="token punctuation">(</span>Object key<span class="token punctuation">,</span> Text value<span class="token punctuation">,</span> Context context
                  <span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> InterruptedException <span class="token punctuation">{</span>
    StringTokenizer itr <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">StringTokenizer</span><span class="token punctuation">(</span>value<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token keyword">while</span> <span class="token punctuation">(</span>itr<span class="token punctuation">.</span><span class="token function">hasMoreTokens</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
      word<span class="token punctuation">.</span><span class="token function">set</span><span class="token punctuation">(</span>itr<span class="token punctuation">.</span><span class="token function">nextToken</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
      context<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>word<span class="token punctuation">,</span> one<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
  <span class="token punctuation">}</span>
</code></pre>
<h3 id="overview-of-mapper-class">Overview of Mapper Class</h3>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">static</span> <span class="token keyword">class</span> <span class="token class-name">TokenizerMapper</span>
     <span class="token keyword">extends</span> <span class="token class-name">Mapper</span><span class="token operator">&lt;</span>Object<span class="token punctuation">,</span> Text<span class="token punctuation">,</span> Text<span class="token punctuation">,</span> IntWritable<span class="token operator">&gt;</span><span class="token punctuation">{</span>

  <span class="token keyword">private</span> <span class="token keyword">final</span> <span class="token keyword">static</span> IntWritable one <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">IntWritable</span><span class="token punctuation">(</span><span class="token number">1</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token keyword">private</span> Text word <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">Text</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

  <span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">map</span><span class="token punctuation">(</span>Object key<span class="token punctuation">,</span> Text value<span class="token punctuation">,</span> Context context
                  <span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> InterruptedException <span class="token punctuation">{</span>
    StringTokenizer itr <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">StringTokenizer</span><span class="token punctuation">(</span>value<span class="token punctuation">.</span><span class="token function">toString</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token keyword">while</span> <span class="token punctuation">(</span>itr<span class="token punctuation">.</span><span class="token function">hasMoreTokens</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span> <span class="token punctuation">{</span>
      word<span class="token punctuation">.</span><span class="token function">set</span><span class="token punctuation">(</span>itr<span class="token punctuation">.</span><span class="token function">nextToken</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
      context<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>word<span class="token punctuation">,</span> one<span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<hr>
<h2 id="reducer">Reducer</h2>
<blockquote>
<p>Reducer receives (key, values) pairs and aggregate values to a desired format, then write produced (key, value) pairs back into HDFS.</p>
</blockquote>
<h3 id="reducer-class-prototype">Reducer Class Prototype:</h3>
<pre class=" language-java"><code class="prism  language-java">Reducer<span class="token operator">&lt;</span>Text<span class="token punctuation">,</span> IntWritable<span class="token punctuation">,</span> Text<span class="token punctuation">,</span> IntWritable<span class="token operator">&gt;</span> 
<span class="token comment">// Text:: INPUT_KEY</span>
<span class="token comment">// IntWritable:: INPUT_VALUE</span>
<span class="token comment">// Text:: OUTPUT_KEY</span>
<span class="token comment">// IntWritable:: OUTPUT_VALUE</span>
</code></pre>
<h3 id="reduce-method-for-mapper">Reduce Method for Mapper</h3>
<h4 id="method-header-1">Method header</h4>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">reduce</span><span class="token punctuation">(</span>Text key<span class="token punctuation">,</span> Iterable<span class="token operator">&lt;</span>IntWritable<span class="token operator">&gt;</span> values<span class="token punctuation">,</span>
                     Context context
                     <span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> InterruptedException 
<span class="token comment">// Text key:: Declare data type of input key;</span>
<span class="token comment">// Iterable&lt;IntWritable&gt; values:: Declare data type of input values; (Note: Received values from mapper should be in a list)</span>
<span class="token comment">// Context context:: Declare data type of output. Context is often used for output data collection.</span>
</code></pre>
<h4 id="aggregate-values">Aggregate Values</h4>
<pre class=" language-java"><code class="prism  language-java"><span class="token comment">// Iterate through all the values wrt the key:</span>
<span class="token keyword">int</span> sum <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>
<span class="token keyword">for</span> <span class="token punctuation">(</span>IntWritable val <span class="token operator">:</span> values<span class="token punctuation">)</span> <span class="token punctuation">{</span>
  sum <span class="token operator">+=</span> val<span class="token punctuation">.</span><span class="token function">get</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token punctuation">}</span>
</code></pre>
<h4 id="building-key-value-pairs-1">Building (key, value) pairs</h4>
<pre class=" language-java"><code class="prism  language-java"><span class="token comment">// Convert built-in int into IntWritable</span>
result<span class="token punctuation">.</span><span class="token function">set</span><span class="token punctuation">(</span>sum<span class="token punctuation">)</span><span class="token punctuation">;</span>
<span class="token comment">// build (key, value) pair into Context and emit:</span>
context<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>key<span class="token punctuation">,</span> result<span class="token punctuation">)</span><span class="token punctuation">;</span>
</code></pre>
<h3 id="reducer-class-summary">Reducer Class Summary</h3>
<blockquote>
<p>Reducer class produces Reducer.Context object and serialize obtained (key, value) pair into HDFS.</p>
</blockquote>
<h3 id="overview-of-reducer-class">Overview of Reducer Class</h3>
<pre class=" language-java"><code class="prism  language-java"><span class="token keyword">public</span> <span class="token keyword">static</span> <span class="token keyword">class</span> <span class="token class-name">IntSumReducer</span>
     <span class="token keyword">extends</span> <span class="token class-name">Reducer</span><span class="token operator">&lt;</span>Text<span class="token punctuation">,</span>IntWritable<span class="token punctuation">,</span>Text<span class="token punctuation">,</span>IntWritable<span class="token operator">&gt;</span> <span class="token punctuation">{</span>
  <span class="token keyword">private</span> IntWritable result <span class="token operator">=</span> <span class="token keyword">new</span> <span class="token class-name">IntWritable</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>

  <span class="token keyword">public</span> <span class="token keyword">void</span> <span class="token function">reduce</span><span class="token punctuation">(</span>Text key<span class="token punctuation">,</span> Iterable<span class="token operator">&lt;</span>IntWritable<span class="token operator">&gt;</span> values<span class="token punctuation">,</span>
                     Context context
                     <span class="token punctuation">)</span> <span class="token keyword">throws</span> IOException<span class="token punctuation">,</span> InterruptedException <span class="token punctuation">{</span>
    <span class="token keyword">int</span> sum <span class="token operator">=</span> <span class="token number">0</span><span class="token punctuation">;</span>
    <span class="token keyword">for</span> <span class="token punctuation">(</span>IntWritable val <span class="token operator">:</span> values<span class="token punctuation">)</span> <span class="token punctuation">{</span>
      sum <span class="token operator">+=</span> val<span class="token punctuation">.</span><span class="token function">get</span><span class="token punctuation">(</span><span class="token punctuation">)</span><span class="token punctuation">;</span>
    <span class="token punctuation">}</span>
    result<span class="token punctuation">.</span><span class="token function">set</span><span class="token punctuation">(</span>sum<span class="token punctuation">)</span><span class="token punctuation">;</span>
    context<span class="token punctuation">.</span><span class="token function">write</span><span class="token punctuation">(</span>key<span class="token punctuation">,</span> result<span class="token punctuation">)</span><span class="token punctuation">;</span>
  <span class="token punctuation">}</span>
<span class="token punctuation">}</span>
</code></pre>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

