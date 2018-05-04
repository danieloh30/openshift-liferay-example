# openshift-liferay-example

## Build from scratch

```console
$ export APP_NAME=wildfly12-liferay7

$ oc new-project liferay-example

$ oc new-app mysql-persistent -p DATABASE_SERVICE_NAME=mysql-lportal -p MYSQL_DATABASE=lportal

$ # export LF_REPO_PREFIX=http://httpd-ex-liferay-distr.apps.172.20.10.5.xip.io/liferay/
$ oc new-app \
    openshift/wildfly:12.0~https://github.com/dsevost/openshift-liferay-example \
    --context-dir=/src \
    --name=$APP_NAME \
    -e MYSQL_DATASOURCE=LiferayDS \
    --build-env=LF_REPO_PREFIX=$LF_REPO_PREFIX \
    -l liferay=

$ oc set env dc/$APP_NAME --from=secret/mysql

$ oc set resources dc/$APP_NAME --limits=cpu=2,memory=1536Mi --requests=cpu=1,memory=1535Mi

$ oc create cm liferay-portal-ext --from-file=docker/portal-ext.properties

$ oc set volume dc/$APP_NAME --add --name=portal-ext-props --type=configmap --mount-path=/opt/app-src/liferay/portal-ext.properties \
    --configmap-name=liferay-portal-ext --sub-path=portal-ext.properties # --overwrite

```
