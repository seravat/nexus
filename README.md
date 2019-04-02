# Sonatype Nexus on OpenShift

This repo contains OpenShift templates and scripts for deploying Sonatype Nexus 2 an 3
and pre-configuring Red Hat and JBoss maven repositories on Nexus via post deploy hooks.
You can modify the post hook in the templates and add other Nexus repositories by using these [helper
functions](scripts/nexus-functions).

```
post:
  execNewPod:
    containerName: ${SERVICE_NAME}
    command:
      - "/bin/bash"
      - "-c"
      - "curl -o /tmp/nexus-functions -s https://raw.githubusercontent.com/OpenShiftDemos/nexus/master/scripts/nexus-functions; source /tmp/nexus-functions; add_nexus3_redhat_repos admin admin123 http://${SERVICE_NAME}:8081"
```

# Import Templates

In order to add Sonatype Nexus templates to OpenShift service catalog run the following commands:

Sonatype Nexus 2:
```
oc create -f https://raw.githubusercontent.com/seravat/openshift-nexus/master/nexus2-template.yaml
oc create -f https://raw.githubusercontent.com/seravat/openshift-nexus/master/nexus2-persistent-template.yaml
```

Sonatype Nexus 3:
```
oc create -f https://raw.githubusercontent.com/seravat/openshift-nexus/master/nexus3-template.yaml
oc create -f https://raw.githubusercontent.com/seravat/openshift-nexus/master/nexus3-persistent-template.yaml
```

# Deploy Nexus 2

Deploy Sonatype Nexus 2 using one of the provided templates. If you have persistent volumes available in your cluster:
```
oc new-app nexus2-persistent
```
Otherwise:
```
oc new-app nexus2
```
# Deploy Nexus 3

Deploy Sonatype Nexus 3 using one of the provided templates. If you have persistent volumes available in your cluster:
```
oc new-app nexus3-persistent
```
Otherwise:
```
oc new-app nexus3
```

# Deploy Specific Version
In order to specify the Nexus version to be deployed use ```NEXUS_VERSION``` parameter:
```
oc new-app nexus2 -p NEXUS_VERSION=2.14.3
oc new-app nexus3 -p NEXUS_VERSION=3.5.2
```
# Configure Nexus Server with the RedHat repositories

Once all of the components of your application are up, you are ready to configure the Nexus server and add three Maven repositories to the list of proxied repositories.

1. Check the route for Nexus:
$ oc get route | grep nexus

nexus     nexus-xpaas.cloudapps.test.openshift.opentlc.com             nexus      web

2. In a browser window, navigate to the URL of the Nexus route.
3. Log in with the username admin and password admin123.
4. In the navigation panel on the left, click Repositories.
5. Click the Add icon in the top menu to access the list of options.
6. Click Proxy Repository and on the New Proxy Repository page, enter the following values, leaving the other fields unchanged:
    * Repository ID: redhat-ga
    * Repository Name: Red Hat GA
    * Remote Storage Location: https://maven.repository.redhat.com/ga/
7. Click Save.
8. Add the Red Hat GA repository to the public repository group:
    1. In the left navigation panel, click Repositories.
    2. Select Public Repositories.
    3. In the bottom panel, click the Configuration tab.
    4. Make sure that the Red Hat GA repository is in the Ordered Group Repositories panel:
    5. Click Save.
9. Add the JBoss Public Repository, https://repository.jboss.org/nexus/content/repositories/public/, as a proxy repository and add it to the public repository group, following the same steps you used to set up the Red Hat GA repository:
10. Add the Red Hat Early Access repository, https://maven.repository.redhat.com/earlyaccess/all/, as a proxy repository and add it to the public repository group, again following the same steps.
The Git server and Nexus server are set up for your environment. You use these for running the labs that follow.

