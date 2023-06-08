<h1 align="center"><a href="https://lemmy.help/">Lemmy_Help</a></h1>

<h3>Lemmy_Help is a setup guide that provides instructions for deploying <a href="https://github.com/LemmyNet/lemmy">Lemmy</a>, a self-hosted link aggregator and social platform, using Helm charts. This repository aims to simplify the deployment process and assist users in setting up Lemmy on their cluster.</h3>
<p>These helm charts are configured to work with <a href="https://cert-manager.io/">cert-manager</a>.</p>
<p>Make sure to set the correct issuerName in the values.yaml files.</p>
<h2>Postgres and pgAdmin Setup:</h2>
<p>Configure the postgres chart to use your configuration values:</p>
<ul>
  <li><a href="/postgres/templates/20-secret.yaml">postgres/templates/20-secret.yaml</a></li>
  <li><a href="/postgres/values.yaml">postgres/values.yaml</a></li>
</ul>
<p>You can then deploy postgres and pgAdmin using:</p>

<code>kubectl apply -f ./postgres</code>
<p>When postgres and pgAdmin are running, you can use pgAdmin to create a database, user, and password for Lemmy. Login to pgAdmin and add your postgres instance in the UI.</p>
<p>The postgres connection string to add your instance in pgAdmin should be similar to this one:</p>
<code>postgres-service-p.postgres.svc.cluster.local</code>
<h2>Pict-Rs Setup:</h2>
<p>Configure the pict-rs chart to use your configuration values:</p>
<ul>
  <li><a href="/pictrs/templates/30-secret.yaml">pictrs/templates/30-secret.yaml</a></li>
  <li><a href="/pictrs/values.yaml">pictrs/values.yaml</a></li>
</ul>
<p>You can then deploy pict-rs using:</p>

<code>kubectl apply -f ./pictrs</code>
<h2>Lemmy Setup:</h2>
<p>Configure the Lemmy chart to use your configuration values:</p>
<ul>
  <li><a href="/lemmy/values.yaml">lemmy/values.yaml</a></li>
</ul>
<p>You can then deploy lemmy using:</p>

<code>kubectl apply -f ./lemmy</code>
<p>Lemmy requires a valid config.hjson file to run. You can download it from the releases <a href="https://github.com/LemmyNet/lemmy/releases">here</a>.</p>
<p>Edit the values to fit your configuration. Example values:</p>

```yaml
{
    setup: {
    admin_username: "admin"
    admin_password: "password"
    site_name: "Lemmy Help"
    }
    hostname: "lemmy.example.com"
    bind: "0.0.0.0"
    port: 8536
    tls_enabled: true
    pictrs: {
    url: "http://pictrs-service.pictrs.svc.cluster.local:8080/"
    api_key: ""
    }
    database: {
    database: "lemmy"
    user: "lemmy"
    password: "lemmy-password"
    host: "postgres-service-p.postgres.svc.cluster.local"
    port: 5432
    pool_size: 5
    }
}
```
<p>You can then copy the config.hjson file to the Lemmy volume. For example, like this:</p>

<code>kubectl cp -n lemmy config.hjson lemmy:/config</code>
<p>If this does not work, you can set the deployment.replicaCount value to 0 in the <a href="/lemmy/values.yaml">values.yaml</a> and use a debugger pod to attach to the volume and copy the config file to the volume with it.</p>

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: busybox
  namespace: lemmy
spec:
  containers:
  - image: busybox
    command:
      - sleep
      - "3600"
    imagePullPolicy: IfNotPresent
    name: busybox
    volumeMounts:
    - name: busybox-data
      mountPath: /data
  volumes:
    - name: busybox-data
      persistentVolumeClaim:
        claimName: lemmy-pvc
  restartPolicy: Always
```
<p>The command to copy the config file to the volume is:</p>

<pre><code>kubectl cp -n lemmy config.hjson busybox:/data</code></pre>

<p>Once the file is successfully copied to the volume, you can delete the busybox debugger:</p>

<pre><code>kubectl delete pod -n lemmy busybox</code></pre>

<p>After restarting the Lemmy pod with the config file in the volume, it should launch successfully.</p>

<p>Congratulations! You have successfully set up Lemmy on your cluster.</p>

[Grouvie](https://github.com/grouvie/)
