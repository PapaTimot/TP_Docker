# Author : Timothée Schneider-Maunoury and Yoann Breton

# Take a parent image with only a JRE (and not a whole JDK)
FROM openjdk:8-jre-alpine


# Copy only the files strictly necessary to execute the webapp
# These files are copied directly from the heavy build image (available on docker hub)
COPY --from=timmesir/mditpdocker:heavyweightimg \
     /ESIRTPDockerSampleApp/target/fatjar-0.0.1-SNAPSHOT.jar\ 
     fatjar-0.0.1-SNAPSHOT.jar
COPY --from=timmesir/mditpdocker:heavyweightimg \
    /opencv/data/haarcascades/haarcascade_frontalface_default.xml \
    /opencv/data/haarcascades/haarcascade_frontalface_default.xml
COPY --from=timmesir/mditpdocker:heavyweightimg /usr/lib/*.so* /usr/lib/
COPY --from=timmesir/mditpdocker:heavyweightimg /opencv/build/lib/libopencv_java346.so /opencv/lib/
COPY --from=timmesir/mditpdocker:heavyweightimg /opencv/build/lib/*core* /opencv/lib/
COPY --from=timmesir/mditpdocker:heavyweightimg /opencv/build/lib/*imgcodecs* /opencv/lib/
COPY --from=timmesir/mditpdocker:heavyweightimg /opencv/build/lib/*objdetect* /opencv/lib/

# Expose the port 8080 outside the container
EXPOSE 8080

# Run the webapp when we run this image
# by calling the command 'java -Djava.library.path=/opencv/lib/ -jar fatjar-0.0.1-SNAPSHOT.jar'
CMD [ "java", "-Djava.library.path=/opencv/lib/", "-jar", "/fatjar-0.0.1-SNAPSHOT.jar" ]