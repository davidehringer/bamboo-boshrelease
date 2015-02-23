# Bamboo BOSH Release

## Running Locally

### Install BOSH Lite

Install BOSH Lite using the instructions at https://github.com/cloudfoundry/bosh-lite.  You do not need to install Cloud Foundry.

### Deploy Bamboo Release

* Upload the Warden stemcell

 A stemcell is a VM template with an embedded BOSH Agent. BOSH Lite uses the Warden CPI, so we need to use the Warden Stemcell which will be the root file system for all Linux Containers created by the Warden CPI.

 Download latest Warden stemcell:

 ```
 wget http://bosh-jenkins-artifacts.s3.amazonaws.com/bosh-stemcell/warden/latest-bosh-stemcell-warden.tgz
 ```

 Upload the stemcell:

 ```
 bosh upload stemcell latest-bosh-stemcell-warden.tgz
 ```

* Populate the blobs directory

The blobs aren't published yet to a blobstore so we need to populate our local blobs directory. It should look like the following:

```
$ tree blobs/
blobs/
├── apache-maven-3.2.5
│   └── apache-maven-3.2.5-bin.tar.gz
├── atlassian-bamboo-5.7.2
│   └── atlassian-bamboo-5.7.2.tar.gz
├── bamboo-agent-5.7.2
│   └── bamboo-agent-5.7.2.jar
├── openjdk-1.7.0_71
│   └── openjdk-1.7.0_71.tar.gz
└── openjdk-1.8.0_25
    └── openjdk-1.8.0_25.tar.gz

5 directories, 5 files
```

* Upload the Bamboo release

Create a dev release:

```
bosh create release --force 
```

Upload the release:

```
bosh upload release
```

* Generate a deployment manifest

```
bosh-lite/make_manifest
```

* Deploy

Target the generated deployment manifest

```
bosh deployment bosh-lite/manifests/bamboo-manifest.yml
```

Deploy

```
bosh deploy
```

Cross your fingers.

Bamboo server is available at http://10.244.2.2:8085