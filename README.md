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

<p>Lemmy requires a valid config.hjson file to run. You can download it from the releases <a href="https://github.com/LemmyNet/lemmy/releases">here</a>.</p>
<p>Configure the Lemmy chart to use your configuration values. You can set the config.hjson values in the configmap:</p>

<ul>
  <li><a href="/lemmy/values.yaml">lemmy/values.yaml</a></li>
  <li><a href="/lemmy/templates/20-configmap.yaml">lemmy/templates/20-configmap.yaml</a></li>
</ul>

<p>You can then deploy Lemmy using:</p>

<code>kubectl apply -f ./lemmy</code>

<p>Congratulations! You have successfully set up Lemmy on your cluster.</p>

[Grouvie](https://github.com/grouvie/)
