FROM openjdk:8-jre-alpine
RUN apk update  && apk add \
    build-base \
    g++ \
    maven \
    apache-ant \
    cmake \
    make \
    git \
    && rm -rf /var/lib/apt/lists/*

RUN git clone https://github.com/opencv/opencv
WORKDIR /opencv/
RUN git checkout 3.4.6

RUN mkdir build
WORKDIR /opencv/build/
RUN cmake -D BUILD_SHARED_LIBS=OFF ..
RUN make -j8

RUN mvn install:install-file \ 
    -Dfile=./bin/opencv-346.jar \
    -DgroupId=org.opencv \
    -DartifactId=opencv \
    -Dversion=3.4.6 \
    -Dpackaging=jar

WORKDIR /
RUN git clone https://github.com/barais/ESIRTPDockerSampleApp
WORKDIR /ESIRTPDockerSampleApp
RUN sed -i 's/\"/home/barais/git/opencv/data/haarcascades/"\/\1"/buildneeds/opencv/data/haarcascades/"/' \
    src/main/java/main/Main.java

RUN mvn install
RUN mvn package

WORKDIR /
RUN mkdir -p buildneeds/opencv/lib
RUN mkdir -p buildneeds/opencv/data/haarcascades
RUN cp /opencv/build/lib/ buildneeds/opencv/lib/
RUN cp /ESIRTPDockerSampleApp/target/fatjar-0.0.1-SNAPSHOT.jar \
    buildneeds/fatjar-0.0.1-SNAPSHOT.jar
RUN cp /opencv/data/haarcascades/haarcascade_frontalface_default.xml \
    buildneeds/opencv/data/haarcascades/haarcascade_frontalface_default.xml

VOLUME [ "/buildneeds" ]

EXPOSE 8080

CMD [ "java", "-Djava.library.path=/opencv/build/lib/ \
    -jar /ESIRTPDockerSampleApp/target/fatjar-0.0.1-SNAPSHOT.jar" ]