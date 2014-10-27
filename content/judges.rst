.. _judges-normal:

Judges
======

We have mentioned that the problem setter can choose from default test cases judges and master judges. In practice it is usually enough.

Test case judges
----------------

There are five default test case judges:

**Strict**
  it requires output files to be identical
  
**Ignoring differences in whitespaces**
  similar to the previous one but it ignores all extra whitespaces. It is the standard 
  judge for all problems with exactly one correct solution (in particular for binary problems).
  
.. note::
   For example consider the problem of prime factorisation of the number (i.e. for a number 
   *n* find the prime factors *p\ :sub:`1`, p\ :sup:`2`, ..., p\ :sup:`k`* 
   that *n = p<sub>1</sub>&middot;p<sub>2</sub>&middot;...&middot;p<sub>k</sub>*). 
   It is known that the prime factorisation of the number is unique up to the order of prime 
   factors so if we require in output specification to write sorted list of factors there is 
   only one good answer to the problem.</p>

**Ignoring floating point errors up to 10^-2**
  it allows the floating point numbers to be inaccurate i.e. we can accept the errors up 
  to *0.01*. It is good choice for problems when the result is an approximation which don't 
  require good precision.
  
.. note::
   For example consider the problem of the triangle area, i.e. for given integer side lenghts 
   *a,b,c* calculate the area of the triangle. In general the result is not an integer and 
   accuracy is not crucial.

**Ignoring floating point errors up to 10^-6**
  it allows the floating point numbers to be inaccurate i.e. we can accept the errors up 
  to *0.000001*. When you need a good precision it is obviously better choice than the previous judge.

.. note::
   For example consider the problem of the value of the sine function. The range of the sine 
   function is *[-1,1]* thus the precision is important.

**Score is source length**
   it is the modification of the *Ignoring differences in whitespaces* judge i.e. it verifies 
   the correctness of the solution in the same manner but in addition it returns the score which 
   is the length of the source code. The judge is designed for challange type problems where the 
   objective is to solve the problem with the shortest source code.

.. note::
   For example consider the problem of determining first *n* numbers in decimal expansion of the 
   number &pi; for given *n*. The challange is to solve that problem with the shortest possible source code.

.. _master-judges-normal:

Master judges
-------------

We have two default master judges both were described in section <a href="#problems">problems

**Generic masterjudge**
  it gathers information from test case judges and requires each of them to achieve *"accepted"* as 
  the result to establish final result as the *"accepted"*. When any test case ends with error the final 
  answer is inherited from the first failed test case. For example when the problem has five test cases 
  and the second and the fourth ones failed, the final result is inherited from the second test case. 
  Generic masterjudge combines the execution times of all testcases and yields the sum as the final score.
  
  It is a proper choice when the problem setter requires that the solution fulfills all his requirements 
  i.e. it is correct and sufficiently efficient.
  
**Score is % of correctly solved sets**
   it is a more liberal masterjudge which allows to accept incomplete solution with the score which is the 
   percentage of correctly solved test cases. For example when the problem has five test cases and again 
   the second and the fourth ones failed but the rest was passed, the final score is equal to *60%*. 
   The advantage is that the user gets more information about the correctness level of its solution.
   
   It is a proper choice when the problem setter wants to distinguish user's solutions. It is possible to 
   design test cases to be easier or more difficult to pass.
   
.. note::
   For example consider the problem of power function i.e. for (possibly big) integer numbers *a* and *b* 
   calculate the value of *a<sup>b</sup>*. The first test case can deliver only input instances for which 
   the result is in the standard numeric type scope. Another test case can require from the solution to 
   implement the big numbers model. These two test cases give an information on the advancement of the solution. 
   The third test case could also take into account the aspect of the performance and distinguish solutions 
   implementing naive algorithms from the better ones which implement the fast power algorithm.
   
   The least advanced (but in some way correct) solutions will pass the first test case and achieve the result 
   of *33%* while the more complex solutions (implementing big numbers) are able to pass the first and the second 
   test and achieve the result of *66%*. To achieve the best result of *100%* the solution needs to implement 
   both big numbers and fast power algorithms to pass all three test cases.
        
