FROM openjdk:latest

ENV ZEPPELIN_VERSION=0.8.0 \
    ZEPPELIN_HOME=/opt/zeppelin \
    MONGO_VERSION=3.6

RUN mkdir -p ${ZEPPELIN_HOME} && \
    curl -SL http://mirror.easyname.ch/apache/zeppelin/zeppelin-${ZEPPELIN_VERSION}/zeppelin-${ZEPPELIN_VERSION}-bin-all.tgz \
      | tar xz -C ${ZEPPELIN_HOME} && \
    mv ${ZEPPELIN_HOME}/zeppelin-${ZEPPELIN_VERSION}-bin-all/* ${ZEPPELIN_HOME} && \
    rm -rf ${ZEPPELIN_HOME}/zeppelin-${ZEPPELIN_VERSION}-bin-all && \
    rm -rf *.tgz && \
    mkdir -p ${ZEPPELIN_HOME}/interpreter/mongodb && \
    curl -SL -o ${ZEPPELIN_HOME}/interpreter/mongodb/zeppelin-mongodb-${ZEPPELIN_VERSION}.jar \
      https://github.com/bbonnin/zeppelin-mongodb-interpreter/releases/download/${ZEPPELIN_VERSION}/zeppelin-mongodb-${ZEPPELIN_VERSION}.zip

COPY run.sh /${ZEPPELIN_HOME}/run.sh
RUN chmod +x /${ZEPPELIN_HOME}/run.sh 

# Install Mongo Shell
RUN  apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 0C49F3730359A14518585931BC711F9BA15703C6 && \
     echo "deb http://repo.mongodb.org/apt/debian jessie/mongodb-org/${MONGO_VERSION} main" > /etc/apt/sources.list.d/mongodb-org.list && \
     apt-get update && \
     apt-get install -y mongodb-org-shell
 
# Install Anaconda3
RUN  cd /tmp/ && \
     wget https://repo.anaconda.com/archive/Anaconda3-2019.07-Linux-x86_64.sh -O ~/anaconda.sh && \
     /bin/bash ~/anaconda.sh -b -p /opt/conda && \
     rm ~/anaconda.sh && \
     ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
     echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
     chmod +x /opt/conda/etc/profile.d/conda.sh && \ 
     echo "conda activate base" >> ~/.bashrc && \
     echo "conda install numpy pandas tensorflow bokeh curl keras matplotlib pip pymongo pyspark scikit-learn scipy seaborn" >> ~/.bashrc
	
# Install Spark
RUN  cd /opt/ && \
     curl -O "https://archive.apache.org/dist/spark/spark-2.0.0/spark-2.0.0-bin-hadoop2.7.tgz" && \
     tar -xzvf spark-2.0.0-bin-hadoop2.7.tgz && \
     cd /opt/Spark/spark-2.0.0-bin-hadoop2.7/jars && \
     curl -O "https://repo1.maven.org/maven2/org/mongodb/mongo-java-driver/3.8.0/mongo-java-driver-3.8.0.jar"  && \
     curl -O "http://repo1.maven.org/maven2/org/mongodb/spark/mongo-spark-connector_2.11/2.0.0/mongo-spark-connector_2.11-2.0.0.jar" && \
     export SPARK_HOME =/opt/Spark/spark-2.0.0-bin-hadoop2.7

# Install Python 2.7.9
RUN  cd /opt/ && \
     mkdir python2 && \
     curl -O "https://www.python.org/ftp/python/2.7.9/Python-2.7.9.tgz" && \
     tar -xzvf Python-2.7.9.tgz && \
     export PYTHONPATH =/opt/python2/Python-2.7.9/python
     
# Install Java 8
RUN  cd /opt/ && \
     mkdir java && \
     curl -O "https://download.oracle.com/otn/java/jdk/8u131-b11/d54c1d3a095b4ff2b6607d096fa80163/jdk-8u131-linux-x64.tar.gz" && \
     tar -xzvf jdk-8u131-linux-x64.tar.gz && \
     export JAVA_HOME=/opt/java/jdk1.8.0_131
	
EXPOSE 8080

VOLUME ${ZEPPELIN_HOME}/logs ${ZEPPELIN_HOME}/notebook

CMD ${ZEPPELIN_HOME}/run.sh
