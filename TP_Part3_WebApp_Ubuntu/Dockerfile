FROM openjdk:8-jre

# COPY todo

EXPOSE 8080
CMD [ "java", "-Djava.library.path=buildneeds/opencv/lib/", "-jar", "buildneeds/fatjar-0.0.1-SNAPSHOT.jar" ]