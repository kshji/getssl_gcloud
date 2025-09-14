# getssl using Google Cloud DNS

https://github.com/kshji/getssl_gcloud

ver 2025-09-13

## You need install gcloud command

[Gcloud install](gle.com/sdk/docs/install)

If you are using *nix server without desktop (x-term), remove DISPLAY set if it's.
If you have set DISPLAY, try init process to start local chrome GUI process.
``` sh
unset DISPLAY
```

Init gcloud using, need to do once.

``` sh
gcloud init
```

Need also Google Cloud Console Service Account:
[Google Cloud Console Service Account](https://console.cloud.google.com/projectselector2/iam-admin/serviceaccounts )

* Permissions role "DNS Zone Editor"
* result is JSON keyfile

Service Account  account is email, usually something like  xxx@xxx.gserviceaccount.com

***Example*** using keyfile:

``` sh
gcloud auth activate-service-account xxx@xxx.gserviceaccount.com --key-file=/somepath/PROJECT_ID_xxxxx.json
```

After auth you can use gcloud dns services.

***Example***: Get zones from your Google Cloud DNS project:

``` sh
gcloud dns managed-zones list --project=PROJECT_ID
``` 

More gcloud dns commands: [Reference DNS](https://google.com/sdk/gcloud/reference/dns)

***Example***: List domain TXT records:

``` sh
gcloud dns record-sets list --zone=ZONEID --name="example.com." --type="TXT" 
```

ZONEID is usually ex. for domain example.com it is ***examplecom***

## getssl configuration DNS validation using Google Cloud DNS

example.com/getssl.cfg

``` sh
CA="https://acme-v02.api.letsencrypt.org"
SANS="*.example.com"

#Set this to "true" to enable DNS validation
VALIDATE_VIA_DNS="true"             
# Google Cloud DNS setup
# Use this command/script to add the challenge token to the DN#S entries for the domain
DNS_ADD_COMMAND="/somepath/dns_add_gcloud"              
# Use this command/script to remove the challenge token from the DNS entries for the domain
DNS_DEL_COMMAND="/somepath/dns_del_gcloud"              `

# example.com setup Google Cloud DNS validation
export GCLOUD_ZONE="examplecom"          # Google Cloud DNS zoneid
export GCLOUD_PROJECTID="mydnsproject"   # Google Cloud projectid
# Google Cloud Service Account
export GCLOUD_ACCOUNT="someuser@mydnsproject.iam.gserviceaccount.com"  
export GCLOUD_KEYFILE="/somepath/mydnsprojectSomeid.json"

```

