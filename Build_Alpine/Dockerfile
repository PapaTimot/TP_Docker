# Author : Timothée Schneider-Maunoury and Yoann Breton

# Take the lightest jdk parent image available on docker hub
FROM openjdk:8-alpine

# Download all the necessary packages to compile opencv and the webapp
RUN apk update  && apk add       \
    linux-headers build-base g++ \
    maven apache-ant cmake make  \
    git alpine-sdk wget gtk+2.0  \
    glib-dev pkgconfig pkgconf   \
    ffmpeg-dev ffmpeg-libs       \
    && rm -rf /var/lib/apt/lists/*

# Clone the opencv sources and go to the version 3.4.6
RUN git clone --progress https://github.com/opencv/opencv
WORKDIR /opencv/
RUN git checkout 3.4.6

RUN mkdir build
WORKDIR /opencv/build/

# Build all the libraries of opencv
RUN echo $JAVA_HOME && \
    cmake -D BUILD_SHARED_LIBS=OFF ..
RUN make -j8

# Install the opencv libraries in maven
RUN mvn install:install-file \ 
    -Dfile=/opencv/build/bin/opencv-346.jar \
    -DgroupId=org.opencv \
    -DartifactId=opencv \
    -Dversion=3.4.6 \
    -Dpackaging=jar

# Clone the sources of the webapp and replace a bad path in the Main.java file
WORKDIR /
RUN git clone https://github.com/barais/ESIRTPDockerSampleApp
WORKDIR /ESIRTPDockerSampleApp
RUN sed -i "s/\/home\/barais\/git\/opencv\/data\/haarcascades\//\/opencv\/data\/haarcascades\//g" \
    src/main/java/main/Main.java

# Install with maven all the libraries needed for the project
# and build the project (especially the fatjar-0.0.1-SNAPSHOT.jar file)
RUN mvn install
RUN mvn package

# Expose the port 8080 outside the container
EXPOSE 8080

# Run the webapp when we run this image
CMD [ "java", "-Djava.library.path=/opencv/build/lib/", "-jar", "/ESIRTPDockerSampleApp/target/fatjar-0.0.1-SNAPSHOT.jar" ]