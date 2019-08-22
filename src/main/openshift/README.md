# Axonserver on OpenShift
## Deployment examples
1. Create the axonserver template
  - `oc create -f axonserver.yaml`
2. Deploy an axonserver
  - `oc new-app --template=axonserver`
3. Build the axonserver-openshift image
  - `oc start-build axonserver-image`
4. Start a new deployment
  - `oc rollout latest axonserver`
5. Alter application parameters by setting BuildConfig environment variables
  - `oc set env dc/axonserver AXONSERVER_TOKEN='yoursupersecrettoken'`

## Configuration parameters
You can configure axonserver using the following DeploymentConfig environment variables:
- AXONSERVER_NAME
- AXONSERVER_HOSTNAME
- AXONSERVER_DOMAIN
- AXONSERVER_HTTP_PORT
- AXONSERVER_GRPC_PORT
- AXONSERVER_TOKEN
- AXONSERVER_EVENTSTORE
- AXONSERVER_LOGSTORE
- AXONSERVER_CONTROLDB

More detailed information is available in the [startup.sh](https://github.com/cegeka/axon-server-openshift/blob/master/src/main/docker/startup.sh) script.
