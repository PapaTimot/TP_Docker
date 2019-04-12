FROM openjdk:8-alpine

RUN apk update  && apk add       \
    linux-headers build-base g++ \
    maven apache-ant cmake make  \
    git alpine-sdk wget gtk+2.0  \
    glib-dev pkgconfig pkgconf   \
    ffmpeg-dev ffmpeg-libs       \
    && rm -rf /var/lib/apt/lists/*

RUN git clone --progress https://github.com/opencv/opencv
WORKDIR /opencv/
RUN git checkout 3.4.6

RUN mkdir build
WORKDIR /opencv/build/

RUN echo $JAVA_HOME && \
    cmake -D BUILD_SHARED_LIBS=OFF ..
RUN make -j8
RUN mvn install:install-file \ 
    -Dfile=/opencv/build/bin/opencv-346.jar \
    -DgroupId=org.opencv \
    -DartifactId=opencv \
    -Dversion=3.4.6 \
    -Dpackaging=jar

WORKDIR /
RUN git clone https://github.com/barais/ESIRTPDockerSampleApp
WORKDIR /ESIRTPDockerSampleApp
RUN sed -i "s/\/home\/barais\/git\/opencv\/data\/haarcascades\//\/opencv\/data\/haarcascades\//g" \
    src/main/java/main/Main.java

RUN mvn install
RUN mvn package

RUN find . -name *libopencv_java346.so

EXPOSE 8080

CMD [ "java", "-Djava.library.path=/opencv/build/lib/", "-jar", "/ESIRTPDockerSampleApp/target/fatjar-0.0.1-SNAPSHOT.jar" ]