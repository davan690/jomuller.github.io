Thank you for you comment. I agree with you, today, Docker seems to be more [compliant to the constraints of my workflow](http://blog.jom.link/reproducible_analysis_my_principles.html):

- multiplaform (including macOS and Windows). Not the case when I tried it one or two year ago.
- as you show in your comment, it's seems simple to use

For the moment, I belive Docker is not a viable solution in my environment :

- In a recent [Titus Brown's blog post](http://ivory.idyll.org/blog/2017-pof-software-archivability.html) point out the lack of robustness and stability over time of Docker. Then I have some psychological difficulties to add it.
- I prefer to rely on a R package (*e.g.* packrat). The reason is pragmatic: on our Windows workstation in the hospital, we have to ask the administrator privileges to an hotline each time we need them, for example to install or update a software. Currently, R, Rstudio, Git seems to be view as a reliable sofware to our IT hotline then they don't ask anything special when I call them to update them. Note sure it will be the case for Docker.
- According to the installation docker's Window installation guide, Hyper-V is required. Hyper-V seems [available only since Windows 8 on server](https://en.wikipedia.org/wiki/Hyper-V#System_requirements) flavor... and our hospital's workstation are under Windows 7 (I don't have the choice). In comparison with virtual box that's could be installed on Windows 7, it's a dead end
- Peoples I works with don't come from a computer science education course. They are epidemiologists, biostatisticians, medical doctor, biologist or mathematicians. For good or bad, I feel they prefer to focus on they core activity and not on the tools. I'm not sure they will be OK to add another layer of complexity on they daily workflow. But if Docker adds enough benefits in regards to the complexity added, it should be used. Then

With these elements, for the moment I will not give another try do Docker. Th
