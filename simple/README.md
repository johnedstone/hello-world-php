```
# Simplest way to get this app up and running
shell> oc new-app --context-dir=simple/ https://github.com/johnedstone/hello-world-php.git 
shell> expose svc hello-world-php --hostname=hello-world-php.<wildcard dns>

# After making changes to code later, then commit, and push to Git, then
shell> oc start-build hello-world-php

# And watch the pods get rebuilt
shell> oc get pods -w -o wide

# Other notes
# To see what would be done, before creating new-app
shell> oc new-app --context-dir=simple/ -o yaml php:latest~https://github.com/johnedstone/hello-world-php.git | tee /var/tmp/withphp
shell> oc new-app --context-dir=simple/ -o yaml https://github.com/johnedstone/hello-world-php.git | tee /var/tmp/withoutphp

# No difference whether one calls out the php image - must detect
shell> sdiff /var/tmp/*php
apiVersion: v1                                                  apiVersion: v1
items:                                                          items:
- apiVersion: v1                                                - apiVersion: v1
  kind: ImageStream                                               kind: ImageStream
  metadata:                                                       metadata:
    annotations:                                                    annotations:
      openshift.io/generated-by: OpenShiftNewApp                      openshift.io/generated-by: OpenShiftNewApp
    creationTimestamp: null                                         creationTimestamp: null
    labels:                                                         labels:
      app: hello-world-php                                            app: hello-world-php
    name: hello-world-php                                           name: hello-world-php
  spec: {}                                                        spec: {}
  status:                                                         status:
    dockerImageRepository: ""                                       dockerImageRepository: ""
- apiVersion: v1                                                - apiVersion: v1
  kind: BuildConfig                                               kind: BuildConfig
  metadata:                                                       metadata:
    annotations:                                                    annotations:
      openshift.io/generated-by: OpenShiftNewApp                      openshift.io/generated-by: OpenShiftNewApp
    creationTimestamp: null                                         creationTimestamp: null
    labels:                                                         labels:
      app: hello-world-php                                            app: hello-world-php
    name: hello-world-php                                           name: hello-world-php
  spec:                                                           spec:
    output:                                                         output:
      to:                                                             to:
        kind: ImageStreamTag                                            kind: ImageStreamTag
        name: hello-world-php:latest                                    name: hello-world-php:latest
    resources: {}                                                   resources: {}
    source:                                                         source:
      contextDir: simple/                                             contextDir: simple/
      git:                                                            git:
        uri: https://github.com/johnedstone/hello-world-php.g           uri: https://github.com/johnedstone/hello-world-php.g
      type: Git                                                       type: Git
    strategy:                                                       strategy:
      sourceStrategy:                                                 sourceStrategy:
        from:                                                           from:
          kind: ImageStreamTag                                            kind: ImageStreamTag
          name: php:latest                                                name: php:latest
          namespace: openshift                                            namespace: openshift
      type: Source                                                    type: Source
    triggers:                                                       triggers:
    - github:                                                       - github:
        secret: P9wzJhjSiuA3ZItAdsu8                          |         secret: twBs7x-GyUvTUawFeGxZ
      type: GitHub                                                    type: GitHub
    - generic:                                                      - generic:
        secret: F6mxkLk38NL3qQB55iv-                          |         secret: S3CHH23gicZmX00myrVe
      type: Generic                                                   type: Generic
    - type: ConfigChange                                            - type: ConfigChange
    - imageChange: {}                                               - imageChange: {}
      type: ImageChange                                               type: ImageChange
  status:                                                         status:
    lastVersion: 0                                                  lastVersion: 0
- apiVersion: v1                                                - apiVersion: v1
  kind: DeploymentConfig                                          kind: DeploymentConfig
  metadata:                                                       metadata:
    annotations:                                                    annotations:
      openshift.io/generated-by: OpenShiftNewApp                      openshift.io/generated-by: OpenShiftNewApp
    creationTimestamp: null                                         creationTimestamp: null
    labels:                                                         labels:
      app: hello-world-php                                            app: hello-world-php
    name: hello-world-php                                           name: hello-world-php
  spec:                                                           spec:
    replicas: 1                                                     replicas: 1
    selector:                                                       selector:
      app: hello-world-php                                            app: hello-world-php
      deploymentconfig: hello-world-php                               deploymentconfig: hello-world-php
    strategy:                                                       strategy:
      resources: {}                                                   resources: {}
    template:                                                       template:
      metadata:                                                       metadata:
        annotations:                                                    annotations:
          openshift.io/generated-by: OpenShiftNewApp                      openshift.io/generated-by: OpenShiftNewApp
        creationTimestamp: null                                         creationTimestamp: null
        labels:                                                         labels:
          app: hello-world-php                                            app: hello-world-php
          deploymentconfig: hello-world-php                               deploymentconfig: hello-world-php
      spec:                                                           spec:
        containers:                                                     containers:
        - image: library/hello-world-php:latest                         - image: library/hello-world-php:latest
          name: hello-world-php                                           name: hello-world-php
          ports:                                                          ports:
          - containerPort: 8080                                           - containerPort: 8080
            protocol: TCP                                                   protocol: TCP
          resources: {}                                                   resources: {}
    triggers:                                                       triggers:
    - type: ConfigChange                                            - type: ConfigChange
    - imageChangeParams:                                            - imageChangeParams:
        automatic: true                                                 automatic: true
        containerNames:                                                 containerNames:
        - hello-world-php                                               - hello-world-php
        from:                                                           from:
          kind: ImageStreamTag                                            kind: ImageStreamTag
          name: hello-world-php:latest                                    name: hello-world-php:latest
      type: ImageChange                                               type: ImageChange
  status: {}                                                      status: {}
- apiVersion: v1                                                - apiVersion: v1
  kind: Service                                                   kind: Service
  metadata:                                                       metadata:
    annotations:                                                    annotations:
      openshift.io/generated-by: OpenShiftNewApp                      openshift.io/generated-by: OpenShiftNewApp
    creationTimestamp: null                                         creationTimestamp: null
    labels:                                                         labels:
      app: hello-world-php                                            app: hello-world-php
    name: hello-world-php                                           name: hello-world-php
  spec:                                                           spec:
    ports:                                                          ports:
    - name: 8080-tcp                                                - name: 8080-tcp
      port: 8080                                                      port: 8080
      protocol: TCP                                                   protocol: TCP
      targetPort: 8080                                                targetPort: 8080
    selector:                                                       selector:
      app: hello-world-php                                            app: hello-world-php
      deploymentconfig: hello-world-php                               deploymentconfig: hello-world-php
  status:                                                         status:
    loadBalancer: {}                                                loadBalancer: {}
kind: List                                                      kind: List
metadata: {}                                                    metadata: {}
[rx62126_lx@pocosemas01]$ oc new-app --context-dir=simple/ https://github.com/johnedstone/hello-world-php.git
--> Found image 6570d23 (4 weeks old) in image stream "php in project openshift" under tag :latest for "php"
    * The source repository appears to match: php
    * A source build using source code from https://github.com/johnedstone/hello-world-php.git will be created
      * The resulting image will be pushed to image stream "hello-world-php:latest"
    * This image will be deployed in deployment config "hello-world-php"
    * Port 8080/tcp will be load balanced by service "hello-world-php"
--> Creating resources with label app=hello-world-php ...
    ImageStream "hello-world-php" created
    BuildConfig "hello-world-php" created
    DeploymentConfig "hello-world-php" created
    Service "hello-world-php" created
--> Success
    Build scheduled for "hello-world-php" - use the logs command to track its progress.
    Run 'oc status' to view your app.
```
