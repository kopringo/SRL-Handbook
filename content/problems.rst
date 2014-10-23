
Problems
========

Problems and submissions
------------------------

In this section we are going to give you insight into submission flow and online judge problem 
construction. From the previous section we know basics of the concept of the online judging. 
To better understand and make it possible to create own problems we will describe components of 
the problem:

**Description**
- The task
- Input / output specification
- Input / output examples

**Test case(s)** (possibly many)
- Input file, Output file
- Time limit
- Judge

**Master judge**


Before we begin the analysys of the individual components let us take a loot at the complete 
diagram of submission flow to embed the listed components in overall view.

.. image:: ../_static/full_diagram.png
   :alt: Full submission flow diagram
   :width: 700px
   :align: center

The diagram is the extension of the one from the previous section. The main difference is the 
appearance of the *master judge* component. Since we have introduced possibility of many test 
cases (input, output and judge) thus we need the summary component to combine the results from 
test case judges - we call that summary component *master judge*. The other new elements of the 
diagram was mentally present before but we purposely omited them to keep the previous view as 
simple as possible. Now we can explain that every submission needs to have precised 
*programming language* to choose proper compiler or interpreter. The problem description is the 
crucial part for users to make it possible to solve the problem.

Now let us recall the *Integer Power* problem since we are going to use it as a demonstration 
example. The task described in the problem is to compute the value of *a<sup>b</sup>* for given 
integers *a* and *b*

Description
~~~~~~~~~~~

User must be able to understand the problem to solve it successfully. Description contains 
complete information needed to solve the problem thus it is the most important feature from 
the user's point of view.

**The task**
The formal text that describes the specificity of the problem usually in mathematical manner. 
To achieve good quality of the description it is a good habit to graphically illustrate the 
problem. It is also desirable to support a dry theory with simple examples.

.. note::
   According to the *Integer Power* problem we could write the taks as follows:
   
   Write the program which takes two numbers *a* and *b* and returns the value of *a<sup>b</sup>*. 
   For example for *a = 3* and *b = 4* the result is *3^4 = 81*
   

**Input / output specification**

The previous part was formal from the mathematical perspective but not syntactically. Automatic 
judging is only possible when we can make an assumption on the problems's input and output behaviour. 
The interior of the user's program is a black box for us right now. We specify what is the input file 
structure to make it possible for users to implement proper input reading. The specification of input 
file should be an information that user can rely on.

On the other hand we expect that user produces output file with a predictable structure. Formal 
specification of the output file is an information that we are going to rely on to verify the correctness 
of the submission.

        <p>Referring to the *Integer Power* problem let us recall <a href="#judge-source-code">possible solution</a> of the problem. We didn't specify any formal input or output syntax in the previous section. First attempt was the solution with the human readable interface and after that we modified the code to achieve a raw version. We can use that source code as a base for following formal specification:
          <div class="example-box">
            <p>
              <strong>Input:</strong> In the only line of the input there will be two integer numbers *1 <= a <= 8* and *0 <= b <= 10* separated by a single space character.</p>
            <p>
              <strong>Output:</strong> Program should write a single number which is a value of *a<sup>b</sup>*.
            </p>
          </div>
        </p>

        <h4>Input / output examples</h4>
        <p>In the task subsection we mentioned that it is a good habit to ilustrate the problem with the examples. The examples here are dedicated to ilustrate the input and output files structure. In the best case scenario they cover every distinct configuration of parameters (up to numbers, letters etc.) which is important for more complex problems.</p>

        <p>Referring to the *Integer Power* problem we present how we could compose the examples:
          <div class="example-box">
            <p>
            <strong>Example 1:</strong><br />
            &nbsp;&nbsp;<strong>Input:</strong><br />
            &nbsp;&nbsp;3 4<br />
            &nbsp;&nbsp;<strong>Output:</strong><br />
            &nbsp;&nbsp;81<br />
            </p>
            <p>
            <strong>Example 2:</strong><br />
            &nbsp;&nbsp;<strong>Input:</strong><br />
            &nbsp;&nbsp;7 0<br />
            &nbsp;&nbsp;<strong>Output:</strong><br />
            &nbsp;&nbsp;1<br />
            </p>
          </div>
        </p>
          
Test case(s)
~~~~~~~~~~~~

