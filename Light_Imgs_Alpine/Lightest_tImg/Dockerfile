# Author : Timothée Schneider-Maunoury and Yoann Breton

# Take an empty parent image
FROM scratch


# Copy only the files strictly necessary to execute the webapp
# These files are copied directly from a local light image where useless libs have been removed

COPY --from=locallightimg /fatjar-0.0.1-SNAPSHOT.jar /fatjar-0.0.1-SNAPSHOT.jar
COPY --from=locallightimg /opencv /opencv
COPY --from=locallightimg /usr/lib /usr/lib
COPY --from=locallightimg /usr/bin /usr/bin
COPY --from=locallightimg /lib /lib
COPY --from=locallightimg /tmp /tmp

# Work also : COPY --from=timmesir/mditpdocker:lightweightimg / /


# Expose the port 8080 outside the container
EXPOSE 8080

# Run the webapp when we run this image
# by calling the command 'java -Djava.library.path=/opencv/lib/ -jar fatjar-0.0.1-SNAPSHOT.jar'
CMD [ "java", "-Djava.library.path=/opencv/lib/", "-jar", "/fatjar-0.0.1-SNAPSHOT.jar" ]