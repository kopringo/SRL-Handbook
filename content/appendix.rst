Appendix
========

Appendix: Statuses
------------------

There are two levels when the status is assigned to the submission:

 - **test case** the status produced by the test case judge
 - **master judge**

The master judge is a high order level component and it can arbitrary assign any status to 
the submission. We are going to focus on the test case judge statuses.

We separate statuses into two groups: semantic and systemic. The semantic statuses are 
strictly related to the correctness of the answer to the problem. On the other hand, 
the systemic statuses are syntactic related and the judge gets it from the system.

**Semantic statuses**
 - **Accepted (AC)** the submission is a correct solution to the problem.
 - **Wrong answer (WA)** the submission is an incorrect solution.     

**Sytemic statuses**
 - Time limit exceeded (TLE): the submission execution took too long.
 - Runtime error (RE): the error occurred during program execution.
 - Compilation error (CE): the error occurred during compilation or syntax validation in interpreter.
 - Internal error (IE): the error occurred on the serivice side. One of the possible reasons can be poorly designed test case judge or master judge.


The Integral error covers wide area of errors thus in the near future we will introduce another type of error for judge and master judge errors.

To ilustrate errors consider again the following example:

         <div class="example-box">
               <p>For a positive integer <em>n</em> calculate the value of the sum of all positive integers that are not greater than <em>n</em> i.e. <em>1 + 2 + 3 + ... + n</em>. For example when <em>n = 5</em> then the correct answer is <em>15</em>.</p>
          <p>
            <strong>Input:</strong> In the first line there will be the number <em>1 <= t <= 10000000</em> which is the number of instances for your problem. In each of the next <em>t</em> lines there will be one number <em>n</em> for which you should calculate the described initial sum.
          </p>
          <p>
            <strong>Output:</strong> For each <em>n</em> print the calculated initial sum. Separate answers with new line character.
          </p>
         </div>

         <p>The first error which can occur is the <em>compilation error</em>, for example submitting the following source code would produce the CE status:</p>

        <div id="judge-source-code" class="problem_sourcecode">
<!-- HTML generated using hilite.me --><div style="background: #f8f8f8; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .1em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%"><span style="color: #BC7A00">#include &lt;stdio.h&gt;</span>

<span style="color: #B00040">long</span> <span style="color: #B00040">long</span> <span style="color: #0000FF">initsum</span>(<span style="color: #B00040">long</span> <span style="color: #B00040">long</span> n)
{
  <span style="color: #008000; font-weight: bold">return</span> n<span style="color: #666666">*</span>(n<span style="color: #666666">+1</span>)<span style="color: #666666">/2</span>;
}

<span style="color: #B00040">int</span> <span style="color: #0000FF">main</span>()
{
  <span style="color: #B00040">int</span> t <span style="color: #408080; font-style: italic">// missing semicolon</span>
  <span style="color: #B00040">long</span> <span style="color: #B00040">long</span> n;
  scanf(<span style="color: #BA2121">&quot;%d&quot;</span>, <span style="color: #666666">&amp;</span>t);
  <span style="color: #008000; font-weight: bold">while</span> (t <span style="color: #666666">&gt;</span> <span style="color: #666666">0</span>)
  {
    scanf(<span style="color: #BA2121">&quot;%lld&quot;</span>, <span style="color: #666666">&amp;</span>n);
    printf(<span style="color: #BA2121">&quot;%lld</span><span style="color: #BB6622; font-weight: bold">\n</span><span style="color: #BA2121">&quot;</span>, initsum(n));
    t<span style="color: #666666">--</span>;
  }
  <span style="color: #008000; font-weight: bold">return</span> <span style="color: #666666">0</span>;
}
</pre></div>
            </div>

      <p>To obtain <em>execution error</em> we can refer to unallocated memory:</p>

      <div id="judge-source-code" class="problem_sourcecode">
<!-- HTML generated using hilite.me --><div style="background: #f8f8f8; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .1em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%"><span style="color: #BC7A00">#include &lt;stdio.h&gt;</span>

<span style="color: #B00040">long</span> <span style="color: #B00040">long</span> <span style="color: #0000FF">initsum</span>(<span style="color: #B00040">long</span> <span style="color: #B00040">long</span> n)
{
  <span style="color: #008000; font-weight: bold">return</span> n<span style="color: #666666">*</span>(n<span style="color: #666666">+1</span>)<span style="color: #666666">/2</span>;
}

<span style="color: #B00040">int</span> <span style="color: #0000FF">main</span>()
{
  <span style="color: #B00040">int</span> t;
  <span style="color: #B00040">long</span> <span style="color: #B00040">long</span> n;
  scanf(<span style="color: #BA2121">&quot;%d&quot;</span>, <span style="color: #666666">&amp;</span>t);
  <span style="color: #008000; font-weight: bold">while</span> (t <span style="color: #666666">&gt;</span> <span style="color: #666666">0</span>)
  {
    scanf(<span style="color: #BA2121">&quot;%lld&quot;</span>, n); <span style="color: #408080; font-style: italic">// referring to unallocated memory </span>
    printf(<span style="color: #BA2121">&quot;%lld</span><span style="color: #BB6622; font-weight: bold">\n</span><span style="color: #BA2121">&quot;</span>, initsum(n));
    t<span style="color: #666666">--</span>;
  }
  <span style="color: #008000; font-weight: bold">return</span> <span style="color: #666666">0</span>;
}
</pre></div>
      </div>

      <p>We will <em>exceed time limit</em> with worse algorithm (if test cases are rich enough):</p>

      <div id="judge-source-code" class="problem_sourcecode">