Just as the description was for users only so the test cases are for the machine checker. 
This is the essence of the automatic judging idea. The vast majority of the usages implements 
the following schema: there is a model input paired to a model output along with the program 
which can compare that model output with the user's output to decide whether user's answer is 
good or not.
 
**Input and output files**
Input file contains the problem instance and it must be consistent with the input specification. 
The output file should contain corresponding correct answers formatted in accordance to the 
output specification.

of the correct program should be contained in the output file. It is not necessery to write the 
solution to the program to create the output file - it can be obtained in any manner.

According to the *Integer Power* problem we present how we could prepare following test cases:

.. note::
   **Test case 1:**
   **Input file:**
   3 4
   **Output file:**
   81

   **Test case 2:**
   **Input file:**
   7 0
   **Output file:**
   1


**Remark.** It is recomended to construct the problems that are able to repeat the desired procedure 
as many times as we want to make it possible to test the user's submission with one test case. 
There are many reasons for that approach and for further information please visit 
<a href="#good-test-cases-design">good test cases design</a> appendix.</p>

**Time limit**
We have already pointed that one of the features of online judging is the possiblity of estimating 
the time complexity. To achieve that the author of the problem has to adjust the timeout for program 
execution. Consider the case when the author knows two different algorithms for a problem, say *A* and *B*. 
Let us assume that the algorithm *A* is noticeably faster than the algorithm *B*. It is not very 
easy and obvious how to preper test cases to distinguish between these two algorithms. However, 
assuming that we have input data which is processed in the time *t<sub>A</sub>* for the algorithm *A* 
which is much faster than execution time *t<sub>B</sub>* for the algorithm *B* we can simply set the 
time limit somewhere between those values.</p>

With the timeout *t<sub>A</sub> <= t<sub>0</sub> <= t<sub>B</sub>* we can assume that *A*-like algorithms 
will pass the test case and *B*-like algorithms will fail it due to exceeding the time limit.
        
**Remark.** Note that the presented approach highly depends on the machine thus you need to adjust your 
time limit to the computing cluster rather then your local machine.

Our toy example problem is much too simple in assumptions to allow us to present example of time limits 
that distinguish different algorithms thus we put default time limit of *1s*. In the next section we present 
more complex example where we further discuss the time limit which can help to estimate the algorithm quality.

**Judge**
The judge is a program which process user's output file after execution. Its task is to establish if the 
submission passed the test case and potentially also returns *the score*. When the user's program pass the 
test case the returned status is *"accepted"*.

Usually the judge implementation is reduced to compare the model output file with the user's output file. 
We support problem setters with default judges:
- **Strict** - it requires output files to be identical 
- **Ignoring differences in whitespaces** - similar to the previous one but it ignores all extra whitespaces
- ""Ignoring floating point errors up to a specific position** - it allows the floating point numbers to be 
  inaccurate i.e. we can accept the errors up to for example *0.001*


**Remark.** The *Ignoring differences in whitespaces* judge is one of the most popular default choice. It is 
more liberal for output formating errors which in fact doesn't affect on the solution semantic correctness. 
Similarly *Ignoring floating point errors up to a specific position* judge is popular choice for problems 
where result numbers are not integers.

We have mentioned that the judge can also return *the score*. More information will be presented in the section 
<a href="#advanced-judges">advanced test case judges</a>, for now you can assume that the score is the test 
case execution time.

It is possible to create custom test case judges. The author can implement any kind of verification having full 
access to the input file, base input file, user's output file and even user's source code. For more information 
visit the section <a href="#advanced-judges">advanced test case judges</a>.

For the *Integer Power* problem we decide to use default *Ignoring differences in whitespaces* judge for each 
test case thus we allow the user to generate extra whitespaces before and after the resulting number *a<sup>b</sup>*. 
For example when user's solution prints *"&nbsp;&nbsp;&nbsp;81&nbsp;&nbsp;&nbsp;&nbsp;"* as a result for *"3 4"* 
problem instance it is still correct answer.

**Master judge**
We have discussed the individual test cases for the problem and established that each of them returns information, 
i.e. status and the score. The master judge is the component which combines all incoming results obtained from test 
cases to produce the final result which is the status and the score. You can look again at the 
<a href="#submission-flow-diagram">submission flow diagram</a> for better understanding.

