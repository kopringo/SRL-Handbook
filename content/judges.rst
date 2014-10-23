Judges
======

5. Build-in judges and master judges
------------------------------------

We have mentioned that the problem setter can choose from default test cases judges and master judges. In practice it is usually enough.

Test case judes
~~~~~~~~~~~~~~~

        <p>There are five default test case judges:
          <ul>
            <li><strong>Strict</strong> - it requires output files to be identical</li>
            <li><strong>Ignoring differences in whitespaces</strong> - similar to the previous one but it ignores all extra whitespaces. It is the standard judge for all problems with exactly one correct solution (in particular for binary problems).
            <div class="example-box">
              <p>For example consider the problem of prime factorisation of the number (i.e. for a number <em>n</em> find the prime factors <em>p<sub>1</sub>, p<sub>2</sub>, ..., p<sub>k</sub></em> that <em>n = p<sub>1</sub>&middot;p<sub>2</sub>&middot;...&middot;p<sub>k</sub></em>). It is known that the prime factorisation of the number is unique up to the order of prime factors so if we require in output specification to write sorted list of factors there is only one good answer to the problem.</p>
            </div>
            </li>
            <li><strong>Ignoring floating point errors up to <em>10<sup>-2</sup></em></strong> - it allows the floating point numbers to be inaccurate i.e. we can accept the errors up to <em>0.01</em>. It is good choice for problems when the result is an approximation which don't require good precision.

               <div class="example-box">
                  <p>For example consider the problem of the triangle area, i.e. for given integer side lenghts <em>a,b,c</em> calculate the area of the triangle. In general the result is not an integer and accuracy is not crucial.</p>
               </div>

            </li>
            <li><strong>Ignoring floating point errors up to <em>10<sup>-6</sup></em></strong> - it allows the floating point numbers to be inaccurate i.e. we can accept the errors up to <em>0.000001</em>. When you need a good precision it is obviously better choice than the previous judge.

                     <div class="example-box">
                  <p>For example consider the problem of the value of the sine function. The range of the sine function is <em>[-1,1]</em> thus the precision is important.</p>
               </div>

            </li>
            <li><strong>Score is source length</strong> - it is the modification of the <em>Ignoring differences in whitespaces</em> judge i.e. it verifies the correctness of the solution in the same manner but in addition it returns the score which is the length of the source code. The judge is designed for challange type problems where the objective is to solve the problem with the shortest source code.

                     <div class="example-box">
                  <p>For example consider the problem of determining first <em>n</em> numbers in decimal expansion of the number &pi; for given <em>n</em>. The challange is to solve that problem with the shortest possible source code.</p>
               </div>

            </li>
          </ul>
        </p>

Master judes
~~~~~~~~~~~~

        <p>We have two default master judges both were described in section <a href="#problems">problems</a>:
            <ul>
              <li><strong>Generic masterjudge</strong> - it gathers information from test case judges and requires each of them to achieve <em>"accepted"</em> as the result to establish final result as the <em>"accepted"</em>. When any test case ends with error the final answer is inherited from the first failed test case. For example when the problem has five test cases and the second and the fourth ones failed, the final result is inherited from the second test case. Generic masterjudge combines the execution times of all testcases and yields the sum as the final score.
              <p>It is a proper choice when the problem setter requires that the solution fulfills all his requirements i.e. it is correct and sufficiently efficient.</p>
              </li>
              <li><strong>Score is % of correctly solved sets</strong> - it is a more liberal masterjudge which allows to accept incomplete solution with the score which is the percentage of correctly solved test cases. For example when the problem has five test cases and again the second and the fourth ones failed but the rest was passed, the final score is equal to <em>60%</em>. The advantage is that the user gets more information about the correctness level of its solution.
              <p>It is a proper choice when the problem setter wants to distinguish user's solutions. It is possible to design test cases to be easier or more difficult to pass.</p>

               <div class="example-box">
                  <p>For example consider the problem of power function i.e. for (possibly big) integer numbers <em>a</em> and <em>b</em> calculate the value of <em>a<sup>b</sup></em>. The first test case can deliver only input instances for which the result is in the standard numeric type scope. Another test case can require from the solution to implement the big numbers model. These two test cases give an information on the advancement of the solution. The third test case could also take into account the aspect of the performance and distinguish solutions implementing naive algorithms from the better ones which implement the fast power algorithm.</p>
                  <p>The least advanced (but in some way correct) solutions will pass the first test case and achieve the result of <em>33%</em> while the more complex solutions (implementing big numbers) are able to pass the first and the second test and achieve the result of <em>66%</em>. To achieve the best result of <em>100%</em> the solution needs to implement both big numbers and fast power algorithms to pass all three test cases.</p>
               </div>
              </li>
            </ul>
        </p>
        
        
        
