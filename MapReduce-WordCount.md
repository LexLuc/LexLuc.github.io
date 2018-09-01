---


---

<h1 id="mapreduce----word-count-example">MapReduce – Word Count Example</h1>
<h2 id="mapper">Mapper</h2>
<blockquote>
<p>Mapper maps input key/value pairs into intermediate key/value pairs.<br>
E.g.<br>
Input: (docID, doc)<br>
Output: (term, 1)</p>
</blockquote>
<h3 id="mapper-class-prototype">Mapper Class Prototype:</h3>
<pre><code>Mapper&lt;Object, Text, Text, IntWritable&gt; 
// Object:: INPUT_KEY
// Text:: INPUT_VALUE
// Text:: OUTPUT_KEY
// IntWritable:: OUTPUT_VALUE
</code></pre>
<h3 id="special-data-type-for-mapper">Special Data Type for Mapper</h3>
<h4 id="intwritable">IntWritable</h4>
<p>A <strong>serializable</strong> and <strong>comparable</strong> object for <strong>integer</strong>.<br>
Example:<br>
<code>private final static IntWritable one = new IntWritable(1);</code></p>
<h4 id="text">Text</h4>
<p>A <strong>serializable</strong>, <strong>deserializable</strong> and <strong>comparable</strong> object for <strong>string</strong> at <strong>byte</strong> level. It stores text in UTF-8 encoding.<br>
Example:<br>
<code>private Text word = new Text();</code></p>
<blockquote>
<p>Hadoop defines its own classes for general data types.<br>
– All “values” must have Writable interface;<br>
– All “keys”  must have WritableComparable interface;</p>
</blockquote>
<h3 id="map-method-for-mapper">Map Method for Mapper</h3>
<h4 id="method-header">Method header</h4>
<pre><code>public void map(Object key, Text value, Context context
               ) throws IOException, InterruptedException
// Object key:: Declare data type of input key;
// Text value:: Declare data type of input value;
// Context context:: Declare data type of output. Context is often used for output data collection.
</code></pre>
<h4 id="tokenization">Tokenization</h4>
<pre><code>// Use Java built-in StringTokenizer to split input value (document) into words:
StringTokenizer itr = new StringTokenizer(value.toString());
</code></pre>
<h4 id="building-key-value-pairs">Building (key, value) pairs</h4>
<pre><code>// Loop over all words:
while (itr.hasMoreTokens()) {
  // convert built-in String back to Text:
  word.set(itr.nextToken());
  // build (key, value) pairs into Context and emit:
  context.write(word, one);
}
</code></pre>
<h3 id="map-method-summary">Map Method Summary</h3>
<blockquote>
<p>Mapper class produces Mapper.Context object, which comprise a series of (key, value) pairs</p>
</blockquote>
<pre><code>  public void map(Object key, Text value, Context context
                  ) throws IOException, InterruptedException {
    StringTokenizer itr = new StringTokenizer(value.toString());
    while (itr.hasMoreTokens()) {
      word.set(itr.nextToken());
      context.write(word, one);
    }
  }
</code></pre>
<h3 id="overview-of-mapper-class">Overview of Mapper Class</h3>
<pre><code>public static class TokenizerMapper
     extends Mapper&lt;Object, Text, Text, IntWritable&gt;{

  private final static IntWritable one = new IntWritable(1);
  private Text word = new Text();

  public void map(Object key, Text value, Context context
                  ) throws IOException, InterruptedException {
    StringTokenizer itr = new StringTokenizer(value.toString());
    while (itr.hasMoreTokens()) {
      word.set(itr.nextToken());
      context.write(word, one);
    }
  }
}
</code></pre>
<blockquote>
<p>Written with <a href="https://stackedit.io/">StackEdit</a>.</p>
</blockquote>

