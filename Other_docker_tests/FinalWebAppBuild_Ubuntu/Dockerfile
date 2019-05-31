FROM ubuntu:latest

RUN apt-get update -qq && apt-get install -yqq \
    openjdk-8-jdk \
    gcc \
    g++ \
    maven \
    ant \
    cmake \
    make \
    git \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update -qq && apt-get install -yqq     \
    build-essential libgtk2.0-dev pkg-config       \
    libavcodec-dev libavformat-dev libswscale-dev  \
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
    -Dfile=./bin/opencv-346.jar \
    -DgroupId=org.opencv \
    -DartifactId=opencv \
    -Dversion=3.4.6 \
    -Dpackaging=jar

WORKDIR /
RUN git clone --progress https://github.com/barais/ESIRTPDockerSampleApp
WORKDIR /ESIRTPDockerSampleApp
RUN sed -i "s/\/home\/barais\/git\/opencv\/data\/haarcascades\//\/opencv\/data\/haarcascades\//g" \
    src/main/java/main/Main.java

RUN mvn install
RUN mvn package

EXPOSE 8080

CMD [ "java", "-Djava.library.path=/opencv/build/lib/ \
    -jar /ESIRTPDockerSampleApp/target/fatjar-0.0.1-SNAPSHOT.jar" ]

# java -Djava.library.path=/opencv/build/lib/ -jar /ESIRTPDockerSampleApp/target/fatjar-0.0.1-SNAPSHOT.jar