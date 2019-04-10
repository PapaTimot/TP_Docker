FROM picoded/ubuntu-openjdk-8-jdk
#ubuntu:latest
COPY . .
# apt-get update -qq && apt-get install -yqq \
  #  openjdk-8-jre \
  #  && 
RUN rm -rf /var/lib/apt/lists/* && mkdir -p /home/barais/git/opencv/data/haarcascades && curl -o /home/barais/git/opencv/data/haarcascades/haarcascade_frontalface_default.xml https://raw.githubusercontent.com/opencv/opencv/master/data/haarcascades/haarcascade_frontalface_default.xml
EXPOSE 8080
CMD [ "java", "-Djava.library.path=./opencv/lib/", "-jar", "./fatjar-0.0.1-SNAPSHOT.jar" ]