There are predefined master judges proper for most situations:
- **Generic masterjudge** - it gathers information from test case judges and requires each of them to achieve *"accepted"* 
  as the result to establish final result as the *"accepted"*. When any test case ends with error the final answer is 
  inherited from the first failed test case. For example when the problem has five test cases and the second and the 
  fourth ones failed, the final result is inherited from the second test case. Generic masterjudge combines the execution 
  times of all testcases and yields the sum as the final score.
- **Score is % of correctly solved sets** - it is a more liberal masterjudge which allows to accept incomplete solution 
  with the score which is the percentage of correctly solved test cases. For example when the problem has five test cases 
  and again the second and the fourth ones failed but the rest was passed, the final score is equal to *60%*. The advantage 
  is that the user gets more information about the correctness level of its solution.

When you need to use more complex master judge it is possible to create the new one or modify the existing ones. You have 
access to the source code of default master judges and they can be used as a base for your modifications. Further information 
about designing master judges you can find in the section <a href="advanced-master-judges">Advanced master judges</a>

The last missing part for the example we successively improve is the choice of master judge. We created two test cases and 
there is no need to implement the specific own master judge thus we select default one. When we need to distinguish the 
solutions as better or worst (but both correct) we should rather choose *Score is % of correctly solved sets* but in our 
situation each test case is a pure verification of correctness (i.e. no performance aspects tested) thus we select 
*Generic masterjudge* to force the user's solution to pass all test cases.

      
**Complete toy example specification**
We have discussed all components of the problem specification therefore we are able to present whole problem setting:

.. warning::
   
   The Integer Power
   
   **Description**
   Write the program which takes two numbers *a* and *b* and returns the value of *a<sup>b</sup>*. For example 
   for *a = 3* and *b = 4* the result is *3<sup>4</sup> = 81*
   
   **Input / output specification:**
                    <div class="left-indent">
                      <p>
                        <strong>Input:</strong> In the only line of the input there will be two integer numbers *1 <= a <= 8* and *0 <= b <= 10* separated by a single space character.</p>
                      <p>
                        <strong>Output:</strong> Program should write a single number which is a value of *a<sup>b</sup>*.</strong>
                      </p>
                    </div>
                  </p>
                  <p>
              <strong>Examples:</strong>
              <div class="left-indent">
                    <p>
                    <strong>Example 1:</strong><br />
                    &nbsp;&nbsp;<strong>Input:</strong><br />
                    &nbsp;&nbsp;3 4<br />
                    &nbsp;&nbsp;<strong>Output:</strong><br />
                    &nbsp;&nbsp;81<br />
                    </p>
                    <p>
                    <strong>Example 2:</strong><br />
                    &nbsp;&nbsp;<strong>Input:</strong><br />
                    &nbsp;&nbsp;7 0<br />
                    &nbsp;&nbsp;<strong>Output:</strong><br />
                    &nbsp;&nbsp;1<br />
                    </p>
                    </div>
                  </p>
                </li>
                <li><h4>Test cases</h4>
                  <div class="left-indent">
                  <p>
                    <p><strong>Test case 1:</strong></p>
                    <p>
                      &nbsp;&nbsp;<strong>Input file:</strong><br />
                      &nbsp;&nbsp;3 4<br />
                      &nbsp;&nbsp;<strong>Output file:</strong><br />
                      &nbsp;&nbsp;81<br />
                      </p>
                      <p><strong>Judge</strong> - Ignoring differences in whitespaces</p>
                      <p><strong>Time limit</strong> - 1s</p>
                  </p>
                  <p>
                    <p><strong>Test case 2:</strong></p>
                    <p>
                      &nbsp;&nbsp;<strong>Input file:</strong><br />
                      &nbsp;&nbsp;7 0<br />
                      &nbsp;&nbsp;<strong>Output file:</strong><br />
                      &nbsp;&nbsp;1<br />
                      </p>
                      <p><strong>Judge</strong> - Ignoring differences in whitespaces</p>
                      <p><strong>Time limit</strong> - 1s</p>
                  </p>
                </div>
                </li>
                <li><h4>Master judge</h4>
                  <div class="left-indent">Generic master judge</div>
                </li>
              </ul>
          </div>
          
          
          
          
          
          
