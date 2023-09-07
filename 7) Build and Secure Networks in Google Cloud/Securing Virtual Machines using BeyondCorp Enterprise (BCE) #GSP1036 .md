# GSP1036
## Run in cloudshell
```cmd
export USERNAME=
```
```cmd
gcloud services enable iap.googleapis.com
gcloud compute instances create linux-iap \
--machine-type e2-medium \
--subnet=default \
--no-address \
--zone us-east1-c
gcloud compute instances create windows-iap \
--machine-type e2-medium \
--subnet=default \
--no-address \
--zone us-east1-c \
--create-disk auto-delete=yes,boot=yes,device-name=windows-iap,image=projects/windows-cloud/global/images/windows-server-2016-dc-v20230315,mode=rw,size=50,type=projects/$DEVSHELL_PROJECT_ID/zones/us-east1-c/diskTypes/pd-balanced
gcloud compute instances create windows-connectivity \
--machine-type e2-medium \
--zone us-east1-c \
--create-disk auto-delete=yes,boot=yes,device-name=windows-connectivity,image=projects/qwiklabs-resources/global/images/iap-desktop-v001,mode=rw,size=50,type=projects/$DEVSHELL_PROJECT_ID/zones/us-east1-c/diskTypes/pd-balanced \
--scopes https://www.googleapis.com/auth/cloud-platform
gcloud compute firewall-rules create allow-ingress-from-iap \
--action ALLOW \
--network default \
--direction INGRESS \
--rules tcp:22,tcp:3389 \
--source-ranges 35.235.240.0/20
export WC_SERVICEACC=$(gcloud compute instances describe windows-connectivity \
--zone us-east1-c \
--format="json" | jq -r .serviceAccounts[0].email)
gcloud projects add-iam-policy-binding $DEVSHELL_PROJECT_ID \
--member='serviceAccount:'$WC_SERVICEACC \
--role="roles/iap.tunnelResourceAccessor"
gcloud iam service-accounts add-iam-policy-binding $WC_SERVICEACC \
--member 'user:'$USERNAME \
--role 'roles/iap.tunnelResourceAcessor'
gcloud compute resource0policies create-iap-policy linux-iap \
--action allow \
--member 'user:'$USERNAME
