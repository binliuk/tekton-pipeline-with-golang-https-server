
Create Tekon pipeline with a simple goland https server in Openshift 4.x

---

####  prerequirements — `tekton operater and command line`

```Tekton operator and cli
 see ref:  https://openshift.github.io/pipelines-docs/docs/0.10.5/assembly_installing-pipelines.html 
```

#### Create project in ocp4 - `go-https-pipeline`

```sh 
#New project:
oc new-project go-https-pipeline

#Check tekon Service account:

oc get sa pipeline

```
#### Tekton resources — `image and git`

```Tekton and oc
oc create -f https://raw.githubusercontent.com/binliuk/golang-tls/master/pipeline/resource.yaml
tkn resource ls

```

#### Tekton tasks — `deploy and customization`

```Tekton
oc create -f https://raw.githubusercontent.com/binliuk/golang-tls/master/pipeline/apply_manifest_task.yaml
oc create -f https://raw.githubusercontent.com/binliuk/golang-tls/master/pipeline/update_deployment_task.yaml

tkn task ls

```
#### Tekton pipeline — `build and deploy`

```Tekton
oc create -f https://raw.githubusercontent.com/binliuk/golang-tls/master/pipeline/pipeline.yaml
tkn pipeline ls 

```
#### Trigger pipeline from cli
```bash
#trigger
tkn pipeline start build-and-deploy
#track logs
tkn pipelinerun logs <pipelinerun ID> -f -n pipelines-tutorial
#retrigger pipeline
tkn pipeline start build-and-deploy --last

```

##### Generate private key (.key)

```sh
# Key considerations for algorithm "RSA" ≥ 2048-bit
openssl genrsa -out server.key 2048

# Key considerations for algorithm "ECDSA" (X25519 || ≥ secp384r1)
# https://safecurves.cr.yp.to/
# List ECDSA the supported curves (openssl ecparam -list_curves)
openssl ecparam -genkey -name secp384r1 -out server.key
```

##### Generation of self-signed(x509) public key (PEM-encodings `.pem`|`.crt`) based on the private (`.key`)

```sh
openssl req -new -x509 -sha256 -key server.key -out server.crt -days 3650
```

Reference Link
---
* https://getgophish.com/blog/post/2018-12-02-building-web-servers-in-go/
* https://github.com/openshift-pipelines
* https://github.com/openshift/pipelines-tutorial
* https://openshift.github.io/pipelines-docs/docs/0.10.5/assembly_installing-pipelines.html
* https://github.com/denji/golang-tls
* …
