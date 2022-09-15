# ==Docker==

Think of Pen and Writing env (hypothetical scenario in office explaining docker) analogy for explaining docker

&emsp;
### What is Docker?

Put simply, Docker is a platform for containerizing applications. Applications that are packaged using docker run in isolated (w.r.t the host system) environments called as containers.

&emsp;
### What are containers?

Containers are runnable instances of an application (to be more precise image of an application). A Docker container runs in isolation from the other processes of the host system. They are lightweight and hence are portable as it contains all the necessary dependencies to run an application. Because of the isolated and self-sustainable nature of the containers there is no dependence whatsover on the host system.

Containers exhibit the same behaviour irrespective of where it is being run or by whom it is being run. Reliability and Reproducibility

&emsp;
### What problem does containers solve?

Before Docker, the steps involved in installing, running, testing and sharing an application could be complex depending on the size and scale of the application. For example, say a web application requires a database for it's operations and a team of developers who work on different machines with different operating systems are working on the application.
- Installation process for the application and the database might be different in different operating systems and between different versions of the same operating system
- The version of the database installed might differ from the required version
- Clashing versions between different projects
 
In order to address these issues the development team can choose to maintain a well-written documentation. But, however well-written a documentation may be, it still doesn't guarantee things to work out smoothly as expected. There is still scope for technical and human errors in the process. 
- The developers may not follow the documentation correctly
- The version of the software required may not be supported by the operating system

Also, the above example is for a simple system involving only a web application and a database. Applications in real life can easily be way more complex than this and in production environments when you think about scaling multiple services in a microservice architecture things could easily get out of hand.

Summary of the pain points that Docker addresses:
- Behaviour of software in different computers can be uncertain at times. Docker brings some level of certainity to the process.
- Docker helps solve the age-old '*it works on my machine*' problem by employing a build once run anywhere concept in its solution which makes applications portable and reliable across machines.








