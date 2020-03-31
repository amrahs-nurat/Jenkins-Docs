##### Deploy war file in Tomcat server using Jenkins CI/CD.

I. Configure Maven in Jenkins:
- Go to Jenkins Dashboard ->Manage Jenkins ->Manage plugins ->Available ->Maven Integration ->Install
- Go to Manage Jenkins->Global tool configuration->Maven -> Add Maven_home variable value (i.e. path of the maven file on your system).
- Go to Jenkins Dashboard -> New Item -> Maven Project option will be available.


II. Build a Maven project (For continuous Integration)
   >  Go to Jenkins Dashboard -> New Item -> Choose name for the Maven Project    (e.g. MyFirstMavenExample)
1. Copy the git url and provide that url in scm -> git -> repository: use the git url. no need credentials in such case as this is public repository.
2. Build -> use pom.xml in Root POM  
3. Goal and Options -> clean install Package 
4. Apply and save it. Build it.
Hence continue integration has completed using above menthod.

III. For continuous deployment Steps:
- To deploy war file we have to create a tomcat server. (In my case i have created a different server, node2 having tomcat configuratin)
- Install **Deploy to container** plugin in jenkins, only after that plugin installtion we will get the deploy option in post-build Actions.
- Add credentials for tomcat manager-script 
    click credentials -> global -> add credentials -> choose kind: username and password section
- Choose exiting project,
    click on configuration -> go to post-build Actions
    given war file details, and choose container
    Provide the credential and Tomcat url: http://server_ip:8081
- Apply and save it. Build it then.
[ Note: ] With the help of Steps II and III we deploy war file manually.

IV. Now we are going to deploy file Automatically using Poll SCM.
1. provide schedule:  */2 * * * * 
   Now make change in file stored in git. and test it.
