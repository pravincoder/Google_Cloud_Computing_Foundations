# GSP104
>🚨 [PLEASE SUBSCRIBE OUR CHANNEL CLOUDHUSTLER](https://www.youtube.com/@cloudhustlers) **&** [JOIN OUR COMMUNITY](https://chat.whatsapp.com/KBfUcSleGGEFf2Xvvm8FW3)
## Run in cloudshell
```cmd
export REGION=
```
```cmd
gcloud config set dataproc/region $REGION
gcloud dataproc clusters create example-cluster --worker-boot-disk-size 500
gcloud dataproc jobs submit spark \
--cluster example-cluster \
--class org.apache.spark.examples.SparkPi \
--jars file:///usr/lib/spark/examples/jars/spark-examples.jar \
-- 1000
```
