FROM ubuntu:latest

RUN apt-get update -qq && apt-get install -yqq \
    openjdk-8-jre \
    gcc \
    g++ \
    maven \
    ant \
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

RUN find -name *.jar


RUN mvn install:install-file \ 
    -Dfile=./bin/opencv-346.jar \
    -DgroupId=org.opencv \
    -DartifactId=opencv \
    -Dversion=3.4.6 \
    -Dpackaging=jar

WORKDIR /
RUN git clone https://github.com/barais/ESIRTPDockerSampleApp
WORKDIR /ESIRTPDockerSampleApp
RUN sed -i 's/\"/home/barais/git/opencv/data/haarcascades/"\/\1"/opencv/data/haarcascades/"/' \
    src/main/java/main/Main.java

RUN mvn install
RUN mvn package

EXPOSE 8080

CMD [ "java", "-Djava.library.path=/opencv/build/lib/ \
    -jar /ESIRTPDockerSampleApp/target/fatjar-0.0.1-SNAPSHOT.jar" ]