Submission status
~~~~~~~~~~~~~~~~~

      Ultimately, for a full understanding of the diagram let as briefly comment possible statuses which can be assigned to the submission.
      <ul>
        <li><strong>Accepted (AC)</strong> - the submission is a correct solution to the problem.</li>
        <li><strong>Wrong answer (WA)</strong> - the submission is incorrect solution. It depends on master judge implementation.</li>
        <li><strong>Time limit exceeded (TLE)</strong> - the submission execution took to long. Again it depends on master judge implementation.</li>
        <li><strong>Runtime error (RE)</strong> - the error occurred during program execution.</li>
        <li><strong>Compilation error (CE)</strong> - the error occurred during compilation or syntax validation in interpreter.</li>
        <li><strong>Internal error (IE)</strong> - the error occurred on the serivice side. One of the possible reasons can be poorly designed test case judge or master judge.</li>
      </ul>

      More information along with examples of problems and source codes can be found in appendix <a href="more-about-statuses">statuses</a>.

Example
-------

        <p>In this section we present more complicated example with full details to give you better overall look at abilities of our services. It is still an elementary example but it will tell you much more about the specific of online judging.</p>

        <p>The problem is to count the sum of numbers from *1* to given *n* i.e. *1 + 2 + 3 + ... + n*, we call it the *Initital sum* problem. We are going to preper it to handle with multiple input instances in a single test case by proper input / output specification design. Look at the following problem description:</p>

        <div class="example-box">
          <p>For a positive integer *n* calculate the value of the sum of all positive integers that are not greater than *n* i.e. *1 + 2 + 3 + ... + n*. For example when *n = 5* then the correct answer is *15*.</p>
          <p>
            <strong>Input:</strong> In the first line there will be the number *1 <= t <= 10000000* which is the number of instances for your problem. In each of the next *t* lines there will be one number *n* for which you should calculate the described initial sum.
          </p>
          <p>
            <strong>Output:</strong> For each *n* print the calculated initial sum. Separate answers with new line character.
          </p>

            <p>
            <strong>Example 1:</strong><br />
            &nbsp;&nbsp;<strong>Input:</strong><br />
            &nbsp;&nbsp;4<br />
            &nbsp;&nbsp;1<br />
            &nbsp;&nbsp;2<br />
            &nbsp;&nbsp;3<br />
            &nbsp;&nbsp;4<br />
            &nbsp;&nbsp;<strong>Output:</strong><br />
            &nbsp;&nbsp;1<br />
            &nbsp;&nbsp;3<br />
            &nbsp;&nbsp;6<br />
            &nbsp;&nbsp;10<br />
            </p>
            <p>
            <strong>Example 2:</strong><br />
            &nbsp;&nbsp;<strong>Input:</strong><br />
            &nbsp;&nbsp;2<br />
            &nbsp;&nbsp;10<br />
            &nbsp;&nbsp;11<br />
            &nbsp;&nbsp;<strong>Output:</strong><br />
            &nbsp;&nbsp;55<br />
            &nbsp;&nbsp;66<br />
            </p>
        </div>

        <p>Note that the input specification allows us to construct rich test cases which are able to distinguis between faster and slower solutions.</p>

        <p>Referring to the solution take into account that the possible input data can be too big for the standard *int* type thus we will use the *long long* type. Before we set test cases let us present two distinct solutions to the problem:</p>

        <div id="judge-source-code" class="problem_sourcecode">
