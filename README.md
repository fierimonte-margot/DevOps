[Devops tp1]{.c20}

[]{.c7 .c29}

[Database :]{.c17} {#h.kqh3radjl2gi .c24}
------------------

### [Basics]{.c16} {#h.7z8zhhjbjahh .c19}

[docker build -t margot/tp1\_database .]{.c3}

[]{.c3}

[Why do we need a volume to be attached to our postgres container?]{.c3}

[→ to save the data into.]{.c3}

[]{.c4}

[1-1 ) Document your database container essentials: commands and
Dockerfile.]{.c31}

-   ### [Init database]{.c15} {#h.6h4gnzdo2ph8 style="display:inline"}

[]{.c3}

[docker network create app-network]{.c3}

[]{.c3}

[docker run  -d \--network app-network  -p  8080:8080 adminer]{.c3}

[]{.c3}

[docker run \--network app-network \--name database -e POSTGRES\_DB=db
-e POSTGRES\_USER=usr -e POSTGRES\_PASSWORD=pwd -d postgres]{.c3}

-   ### [Persist data]{.c15} {#h.4pyivnvc8lh style="display:inline"}

[]{.c3}

[docker run \--network app-network \--name database -e POSTGRES\_DB=db
-e POSTGRES\_USER=usr -e POSTGRES\_PASSWORD=pwd -v
/home/tp/Desktop/tp1:/var/lib/postgresql/data -d postgres]{.c3}

 

