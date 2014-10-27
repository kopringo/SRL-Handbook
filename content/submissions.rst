   
Submissions
===========

Ultimately, for a full understanding of the diagram let as briefly comment possible 
statuses which can be assigned to the submission.

**Accepted (AC)**
  the submission is a correct solution to the problem.
  
**Wrong answer (WA)**
  the submission is incorrect solution. It depends on master judge implementation.
  
**Time limit exceeded (TLE)**
  the submission execution took to long. Again it depends on master judge implementation.
  
**Runtime error (RE)**
  the error occurred during program execution.
  
**Compilation error (CE)**
  the error occurred during compilation or syntax validation in interpreter.
  
**Internal error (IE)**
  the error occurred on the serivice side. One of the possible reasons can be poorly 
  designed test case judge or master judge.
  
More information along with examples of problems and source codes can be found in appendix <a href="more-about-statuses">statuses</a>.

Example
-------

In this section we present more complicated example with full details to give you 
better overall look at abilities of our services. It is still an elementary example 
but it will tell you much more about the specific of online judging.

The problem is to count the sum of numbers from *1* to given *n* i.e. *1 + 2 + 3 + ... + n*, 
we call it the *Initital sum* problem. We are going to preper it to handle with multiple 
input instances in a single test case by proper input / output specification design. 
Look at the following problem description:

.. admonition:: Example
  :class: note

   You can make up your own admonition too.


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

.. code-block:: cpp
   
   #include <stdio.h>

   long long initsum(long long n)
   {
     int i;
     long long sum = 0;
     for (i=1; i <= n; i++)
     {
       sum += i;
     }
     return sum;
   }
   
   int main()
   {
     int t;
     long long n;
     scanf("%d", &t);
     while (t > 0)
     {
       scanf("%lld", &n);
       printf("%lld\n", initsum(n));
       t--;
     }
     return 0;
   }

The first solution directly refers to the definition of the problem i.e. the function *initsum* iterates from *1* to *n* to calculate desired value. The calculation requires *n* operations of addition to obtain the result.

It is basic school knowledge that there exists the compact formula for that problem and we use it in the second implementation:

.. code-block:: cpp
   
   #include <stdio.h>
   
   long long initsum(long long n)
   {
     return n*(n+1)/2;
   }
   
   int main()
   {
     int t;
     long long n;
     scanf("%d", &t);
     while (t > 0)
     {
       scanf("%lld", &n);
       printf("%lld\n", initsum(n));
       t--;
     }
     return 0;
   }

Both programs are correct answer to the problem but if we want to distinguish the algorithms we can design test cases that only the second solution can pass. As we mentioned before it highly depends on the computational power of the machine. We present test cases that are valid for the computer of this text's author. Our suggestion is to design one test case which is easy to pass for both algorithms to give information that the solution is correct and the second test case that is possible to pass only for the second algorithm. It can give an information to the user, that his solution is correct but too slow. The user submitting solution similar to the first one will get information about test cases and will be able to see that his program passes first test case and exceed time limit in the second test case.

We cannot put all input and output data here because of its size thus we write it in shortened manner:

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