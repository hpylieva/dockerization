FROM openjdk:8

# Spark related variables.
ARG SPARK_VERSION=2.2.1
ARG SPARK_BINARY_ARCHIVE_NAME=spark-${SPARK_VERSION}-bin-hadoop2.7
ARG SPARK_BINARY_DOWNLOAD_URL=http://apache.volia.net/spark/spark-${SPARK_VERSION}/${SPARK_BINARY_ARCHIVE_NAME}.tgz


RUN apt-get update && \
	apt-get install scala -y && \
	wget -qO - ${SPARK_BINARY_DOWNLOAD_URL} | tar -xz -C /usr/local/ && \
	cd /usr/local/ && \
	ln -s ${SPARK_BINARY_ARCHIVE_NAME} spark && \
	cp spark/conf/log4j.properties.template spark/conf/log4j.properties && \
	sed -i -e s/WARN/ERROR/g spark/conf/log4j.properties && \
	sed -i -e s/INFO/ERROR/g spark/conf/log4j.properties

# We will be running our Spark jobs as `root` user.
USER root

# Working directory is set to the home folder of `root` user.
WORKDIR /root

COPY SimpleSparkProject.jar SimpleSparkProject.jar
COPY followers.txt followers.txt

CMD ["/usr/local/spark/bin/spark-submit", "--class", "com.ucu.scala.CountTrianglesWithUserDefinedFunctions", "--master", "local", "SimpleSparkProject.jar", "followers.txt"]
