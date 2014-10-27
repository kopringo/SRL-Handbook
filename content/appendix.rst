Appendix
========

.. _appendix-good-test-cases-design:

Good test cases design
----------------------

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

.. _appendix-statuses:

Statuses
--------

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
               <p>For a positive integer *n* calculate the value of the sum of all positive integers that are not greater than *n* i.e. *1 + 2 + 3 + ... + n*. For example when *n = 5* then the correct answer is *15*.</p>
          <p>
            <strong>Input:</strong> In the first line there will be the number *1 <= t <= 10000000* which is the number of instances for your problem. In each of the next *t* lines there will be one number *n* for which you should calculate the described initial sum.
          </p>
          <p>
            <strong>Output:</strong> For each *n* print the calculated initial sum. Separate answers with new line character.
          </p>
         </div>

The first error which can occur is the *compilation error*, for example submitting the following source code would produce the CE status:

.. code-block:: cpp

   #include <stdio.h>
   
   long long initsum(long long n)
   {
     return n*(n+1)/2;
   }
   
   int main()
   {
     int t // missing semicolon
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

To obtain *execution error* we can refer to unallocated memory:

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
       scanf("%lld", n); // referring to unallocated memory 
       printf("%lld\n", initsum(n));
       t--;
     }
     return 0;
   }

We will *exceed time limit* with worse algorithm (if test cases are rich enough):

.. code-block:: cpp

   #include <stdio.h>
   
   // suboptimal algorithm
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

Bad output formatting causes *wrong answer* status:

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
       printf("%lld", initsum(n)); // missing new line character
       t--;
     }
     return 0;
   }

At the end we present correct and optimal solution which passes all test cases and obtains *accepted* status:

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
