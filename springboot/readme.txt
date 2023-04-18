For complete reference  - https://spring.io/blog/2020/01/27/creating-docker-images-with-spring-boot-2-3-0-m1
To run the sample

1. Download demo 
    > curl https://start.spring.io/starter.zip -d bootVersion=2.3.0.M1 -d dependencies=web -o demo.zip

2. Unzip 
    > unzip demo.zip

3. Compile and build images (docker must be running)
   > mvn spring-boot:build-image

4. Run the app
    > docker run -it -p8080:8080 demo:0.0.1-SNAPSHOT


-----------------
With Layered Jar
-----------------
1. Added Layered Jar 

2. rebuild 
	> mvn clean package
3. Run
	> java -Djarmode=layertools -jar target/demo-0.0.1-SNAPSHOT.jar list

3. Create docker file 

FROM adoptopenjdk:11-jre-hotspot as builder
WORKDIR application
ARG JAR_FILE=target/*.jar
COPY ${JAR_FILE} application.jar
RUN java -Djarmode=layertools -jar application.jar extract

FROM adoptopenjdk:11-jre-hotspot
WORKDIR application
COPY --from=builder application/dependencies/ ./
COPY --from=builder application/snapshot-dependencies/ ./
COPY --from=builder application/resources/ ./
COPY --from=builder application/application/ ./
ENTRYPOINT ["java", "org.springframework.boot.loader.JarLauncher"]

4. Build
	> docker build . --tag demo
	
5. run
	> docker run -it -p8080:8080 demo:latest

