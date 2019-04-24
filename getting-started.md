# Getting Started

Before learning any thing, I always ask set of questions to myself like, what is this ? Why is this ? How is this ? What makes it ? What is its history ? What is its scope ? How it can be helpful in current scenario ? 

Once I get all the answers of all these question, I make myself educated in basics of that thing. On that knowledge again questions can be formed and same process is repeated. 

I would like to do the same thing to understand the docker from the scratch. 

Lets Start :

Lets understand the docker with a scenario. You started a company. You got a team of 2 dev, 1 qa , and 1 ops guy. At initial phase devs are working on the coding a monolithic application. Consider this is a simple application of hotel booking. 1 dev is working on front end , other is working on back end. When phase I is done , they decided to release the initial version. Question here is how to get the code to QA’s system for testing. QA guys test all feature on testing environment. Since this is first product from the company so we have branch new testing env with no other stuff on that host.

Question: How to get code from dev’s laptop to Qa’s env ? 

Points to cover :

History of software supply chain 

 -&gt; What is software supply chain 

 -&gt; How software used to be shipped to tester and then to production in old times 

 -&gt; What were the problems while shipping 

 -&gt; app1 got deployed and running in production after a long release cycle. 

 -&gt; app2 is ready to be released but app2 uses other version of libraries , framework. This app2 is planned to installed 

     on same prod host. Problems with dependencies. 

 -&gt; Analogy of container while shipping goods 

 -&gt; Same analogy is used while shipping software and this is done by docker. 

 -&gt; initial approach of promoting software using VM and its problems

 -&gt; So docker is a company which releases its product which is uses in software supply chain. 

 -&gt; Process in docker container 

