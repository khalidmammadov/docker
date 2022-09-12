#FROM ubuntu:20.04
FROM dongjoon/apache-spark-github-action-image:20220207

RUN apt-get update

RUN DEBIAN_FRONTEND=noninteractive TZ=Etc/UTC apt-get -y install tzdata

ENV MODULES_TO_TEST="pyspark-sql, pyspark-mllib, pyspark-resource"
# ENV HADOOP_PROFILE: ${{ inputs.hadoop }}
ENV HIVE_PROFILE=hive2.3
ENV GITHUB_PREV_SHA=blahblah
ENV SPARK_LOCAL_IP=localhost
ENV SKIP_UNIDOC=true
ENV SKIP_MIMA=true
ENV METASPACE_SIZE=1g

RUN apt-get -y install  openjdk-8-jdk

ADD spark.tgz /home/spark
RUN ls -la /home/spark

RUN python3.9 -m pip list
RUN pypy3 -m pip list

RUN curl -s https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh > miniconda.sh
RUN bash miniconda.sh -b -p "$HOME/miniconda"

WORKDIR /home/spark
RUN export PATH="$PATH:$HOME/miniconda/bin"
RUN python3.9 -m pip install pyarrow
RUN ./build/sbt clean package

# This is to be able to bind MasterWebUI to any container IP so we can access it from Host
# ENV SPARK_LOCAL_IP=0.0.0.0

# Master Web UI
EXPOSE 8080
# Spark Master URL port to submit Jobs: spark://3e0690a39526:7077
EXPOSE 7077 
# Worker Web UI
EXPOSE 8081-8090
# Rest API submit job
EXPOSE 6066

# Random Driver PORTS
EXPOSE 30000-39999

# Random Worker RPC Ports
EXPOSE 40000-49000


# APP Web UI
EXPOSE 4040


RUN apt install net-tools
