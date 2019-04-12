FROM openjdk:8-jre-alpine


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


EXPOSE 8080
CMD [ "java", "-Djava.library.path=/opencv/lib/", "-jar", "/fatjar-0.0.1-SNAPSHOT.jar" ]
# java -Djava.library.path=/opencv/lib/ -jar fatjar-0.0.1-SNAPSHOT.jar