<!-- HTML generated using hilite.me --><div style="background: #f8f8f8; overflow:auto;width:auto;border:solid gray;border-width:.1em .1em .1em .1em;padding:.2em .6em;"><pre style="margin: 0; line-height: 125%"><span style="color: #BC7A00">#include &lt;stdio.h&gt;</span>

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

      <p>The first solution directly refers to the definition of the problem i.e. the function *initsum* iterates from *1* to *n* to calculate desired value. The calculation requires *n* operations of addition to obtain the result.</p>

      <p>It is basic school knowledge that there exists the compact formula for that problem and we use it in the second implementation:</p>


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
</pre></div>
      </div>

      <p>Both programs are correct answer to the problem but if we want to distinguish the algorithms we can design test cases that only the second solution can pass. As we mentioned before it highly depends on the computational power of the machine. We present test cases that are valid for the computer of this text's author. Our suggestion is to design one test case which is easy to pass for both algorithms to give information that the solution is correct and the second test case that is possible to pass only for the second algorithm. It can give an information to the user, that his solution is correct but too slow. The user submitting solution similar to the first one will get information about test cases and will be able to see that his program passes first test case and exceed time limit in the second test case.</p>

      <p>We cannot put all input and output data here because of its size thus we write it in shortened manner:</p>

      <div class="example-box">
        <p>
        <strong>Test case 1:</strong><br />
        &nbsp;&nbsp;<strong>Input file:</strong><br />
        &nbsp;&nbsp;1000<br />
        &nbsp;&nbsp;1<br />
        &nbsp;&nbsp;2<br />
        &nbsp;&nbsp;...<br />
        &nbsp;&nbsp;1000<br />
        &nbsp;&nbsp;1000000<br />
        &nbsp;&nbsp;<strong>Output file:</strong><br />
        &nbsp;&nbsp;1<br />
        &nbsp;&nbsp;3<br />
        &nbsp;&nbsp;...<br />
        &nbsp;&nbsp;500500<br />
        &nbsp;&nbsp;500000500000<br />
        </p>
        <p>
        <strong>Test case 2:</strong><br />
        &nbsp;&nbsp;<strong>Input file:</strong><br />
        &nbsp;&nbsp;1000000<br />
        &nbsp;&nbsp;1<br />
        &nbsp;&nbsp;2<br />
        &nbsp;&nbsp;...<br />
        &nbsp;&nbsp;1000000<br />
        &nbsp;&nbsp;<strong>Output file:</strong><br />
        &nbsp;&nbsp;1<br />
        &nbsp;&nbsp;3<br />
        &nbsp;&nbsp;...<br />
        &nbsp;&nbsp;500000500000<br />
        </p>
      </div>

      <p>Computational power of current machines is enough to finish first test case instantly. Both presented algorithms finished computations with time below *0.01s*. However it is a good test case for a corectness verification only. First *1000* positive integers give us the assurance that solution is mathematically correct. We have also added single test with big number i.e. *n = 1000000* to make sure that user's solution bases on *long long* type. On the other hand the second test case is rich enough to make the first algorithm to exceed even *5s* time limit. The second algorithm works fast enough to pass that test case in time below *0.1s*. We have huge gap between *0.1s* and *5s* thus we can easily choose safe value as our time limit, for example again *1s*.</p>

      <p>We still haven't chosen judges for test cases and master judge for the problem. We don't have floating point numbers in our output file specification thus we rather decide to choose *Ignoring differences in whitespaces* judge for both test cases. It leaves users with possiblity of small formating errors without risk of unwanted rejections of theirs solutions. For example it is possible to replace new line characters with spaces in output formatting and still pass the test case.</p>

      <p>We assume that we want to accept every correct solution but distinguish the better ones and give them a better score. The *Score is % of correctly solved sets* master judge is perfect for that purpose. Submitting the first solution achieves the result of *50%* while the second solution passes both test cases and its result is *100%*.</p>

      <p><strong>Remark.</strong> Presented scoring method assumed both tests as equally worth *50%* each. To achieve different distribution of scores you need to modify the master judge and pick the scoring of test cases arbitrary. We present the example in the section <a href="advanced-master-judges">advanced master judges</a>.</p>

      <p>To sum up we present full problem specification:</p>

          <div class="example-box">
            <h3 align="center">The Initial Sum</h3>
            <p>
              <ul>
                <li><h4>Description</h4>
                  <p>For a positive integer *n* calculate the value of the sum of all positive integers that are not greater than *n* i.e. *1 + 2 + 3 + ... + n*. For example when *n = 5* then the correct answer is *15*.</p>
                  <p>
                    <strong>Input / output specification:</strong>
                    <div class="left-indent">
                      <p>
                        <strong>Input:</strong> In the first line there will be the number *1 <= t <= 10000000* which is the number of instances for your problem. In each of the next *t* lines there will be one number *n* for which you should calculate the described initial sum.
                      </p>
                      <p>
                        <strong>Output:</strong> For each *n* print the calculated initial sum. Separate answers with new line character.
                      </p>
                    </div>
                  </p>
                  <p>
              <strong>Examples:</strong>
                  <div class="left-indent">
                      <p>
                      <strong>Example 1:</strong><br />
                      &nbsp;&nbsp;<strong>Input:</strong><br />
                      &nbsp;&nbsp;4<br />
                      &nbsp;&nbsp;1<br />
                      &nbsp;&nbsp;2<br />
                      &nbsp;&nbsp;3<br />
                      &nbsp;&nbsp;4<br />
                      &nbsp;&nbsp;<strong>Output:</strong><br />
                      &nbsp;&nbsp;1<br />
                      &nbsp;&nbsp;3<br />
                      &nbsp;&nbsp;6<br />
                      &nbsp;&nbsp;10<br />
                      </p>
                      <p>
                      <strong>Example 2:</strong><br />
                      &nbsp;&nbsp;<strong>Input:</strong><br />
                      &nbsp;&nbsp;2<br />
                      &nbsp;&nbsp;10<br />
                      &nbsp;&nbsp;11<br />
                      &nbsp;&nbsp;<strong>Output:</strong><br />
                      &nbsp;&nbsp;55<br />
                      &nbsp;&nbsp;66<br />
                      </p>
                    </div>
                  </p>
                </li>
                <li><h4>Test cases</h4>
                  <div class="left-indent">
                  <p>
                    <p><strong>Test case 1:</strong></p>
                    <p>
                      &nbsp;&nbsp;<strong>Input file:</strong><br />
                      &nbsp;&nbsp;1000<br />
                      &nbsp;&nbsp;1<br />
                      &nbsp;&nbsp;2<br />
                      &nbsp;&nbsp;...<br />
                      &nbsp;&nbsp;1000<br />
                      &nbsp;&nbsp;1000000<br />
                      &nbsp;&nbsp;<strong>Output file:</strong><br />
                      &nbsp;&nbsp;1<br />
                      &nbsp;&nbsp;3<br />
                      &nbsp;&nbsp;...<br />
                      &nbsp;&nbsp;500500<br />
                      &nbsp;&nbsp;500000500000<br />
                      </p>
                      <p><strong>Judge</strong> - Ignoring differences in whitespaces</p>
                      <p><strong>Time limit</strong> - 1s</p>
                  </p>
                  <p>
                    <p><strong>Test case 2:</strong></p>
                    <p>
                      &nbsp;&nbsp;<strong>Input file:</strong><br />
                      &nbsp;&nbsp;1000000<br />
                      &nbsp;&nbsp;1<br />
                      &nbsp;&nbsp;2<br />
                      &nbsp;&nbsp;...<br />
                      &nbsp;&nbsp;1000000<br />
                      &nbsp;&nbsp;<strong>Output file:</strong><br />
                      &nbsp;&nbsp;1<br />
                      &nbsp;&nbsp;3<br />
                      &nbsp;&nbsp;...<br />
                      &nbsp;&nbsp;500000500000<br />
                      </p>
                      <p><strong>Judge</strong> - Ignoring differences in whitespaces</p>
                      <p><strong>Time limit</strong> - 1s</p>
                  </p>
                </div>
                </li>
                <li><h4>Master judge</h4>
                  <div class="left-indent">Score is % of correctly solved sets</div>
                </li>
              </ul>
          </div>