<!-- HTML generated using hilite.me --><div style="background: #f8f8f8; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .1em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%"><span style="color: #BC7A00">#include &lt;stdio.h&gt;</span>

<span style="color: #408080; font-style: italic">// suboptimal algorithm</span>
<span style="color: #B00040">long</span> <span style="color: #B00040">long</span> <span style="color: #0000FF">initsum</span>(<span style="color: #B00040">long</span> <span style="color: #B00040">long</span> n)
{
  <span style="color: #B00040">int</span> i;
  <span style="color: #B00040">long</span> <span style="color: #B00040">long</span> sum <span style="color: #666666">=</span> <span style="color: #666666">0</span>;
  <span style="color: #008000; font-weight: bold">for</span> (i<span style="color: #666666">=1</span>; i <span style="color: #666666">&lt;=</span> n; i<span style="color: #666666">++</span>)
  {
    sum <span style="color: #666666">+=</span> i;
  }
  <span style="color: #008000; font-weight: bold">return</span> sum;
}

<span style="color: #B00040">int</span> <span style="color: #0000FF">main</span>()
{
  <span style="color: #B00040">int</span> t;
  <span style="color: #B00040">long</span> <span style="color: #B00040">long</span> n;
  scanf(<span style="color: #BA2121">&quot;%d&quot;</span>, <span style="color: #666666">&amp;</span>t);
  <span style="color: #008000; font-weight: bold">while</span> (t <span style="color: #666666">&gt;</span> <span style="color: #666666">0</span>)
  {
    scanf(<span style="color: #BA2121">&quot;%lld&quot;</span>, <span style="color: #666666">&amp;</span>n);
    printf(<span style="color: #BA2121">&quot;%lld</span><span style="color: #BB6622; font-weight: bold">\n</span><span style="color: #BA2121">&quot;</span>, initsum(n));
    t<span style="color: #666666">--</span>;
  }
  <span style="color: #008000; font-weight: bold">return</span> <span style="color: #666666">0</span>;
}
</pre></div>
      </div>

      <p>Bad output formatting causes <em>wrong answer</em> status:</p>

      <div id="judge-source-code" class="problem_sourcecode">
<!-- HTML generated using hilite.me --><div style="background: #f8f8f8; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .1em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%"><span style="color: #BC7A00">#include &lt;stdio.h&gt;</span>

<span style="color: #B00040">long</span> <span style="color: #B00040">long</span> <span style="color: #0000FF">initsum</span>(<span style="color: #B00040">long</span> <span style="color: #B00040">long</span> n)
{
  <span style="color: #008000; font-weight: bold">return</span> n<span style="color: #666666">*</span>(n<span style="color: #666666">+1</span>)<span style="color: #666666">/2</span>;
}

<span style="color: #B00040">int</span> <span style="color: #0000FF">main</span>()
{
  <span style="color: #B00040">int</span> t;
  <span style="color: #B00040">long</span> <span style="color: #B00040">long</span> n;
  scanf(<span style="color: #BA2121">&quot;%d&quot;</span>, <span style="color: #666666">&amp;</span>t);
  <span style="color: #008000; font-weight: bold">while</span> (t <span style="color: #666666">&gt;</span> <span style="color: #666666">0</span>)
  {
    scanf(<span style="color: #BA2121">&quot;%lld&quot;</span>, <span style="color: #666666">&amp;</span>n);
    printf(<span style="color: #BA2121">&quot;%lld&quot;</span>, initsum(n)); <span style="color: #408080; font-style: italic">// missing new line character</span>
    t<span style="color: #666666">--</span>;
  }
  <span style="color: #008000; font-weight: bold">return</span> <span style="color: #666666">0</span>;
}
</pre></div>
      </div>

      <p>At the end we present correct and optimal solution which passes all test cases and obtains <em>accepted</em> status:</p>

      <div id="judge-source-code" class="problem_sourcecode">
<!-- HTML generated using hilite.me --><div style="background: #f8f8f8; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .1em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%"><span style="color: #BC7A00">#include &lt;stdio.h&gt;</span>

<span style="color: #B00040">long</span> <span style="color: #B00040">long</span> <span style="color: #0000FF">initsum</span>(<span style="color: #B00040">long</span> <span style="color: #B00040">long</span> n)
{
  <span style="color: #008000; font-weight: bold">return</span> n<span style="color: #666666">*</span>(n<span style="color: #666666">+1</span>)<span style="color: #666666">/2</span>;
}

<span style="color: #B00040">int</span> <span style="color: #0000FF">main</span>()
{
  <span style="color: #B00040">int</span> t;
  <span style="color: #B00040">long</span> <span style="color: #B00040">long</span> n;
  scanf(<span style="color: #BA2121">&quot;%d&quot;</span>, <span style="color: #666666">&amp;</span>t);
  <span style="color: #008000; font-weight: bold">while</span> (t <span style="color: #666666">&gt;</span> <span style="color: #666666">0</span>)
  {
    scanf(<span style="color: #BA2121">&quot;%lld&quot;</span>, <span style="color: #666666">&amp;</span>n);
    printf(<span style="color: #BA2121">&quot;%lld</span><span style="color: #BB6622; font-weight: bold">\n</span><span style="color: #BA2121">&quot;</span>, initsum(n));
    t<span style="color: #666666">--</span>;
  }
  <span style="color: #008000; font-weight: bold">return</span> <span style="color: #666666">0</span>;
}
</pre>
