#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#    http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#

ARG base_img

FROM $base_img
WORKDIR /

# Reset to root to run installation tasks
USER 0

RUN mkdir ${SPARK_HOME}/python
RUN apt-get update && \
    apt install -y wget build-essential zlib1g-dev libncurses5-dev libgdbm-dev libnss3-dev libssl-dev libreadline-dev libffi-dev libsqlite3-dev wget libbz2-dev && \
    wget https://www.python.org/ftp/python/3.8.0/Python-3.8.0.tgz && \
    tar -xf Python-3.8.0.tgz && \
    cd Python-3.8.0 && ./configure --enable-shared && \
    make && make altinstall && \
    mv ./python /usr/bin/python3.8 && \
    apt install -y  python3-pip && \
    python3.8 -m pip install pip && \
    pip3 install --upgrade pip setuptools && \
    cd ${SPARK_HOME}/jars && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-aws/3.3.2/hadoop-aws-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/com/amazonaws/aws-java-sdk-bundle/1.11.1026/aws-java-sdk-bundle-1.11.1026.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-common/3.3.2/hadoop-common-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-client-core/3.3.2/hadoop-mapreduce-client-core-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-yarn-client/3.3.2/hadoop-yarn-client-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-yarn-common/3.3.2/hadoop-yarn-common-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-hdfs-client/3.3.2/hadoop-hdfs-client-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/net/java/dev/jets3t/jets3t/0.9.4/jets3t-0.9.4.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-client/3.3.2/hadoop-client-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-yarn-api/3.3.2/hadoop-yarn-api-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-annotations/3.3.2/hadoop-annotations-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-client-jobclient/3.3.2/hadoop-mapreduce-client-jobclient-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-yarn-server-common/3.3.2/hadoop-yarn-server-common-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-client-shuffle/3.3.2/hadoop-mapreduce-client-shuffle-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-client-common/3.3.2/hadoop-mapreduce-client-common-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-mapreduce-client-app/3.3.2/hadoop-mapreduce-client-app-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-hdfs/3.3.2/hadoop-hdfs-3.3.2.jar && \
    wget https://repo1.maven.org/maven2/org/apache/hadoop/hadoop-auth/3.3.2/hadoop-auth-3.3.2.jar && \
    mkdir -p  /opt/spark/conf/ && \
    touch /opt/spark/conf/hadoop-metrics2-s3a-file-system.properties && \
    # Removed the .cache to save space
    rm -r /root/.cache && rm -rf /var/cache/apt/*

COPY python/pyspark ${SPARK_HOME}/python/pyspark
COPY python/lib ${SPARK_HOME}/python/lib

WORKDIR /opt/spark/work-dir
ENTRYPOINT [ "/opt/entrypoint.sh" ]

# Specify the User that the actual main process will run as
ARG spark_uid=185
USER ${spark_uid}
