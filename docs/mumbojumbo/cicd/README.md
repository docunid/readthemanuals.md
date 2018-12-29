# Pipeline Development

Development principles and way-of-working in a CI/CD environment.

When developing with multiple people in a single repository you face some challenges. Among others you will need artifacts and resources to run and test your code. In short you will need a development environment closely resembling the real (production) environment where your code is going to run. And preferably you'll also want to incorporate certain - if not all - latest code/features your co-workers have developed or are developing.

Development starts with branching from the main line, thus creating a separate line of coding a.k.a. feature branch. This feature branch will eventually merge back into the main line (better sooner than later, the Small Batches Principle) but during the lifecycle of a feature branch you will need an environment specific to that feature. We call this a feature branch environment or `fbe` for short. During active development a multitude of fbe's can be present at any moment.

*Figure one:*

```txt
fb_A                  o----------o-----o
                     /        CI        \ MR      
master -------------o--------o----o------o------o------ CD
                              \                / MR
                               \       CI    /
fb_B                            o---o---o--o  

```

Feature branch (environment) A "fb_A" is created by branching from the main (master) line. "fb_A" and its environment ceases to exist just before the merge request (MR) into branch master. During the lifecycle of a feature branch (environment) we do development and continuous integration (CI). Once merged into master the continuous deployment starts (CD)

## Feature branch environment (fbe)

What is a feature branch environment?
Well...basically everything needed to closely/temporary mimic the master branch or production environment so we can run and test the code in the feature branch. Think artifacts (Amazon Machine Images), resources (Amazon Web Services) and code needed to get results in development that resembles a production environment as much as possible. You can look at it as a `feature branch specific pipeline` (more on that later).

The lifeclye of a fbe contains 3 stages:

- Create fbe > when branching from master
- Update fbe > during active development of code
- Destroy fbe > just before merging into master and starting cd

### Exception

There is one special branch that is an exception to this lifecyle: `master`. This branch will be created once and never destroyed. This is your main line that deploys into production.   

### Moving parts

An example. When developing you will create a feature branch fb_A and commit code. Creation of a branch triggers the branch manager (a meta pipeline). The branch manager will create a pipeline tailored to feature branch fb_A. That feature branch specific pipeline will then commence to create, update, use and destroy artifacts and resources, based on the committed code. In flow:

- create branch/commit code
- branch manager kicks in and creates a pipeline for fb_A
- pipeline for fb_A uses artifacts
- pipeline for fb_A creates resources such as VPC, EC2, SQS, S3, Lambda

### Artifacts, build or use

The complete set you need is of course related to the functionality you're developing. It would be overkill to instantiate a feature branch specific pipeline that creates an AMI when all you want to do is adding a security group.

In general, binary cloud artifacts like an AMI are created or referenced in the code. The latter for example if you are developing an additional security group, then you would have no need for a build job for the AMI. Instead you just want to use the AMI, not rebuild it. In that case the AMI ID will be copied into the fbe by the branch manager (more on that later). You can then use that ID in your code without needing to run a build job for an artifact that already exists.

### Create feature branch environment

The trigger for creating a feature branch environment is the creation of a feature branch in VCS. At that point the branch manager should create a production-like albeit temporary environment that is able to develop and run the code in the feature branch. In other words: the branch manager creates a `feature branch specific pipeline`. Of course changing the environment will be part of the development most of the times. So the feature branch environment will be updated frequently by the pipeline.  

### Update feature branch environment

The trigger for updating a feature branch environment is a code commit to the feature branch. A commit can or should (depending on the code committed) result in updating the feature branch environment.

### Destroy feature branch environment

The trigger for destroying a feature branch environment is a merge request. Before (!) accepting the merge request the feature branch environment should be destroyed. The feature branch will cease to exist and so must the temporary feature branch environment. After the environment is destroyed, the merge request can be accepted and the code merged into master (barring merge conflicts). After that the pipeline should start building the master branch.