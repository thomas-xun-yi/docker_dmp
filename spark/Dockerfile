FROM gudaoxuri/scala_2.11:latest AS scala_env
MAINTAINER gudaoxuri <i@sunisle.org>

FROM gudaoxuri/hdfs:latest

COPY --from=scala_env /root/.bashrc /root/.bashrc
COPY --from=scala_env /opt/env/scala /opt/env/scala

RUN echo "export SCALA_HOME=/opt/env/scala" >> /etc/profile \
 && echo "export PATH=/opt/env/scala/bin:$PATH" >> /etc/profile

# Setup env
ENV SPARK_HOME /opt/spark
ENV PATH $PATH:$SPARK_HOME/sbin:$SPARK_HOME/bin
ENV KEEP false

RUN echo "export SPARK_HOME=/opt/spark" >> /etc/profile \
 && echo "export PATH=$SPARK_HOME/sbin:$SPARK_HOME/bin:$PATH" >> /etc/profile

# Download spark
RUN wget -q -O - http://mirrors.hust.edu.cn/apache/spark/spark-2.2.1/spark-2.2.1-bin-hadoop2.7.tgz | tar -xzf - -C /opt/ \
 && mv /opt/spark-2.2.1-bin-hadoop2.7 /opt/spark

COPY spark-env.sh $SPARK_HOME/conf/
COPY bootstrap_spark.sh /bin/bootstrap_spark.sh
COPY bootstrap.sh /bin/bootstrap.sh

RUN chmod +x /bin/bootstrap*

EXPOSE 4040 7077 18080 18081

ENTRYPOINT /bin/bootstrap.sh $KEEP
