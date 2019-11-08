---
description: Guides you how to install the Connector on Debian
---

# Install the Connector \(Debian\)

## Prerequisites

Before installing the Connector, you must first install some supporting software.

{% hint style="info" %}
Be sure to interact with the command shell when executing the commands above. At various points, most of the following scripts will require you to enter `Y` to continue the installation process.
{% endhint %}

### Install Misc Tools

```bash
sudo apt-get update
sudo apt-get install maven
sudo apt-get install git
sudo apt-get install unzip
```

### Install OpenJDK 8

Use the following script to install OpenJDK 8.

{% tabs %}
{% tab title="installOpenJdk8.sh" %}
```bash
sudo apt-get update
sudo apt-get install default-jdk
```
{% endtab %}
{% endtabs %}

### Install Unlimited JCE Policy

{% tabs %}
{% tab title="installUnlimitedJCE.sh" %}
```bash
curl -L --cookie 'oraclelicense=accept-securebackup-cookie;'  http://download.oracle.com/otn-pub/java/jce/8/jce_policy-8.zip -o /tmp/jce_policy.zip
unzip -o /tmp/jce_policy.zip -d /tmp
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64/
sudo mv -f /tmp/UnlimitedJCEPolicyJDK8/US_export_policy.jar $JAVA_HOME/jre/lib/security/US_export_policy.jar
sudo mv -f /tmp/UnlimitedJCEPolicyJDK8/local_policy.jar $JAVA_HOME/jre/lib/security/local_policy.jar
```
{% endtab %}
{% endtabs %}

## Download & Build 

In order to install the connector, you must download the source and build it.

```bash
cd /srv
git clone https://github.com/sappenin/java-ilpv4-connector.git
cd java-ilpv4-connector
mvn clean install -DskipTests
mkdir /srv/connector-java
mv  /srv/java-ilpv4-connector/ilpv4-connector-server/target/ilpv4-connector-server-0.1.0-SNAPSHOT-exec.jar /srv/connector-java/
```

{% hint style="info" %}
Once the connector has its first official release, it will not be necessary to download and build the connector. Instead, you will be able to simply download a single executable and run that directly.
{% endhint %}

{% hint style="success" %}
And that's it! You've successfully installed the Connector!
{% endhint %}