.. _judges-advanced:
        
Advanced test case judges
-------------------------

In the previous section we have discussed default test case judges which are sufficient for most situations. 
However there are problems which require individual solutions due to nature of the problem. In this section 
we present examples of the problems along with descriptions of test case judges.

Test case judge has access to the following information:
 - model input
 - model output
 - user's output
 - user's source code

Impossible model output file
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

         <p>Consider following problem task:</p>

         <div class="example-box">
               For given function *f* find its root i.e. the argument *x<sub>0</sub>* that *f(x<sub>0</sub>) = 0*.
         </div>

         <p>In general there are many solutions to the problem, for example for polynomial *x<sup>2</sup> + x - 2* the numbers *1* and *-2* are both correct answers. You can see that it is hard to prepare model output file in test case. There are possibly infinitely many solutions for the certain functions thus it is impossible to keep all of them in the output file. It forces us to use different approach.</p>

         <div class="example-box">
               <p><strong>Judge description: </strong> The test case judge should verify the condition from the problem task i.e. for the user's answer from the output file it should check if that answer is a root of the function.</p>
               <p>Test case judge uses his access to model input file to read the problem instance.</p>
         </div>


        <h3>Ambiguous model output file</h3>

         <p>Consider the example:</p>

         <div class="example-box">
               For given graph *G* with *n* vertices *1, 2, ..., n* determine if it has hamiltonian cycle (i.e. closed loop through a graph that visits each node exactly once). If the hamiltonian cycle exists print it as a sequence of vertices.
         </div>

         <p>It is easy to see that *1-2-3-1* is the same cycle as *2-3-1-2*. We could add the requirement to start with the smallest vertex number. Unfortunately it is possible that there exists many different hamiltonian cycles which are not cyclic shifts. We could again use the previous approach and verify if user's answer is really hamiltonian cycle. Alternatively we can build model output file with all possible hamiltonian cycles:</p>

         <div class="example-box">
               <p><strong>Judge description: </strong> For user's answer the judge looks for that specific one on the list contained in model output file.</p>
         </div>

         <p><strong>Remark</strong> It can be problematic to keep all answers due to possible huge number of good solutions.</p>

.. _master-judges-advanced:

Advanced master judges
----------------------

Similarly to test case judges it is possible to create custom master judges. In certain situations 
the problem setter may want to extend functionality of existing master judge or even implement 
brand new one. In this section we present examples of the master judges along with a motivation.

        <p>Master judge has access to the following information:</p>
        <ul>
         <li>results from test case judges</li>
         <li>user's source code</li>
        </ul>

**Weighted % of correctly solved sets**

         <p>Here we present the generalisation of *Score is % of correctly solved sets* master judge. It was a little disadventage that each test case is worth the same and to increase to influence of some submission's aspect you were forced to produce many test cases.</p>

         <p>For example when your test cases verify three aspects *A,B* and *C* of the problem and you would like to put weights *20%*, *30%* and *50%* respectively, you were able to do that by creating *10* test cases. Two of them responsible for an aspect *A*, three of them responsible for an aspect *B* and five of them responsible for an aspect *C*. However it is inconvenient and you can consider following idea:</p>

         <div class="example-box">
            <p><strong>Master judge description:</strong> Master judge has the information about the number of test cases and weights which it should assign to each test case. The final score is the weighted sum of accepted test cases.</p>
            <p>For example for three test cases *a,b,c* and weights *20%, 30%, 50%* the submission gets one of the possible results depending on passed test cases:
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

**Forbidden structures in source code**

The problem setter may require that the solution cannot use some programming structures. 
For example he may want to allow to use language *C++* but with no access to STL 
library to force users to implement efficent data structures manually. Another example 
is to restrict source codes to not use loop structures to support only solutions based on recursion.

         <div class="example-box">
            <p><strong>Master judge description:</strong> Master judge uses access to the user's source code to detect usages of forbidden keywords (for example loops: while, for, goto). When forbidden keyword is detected the final status is set to *wrong aswer* in other case the master judge performs classical verification (for example the same as Generic masterjudge).</p>
         </div>
         