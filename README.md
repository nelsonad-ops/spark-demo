# spark-demo

test container:
docker run -it apache/spark /opt/spark/bin/spark-shell

binaries
wget https://dlcdn.apache.org/spark/spark-3.4.1/spark-3.4.1-bin-hadoop3.tgz
To setup cluster
$ kubectl cluster-info

$ ./bin/spark-submit \
    --master k8s://https://<k8s-apiserver-host>:<k8s-apiserver-port> \
    --deploy-mode cluster \
    --name spark-pi \
    --class org.apache.spark.examples.SparkPi \
    --conf spark.executor.instances=5 \
    --conf spark.kubernetes.container.image=apache/spark \
    local:///spark-3.4.1-bin-hadoop3/examples/jars/spark-examples_2.12-3.4.1.jar

s3 for data hosting
...
--packages org.apache.hadoop:hadoop-aws:3.2.2
--conf spark.kubernetes.file.upload.path=s3a://<s3-bucket>/path
--conf spark.hadoop.fs.s3a.access.key=...
--conf spark.hadoop.fs.s3a.impl=org.apache.hadoop.fs.s3a.S3AFileSystem
--conf spark.hadoop.fs.s3a.fast.upload=true
--conf spark.hadoop.fs.s3a.secret.key=....
--conf spark.driver.extraJavaOptions=-Divy.cache.dir=/tmp -Divy.home=/tmp
file:///full/path/to/app.jar

pod template
--conf spark.kubernetes.driver.podTemplateFile=s3a://bucket/driver.yml
--conf spark.kubernetes.executor.podTemplateFile=s3a://bucket/executor.yml

Volume Management
--conf spark.kubernetes.driver.volumes.[VolumeType].[VolumeName].mount.path=<mount path>
--conf spark.kubernetes.driver.volumes.[VolumeType].[VolumeName].mount.readOnly=<true|false>
--conf spark.kubernetes.driver.volumes.[VolumeType].[VolumeName].mount.subPath=<mount subPath>

status management
$ spark-submit --kill spark:spark-pi-1547948636094-driver --master k8s://https://192.168.2.8:8443
$ spark-submit --status spark:spark-pi-1547948636094-driver --master  k8s://https://192.168.2.8:8443
$ spark-submit --kill spark:spark-pi* --master  k8s://https://192.168.2.8:8443

benchmark TPC-DS
http://www.openkb.info/2021/02/how-to-generate-tpc-ds-data-and-run-tpc.html