[Backend API :]{.c17} {#h.uxfg2s1tz6vn .c24}
---------------------

### [Basics:]{.c16} {#h.dsic6wpdej7f .c19}

Dockerfile

------------------------------------------------------------------------

 

[FROM openjdk:11]{.c3}

[COPY Main.java /usr/src/app/]{.c3}

[CMD \[\"java\", \"/usr/src/app/Main.java\"\]]{.c3}

------------------------------------------------------------------------

 

[docker image build -t docker-java-jar:latest .]{.c3}

[]{.c3}

[docker run docker-java-jar:latest]{.c3}

[]{.c3}

### [Multistage build]{.c16} {#h.otje0zgar9gq .c19}

-   #### [Backend simple api ]{.c15} {#h.ur3d0it8ozb style="display:inline"}

[]{.c3}

[1-2 Why do we need a multistage build? And explain each step of this
dockerfile.]{.c6}

[]{.c4}

[To reduce size of a project ]{.c3}

[]{.c4}

[step 1 :]{.c7} [add controller folder and Greetingcontroller file to
the project.]{.c3}

[]{.c3}

[step 2 :]{.c7} [add a Dockerfile into the source project at the
beginning.]{.c3}

[]{.c3}

[step 3 :]{.c7}[ move into folder simpleapi (cd)and  build docker
image]{.c3}

[]{.c3}

[docker image build -t simple-api .]{.c3}

[]{.c3}

[step 4 :]{.c7}[ run the docker]{.c3}

[]{.c3}

[docker run -d \--name java-container -p 8081:8080  simple-api]{.c3}

[]{.c3}

[step 5 :]{.c7}[ open navigator and watch if it's works]{.c3}

[]{.c3}

[[http://localhost:8081/](https://www.google.com/url?q=http://localhost:8081/&sa=D&source=editors&ust=1675260778410832&usg=AOvVaw3SmzlZriYP_zi-jwFWBm2V){.c26}]{.c7
.c25}

-   #### [Backend API]{.c15} {#h.nymzzcrlc2lt style="display:inline"}

[]{.c4}

------------------------------------------------------------------------

[]{.c3}

[application.yml ]{.c4}

------------------------------------------------------------------------

[]{.c3}

[spring:]{.c3}

[  jpa:]{.c3}

[        properties:]{.c3}

[          hibernate:]{.c3}

[            jdbc:]{.c3}

[              lob:]{.c3}

[                non\_contextual\_creation: true]{.c3}

[        generate-ddl: false]{.c3}

[        open-in-view: true]{.c3}

[  datasource:]{.c3}

        [url: jdbc:postgresql://database:5432/db]{.c11}

[        username: usr]{.c11}

[        password: pwd]{.c11}

[        driver-class-name: org.postgresql.Driver]{.c3}

[management:]{.c3}

[ server:]{.c3}

[   add-application-context-header: false]{.c3}

[ endpoints:]{.c3}

[   web:]{.c3}

[         exposure:]{.c3}

[           include: health,info,env,metrics,beans,configprops]{.c3}

------------------------------------------------------------------------

[]{.c3}

[]{.c3}

[docker image build -t simple-api-student-main .]{.c3}

[]{.c4}

[docker run \--network app-network  \--name java-container -p 8081:8080
simple-api-student-main]{.c3}

[]{.c4}

[]{.c4}

[Http server]{.c17} {#h.8k8c76pzb9gw .c24}
-------------------

### [Basics]{.c16} {#h.8tl1k2lerjlh .c19}

#### [Choose an appropriate base image.]{.c5} {#h.r78o8glm6c04 .c14}

------------------------------------------------------------------------

[]{.c3}

[Dockerfile]{.c4}

------------------------------------------------------------------------

[]{.c3}

[FROM httpd:2.4]{.c3}

COPY index.html /usr/local/apache2/htdocs/

------------------------------------------------------------------------

[]{.c4}

[]{.c4}

------------------------------------------------------------------------

[]{.c3}

[index.html]{.c4}

------------------------------------------------------------------------

[]{.c4}

[ \<!DOCTYPE html\>]{.c3}

[\<html\>]{.c3}

[\<body\>]{.c3}

[]{.c3}

[\<h1\>My First Heading\</h1\>]{.c3}

[\<p\>My first paragraph.\</p\>]{.c3}

[]{.c3}

[\</body\>]{.c3}

\</html\>

------------------------------------------------------------------------

[]{.c3}

[docker build -t index .]{.c3}

[]{.c3}

[docker run -dit  \--network app-network  \--name my-running-app -p
8082:80 index]{.c3}

#### [Configuration]{.c5} {#h.2vypcxsuery3 .c14}

[]{.c3}

[docker exec my-running-app  cat /usr/local/apache2/conf/httpd.conf \>
index-httpd.conf]{.c3}

[]{.c3}

#### [Reverse proxy]{.c5} {#h.jbctvfc2ytez .c14}

[uncomment module lines and change url proxy.]{.c3}

------------------------------------------------------------------------

[]{.c3}

[index-httpd.conf]{.c4}

------------------------------------------------------------------------

[]{.c3}

[]{.c3}

[LoadModule proxy\_module modules/mod\_proxy.so]{.c8}

[LoadModule proxy\_http\_module modules/mod\_proxy\_http.so]{.c8}

[]{.c3}

[]{.c3}

[\<VirtualHost \*:80\>]{.c3}

[ProxyPreserveHost On]{.c3}

[ProxyPass / http://java-container:8080/]{.c8}

[ProxyPassReverse / http://java-container:8080/]{.c8}

\</VirtualHost\>

------------------------------------------------------------------------

[]{.c3}

### [Link application]{.c16} {#h.w99h3dszdx79 .c19}

#### [Docker-compose]{.c5} {#h.34k9o26nkrxk .c14}

[]{.c3}

[1-3 Document docker-compose most important commands 1-4 Document your
docker-compose file.]{.c6}

[]{.c3}

Publish {#h.6fgty42jsy9z .c24}
-------

[]{.c3}

[1-5 Document your publication commands and published images in
dockerhub.]{.c6}

[]{.c6}

[docker tag simple-api  margotfierimonte/simple-api:1.0]{.c3}

[docker push margotfierimonte/simple-api:1.0]{.c3}

[]{.c3}

[docker tag adminer  margotfierimonte/adminer:1.0]{.c3}

[docker push margotfierimonte/adminer:1.0]{.c3}

[]{.c3}

[docker tag f29f2ffbae59  margotfierimonte/f29f2ffbae59:1.0]{.c3}

[docker push margotfierimonte/f29f2ffbae59]{.c3}

[]{.c3}

------------------------------------------------------------------------

[]{.c3}

[Devops tp2]{.c20}

[]{.c3}

[IMPORTANT : The docker images used for the tp2 are the one of
correction.]{.c8}

[]{.c3}

[2-1 What are testcontainers?]{.c6}

[]{.c3}

Testcontainers are [Java libraries that allow us to run a bunch of
docker containers while testing]{.c3}

[]{.c3}

[2-2 Document your Github Actions configurations.]{.c6}

[]{.c3}

[Création of .git/workflow folder.]{.c3}

[]{.c3}

[add main.yml]{.c3}

[]{.c3}

[choose the branch : main]{.c3}

[]{.c3}

[add the jdk ]{.c3}

[]{.c3}

[add secret variable :]{.c3}

-   [DOCKERHUB\_USERNAME]{.c3}
-   [DOCKERHUB\_TOKEN]{.c3}

[]{.c3}

[add the command to  build and test the app : ]{.c3}

-   [cd ./devops-resources-main/solution/01-docker/simple-api/ && mvn -B
    verify]{.c3}

[]{.c3}

[add the command to log into docker :]{.c3}

-   [docker login -u \${{ secrets.DOCKERHUB\_USERNAME }} -p
    \${{secrets.DOCKERHUB\_TOKEN }}]{.c3}

[]{.c3}

[and finally add the part for build and push 3 images]{.c3}

[]{.c3}

[]{.c3}

[2-3 Document your quality gate configuration.]{.c6}

[]{.c3}

[add secret variable :]{.c3}

-   [SONAR\_TOKEN]{.c3}

[]{.c3}

[create a token with sonarCloud and get the key project]{.c3}

[]{.c3}

[add in the command to build end test the app a part for the sonarCloud
token with the keys project : ]{.c3}

-   sonar:sonar -Dsonar.projectKey=githubaction\_devops
    -Dsonar.organization=githubaction
    -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=\${{
    secrets.SONAR\_TOKEN }} \--file ./pom.xml