4. Good test cases design
-------------------------

            It is common misunderstanding which leads to bad test cases design. The number of test cases assigned to the problem is limited to *64*. The individual test case is not intended to test only one problem instance, we recommend you to redesign your input / output specification to handle with multiple problem instances in one test case. Consider the following very elementary problem as an example:

            <div class="example-box">
               For given integer numbers *a* and *b* calculate the sum *a + b*.
            </div>

            We could design the input / output specification to calculate the sum only for two numbers:

            <div class="example-box">
              <p>
            <strong>Input:</strong> In the only line of the input there will be two integer numbers *a* and *b* separated by a single space character.</p>
          <p>
            <strong>Output:</strong> Program should write a single number which is the value of *a + b*.
          </p>
            </div>

            <p>It is correct but it is highly not recommended. First of all even *64* test cases cover a small part of the possible problem instances. Secondly, the execution of each test case is time consuming (about *2s* additional time for each test case).</p>

            <p>We recommend to redesign the input / output specification in following manner:</p>

            <div class="example-box">
          <p>
            <strong>Input:</strong> In the first line there will be the number *t* which is the number of instances for the problem. In each of the next *t* lines there will be the pair of two numbers *a* and *b* for which you should calculate the value of *a + b*.
          </p>
          <p>
            <strong>Output:</strong> For each *a,b* pair print the calculated sum. Separate answers with new line character.
          </p>
            </div>

            <p>As you can see it is possible to pack a large number of problem instance into single test case.</p>            

            <p>Note that multiple test cases should rather be used to test different aspect of the problem.</p>