Advanced test case judges
-------------------------

        <p>In the previous section we have discussed default test case judges which are sufficient for most situations. However there are problems which require individual solutions due to nature of the problem. In this section we present examples of the problems along with descriptions of test case judges.</p>

        <p>Test case judge has access to the following information:</p>
        <ul>
         <li>model input</li>
         <li>model output</li>
         <li>user's output</li>
         <li>user's source code</li>
        </ul>

        <h3>Impossible model output file</h3>

         <p>Consider following problem task:</p>

         <div class="example-box">
               For given function <em>f</em> find its root i.e. the argument <em>x<sub>0</sub></em> that <em>f(x<sub>0</sub>) = 0</em>.
         </div>

         <p>In general there are many solutions to the problem, for example for polynomial <em>x<sup>2</sup> + x - 2</em> the numbers <em>1</em> and <em>-2</em> are both correct answers. You can see that it is hard to prepare model output file in test case. There are possibly infinitely many solutions for the certain functions thus it is impossible to keep all of them in the output file. It forces us to use different approach.</p>

         <div class="example-box">
               <p><strong>Judge description: </strong> The test case judge should verify the condition from the problem task i.e. for the user's answer from the output file it should check if that answer is a root of the function.</p>
               <p>Test case judge uses his access to model input file to read the problem instance.</p>
         </div>


        <h3>Ambiguous model output file</h3>

         <p>Consider the example:</p>

         <div class="example-box">
               For given graph <em>G</em> with <em>n</em> vertices <em>1, 2, ..., n</em> determine if it has hamiltonian cycle (i.e. closed loop through a graph that visits each node exactly once). If the hamiltonian cycle exists print it as a sequence of vertices.
         </div>

         <p>It is easy to see that <em>1-2-3-1</em> is the same cycle as <em>2-3-1-2</em>. We could add the requirement to start with the smallest vertex number. Unfortunately it is possible that there exists many different hamiltonian cycles which are not cyclic shifts. We could again use the previous approach and verify if user's answer is really hamiltonian cycle. Alternatively we can build model output file with all possible hamiltonian cycles:</p>

         <div class="example-box">
               <p><strong>Judge description: </strong> For user's answer the judge looks for that specific one on the list contained in model output file.</p>
         </div>

         <p><strong>Remark</strong> It can be problematic to keep all answers due to possible huge number of good solutions.</p>


Advanced master judges
----------------------

        <p>Similarly to test case judges it is possible to create custom master judges. In certain situations the problem setter may want to extend functionality of existing master judge or even implement brand new one. In this section we present examples of the master judges along with a motivation.</p>

        <p>Master judge has access to the following information:</p>
        <ul>
         <li>results from test case judes</li>
         <li>user's source code</li>
        </ul>

Weighted % of correctly solved sets
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

         <p>Here we present the generalisation of <em>Score is % of correctly solved sets</em> master judge. It was a little disadventage that each test case is worth the same and to increase to influence of some submission's aspect you were forced to produce many test cases.</p>

         <p>For example when your test cases verify three aspects <em>A,B</em> and <em>C</em> of the problem and you would like to put weights <em>20%</em>, <em>30%</em> and <em>50%</em> respectively, you were able to do that by creating <em>10</em> test cases. Two of them responsible for an aspect <em>A</em>, three of them responsible for an aspect <em>B</em> and five of them responsible for an aspect <em>C</em>. However it is inconvenient and you can consider following idea:</p>

         <div class="example-box">
            <p><strong>Master judge description:</strong> Master judge has the information about the number of test cases and weights which it should assign to each test case. The final score is the weighted sum of accepted test cases.</p>
            <p>For example for three test cases <em>a,b,c</em> and weights <em>20%, 30%, 50%</em> the submission gets one of the possible results depending on passed test cases:
            <ul>
               <li><strong>no test case passed</strong> - 0%</li>
               <li><strong>a</strong> - 20%</li>
               <li><strong>b</strong> - 30%</li>
               <li><strong>a,b</strong> - 50%</li>
               <li><strong>c</strong> - 50%</li>
               <li><strong>a,c</strong> - 70%</li>
               <li><strong>b,c</strong> - 80%</li>
               <li><strong>a,b,c</strong> - 100%</li>
            </ul>
            </p>
         </div>

Forbidden structures in source code
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The problem setter may require that the solution cannot use some programming structures. 
For example he may want to allow to use language <em>C++</em> but with no access to STL 
library to force users to implement efficent data structures manually. Another example 
is to restrict source codes to not use loop structures to support only solutions based on recursion.

         <div class="example-box">
            <p><strong>Master judge description:</strong> Master judge uses access to the user's source code to detect usages of forbidden keywords (for example loops: while, for, goto). When forbidden keyword is detected the final status is set to <em>wrong aswer</em> in other case the master judge performs classical verification (for example the same as Generic masterjudge).</p>
         </div>
         