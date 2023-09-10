# GSP080
>🚨 [PLEASE SUBSCRIBE OUR CHANNEL CLOUDHUSTLER](https://www.youtube.com/@cloudhustlers) **&** [JOIN OUR COMMUNITY](https://chat.whatsapp.com/KBfUcSleGGEFf2Xvvm8FW3)
## Run in cloudshell
```cmd
export REGION=
```
```cmd
gcloud config set compute/region $REGION
mkdir gcf_hello_world
cd gcf_hello_world
cat > index.js << EOF
exports.helloWorld = (data, context) => {
const pubSubMessage = data;
const name = pubSubMessage.data
    ? Buffer.from(pubSubMessage.data, 'base64').toString() : "Hello World";
console.log("My Cloud Function: "+name);
};
EOF
gsutil mb -p $DEVSHELL_PROJECT_ID gs://$DEVSHELL_PROJECT_ID
gcloud functions deploy helloWorld \
--stage-bucket $DEVSHELL_PROJECT_ID \
--trigger-topic hello_world \
--runtime nodejs20
```
