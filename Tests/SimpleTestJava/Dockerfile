# Use an official Java runtime as a parent image
FROM openjdk:7

# Copy the current directory contents into the container at .
COPY . .

# Compile the java main class
RUN javac Main.java

# Run Main when the container launches
CMD ["java", "Main"]
