# Use Ubuntu as the base image
FROM ubuntu:latest

# Set non-interactive mode for apt-get
#ENV DEBIAN_FRONTEND=noninteractive

# Install required dependencies
RUN apt-get update && apt-get install -y \
    wget \
    curl \
    tar \
    unzip \
    && rm -rf /var/lib/apt/lists/*
RUN apt-get install bash

# Download and install OpenJDK 17
RUN mkdir -p /opt/java && \
    wget -qO- https://builds.openlogic.com/downloadJDK/openlogic-openjdk/11.0.25+9/openlogic-openjdk-11.0.25+9-linux-x64.tar.gz | tar -xz -C /opt/java

# Renaming openjdk folder under /opt/java
RUN cd /opt/java && \
    mv openlogic-openjdk-11.0.25+9-linux-x64 jdk-11.0_25

# Set up JAVA_HOME and PATH for OpenJDK
ENV JAVA_HOME=/opt/java/jdk-11.0_25
ENV PATH="${JAVA_HOME}/bin:${PATH}"


#Setup java home and path
#RUN echo "export JAVA_HOME=/opt/java/jdk-11.0_25 >> ~/.bashrc" && \
#    echo "export PATH=/opt/java/jdk-11.0_25/bin:$PATH >> ~/.bashrc" 
#Reloading configuration

#RUN source ~/.bashrc

# Install Maven
RUN mkdir -p /opt/maven && \
    wget -qO- https://dlcdn.apache.org/maven/maven-3/3.9.9/binaries/apache-maven-3.9.9-bin.tar.gz | tar -xz -C /opt/maven

# Set up M2_HOME and PATH for Maven
ENV M2_HOME=/opt/maven/apache-maven-3.9.9
ENV PATH="${M2_HOME}/bin:${PATH}"

#Setup java home and path
#RUN echo "export M2_HOME=/opt/maven/apache-maven-3.9.9 >> ~/.bashrc" && \
#    echo "export PATH=/opt/maven/apache-maven-3.9.9/bin:$PATH >> ~/.bashrc"
#Reloading configuration

#RUN source ~/.bashrc


# Verify installations
#RUN source /root/.bashrc
#RUN java -version && mvn -version

# Set the default command to bash
CMD ["/bin/bash"]

