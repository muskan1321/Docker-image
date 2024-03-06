# Use Alpine Linux as the base image
FROM alpine:latest

# Install Git, OpenJDK 8, Maven, and Tomcat
RUN apk --update add git openjdk8 maven tomcat-native \
    && rm -rf /var/cache/apk/*

# Set environment variables
ENV JAVA_HOME /usr/lib/jvm/default-jvm
ENV CATALINA_HOME /usr/local/tomcat

# Update PATH to include Java, Maven, and Tomcat binaries
ENV PATH $PATH:$JAVA_HOME/bin:/usr/share/maven/bin:$CATALINA_HOME/bin

# Create a directory for Tomcat
RUN mkdir -p $CATALINA_HOME

# Install Tomcat (adjust the version if needed)
RUN wget -O /tmp/apache-tomcat.tar.gz http://archive.apache.org/dist/tomcat/tomcat-8/v8.5.75/bin/apache-tomcat-8.5.75.tar.gz \
    && tar xzf /tmp/apache-tomcat.tar.gz -C $CATALINA_HOME --strip-components=1 \
    && rm /tmp/apache-tomcat.tar.gz

# Display the installed versions
RUN git --version && java -version && mvn --version && $CATALINA_HOME/bin/catalina.sh version

# Expose the default Tomcat port
EXPOSE 8080

# Set the working directory
WORKDIR /app

# Start Tomcat by default
CMD ["catalina.sh", "run"]