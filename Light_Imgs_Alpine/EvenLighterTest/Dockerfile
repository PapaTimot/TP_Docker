# Author : Timothée Schneider-Maunoury and Yoann Breton

# Take an empty parent image
FROM scratch


# Copy only the files strictly necessary to execute the webapp
# These files are copied directly from the light image (available on docker hub)

COPY --from=timmesir/mditpdocker:lightweightimg /fatjar-0.0.1-SNAPSHOT.jar /fatjar-0.0.1-SNAPSHOT.jar
COPY --from=timmesir/mditpdocker:lightweightimg /opencv /opencv
COPY --from=timmesir/mditpdocker:lightweightimg /usr/lib /usr/lib
COPY --from=timmesir/mditpdocker:lightweightimg /usr/bin /usr/bin
COPY --from=timmesir/mditpdocker:lightweightimg /lib /lib
COPY --from=timmesir/mditpdocker:lightweightimg /tmp /tmp

# Work also : COPY --from=timmesir/mditpdocker:lightweightimg / /


# Expose the port 8080 outside the container
EXPOSE 8080

# Run the webapp when we run this image
# by calling the command 'java -Djava.library.path=/opencv/lib/ -jar fatjar-0.0.1-SNAPSHOT.jar'
CMD [ "java", "-Djava.library.path=/opencv/lib/", "-jar", "/fatjar-0.0.1-SNAPSHOT.jar" ]