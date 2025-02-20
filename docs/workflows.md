# Github Actions
We use Github-Actions to build our software. The costs are minimal because we use free and public repositories.

We designed templates to use github actions. The github action needs permission on the repository for example to create a release or push a tag. Here you can use the automatic token authentication from github. Therefore you can use the access token via ${{ secrets.GITHUB_TOKEN }}. In contrast of the .gitlab-ci.yml  you can create more workflow-files which are independent from each other. The it@M-Templates are arbitrary fit for your project. You can create reuseable actions for single steps.

The templates can be activated under  the tab “actions” with the button “New workflow” . In the software-catalog the templates can be find under the Category “By it@m”.


-	maven-node-build: this includes are “mvn install” for maven projects or a “npm run build” for node projects with a free selectable version of openjdk and nodejs. The right software-language is chosen if a pom.xml or package.json file is included in the folder. On top you specify your subfolders. After the source code is build a “docker build” is executed. Thereby a docker-image is pushed on the internal registry of github with the “tag” lagest. The “Dockerfile” needs to be available in the folder.
-	Maven-Release: This is a manual step. Therefore, you need to select in the tab “actions” on the left the workflow “maven-release” and start it over the button “Run workflow” (on top of the table). After that you can select on the pop-up-menu the version in the format x.y.z  and accordingly SNAPSHOT-X.Y.Z. The manual configuration with write rights is not necessary.
So that the maven-release is working, you need to referent the pom.xml as followed. The variables need to be filled with the actual value if you push your artifact to maven central.
```    
<scm>
        <connection>scm:git:${GITHUB_SERVER_URL}/${GITHUB_REPOSITORY}.git</connection>
        <developerConnection>scm:git:${GITHUB_SERVER_URL}/${GITHUB_REPOSITORY}.git</developerConnection>
      <tag>HEAD</tag>
  </scm>
```
-	Npm-Release: This is similar as Maven-Release. Only for node code. The action is triggert manual. Then you can select what version you want. A npm release is made and a docker image will be created.
-	Deploy docs github pages:  This action is to publish a documentation of vitepress as github pages.

