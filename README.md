git repo for our apt repository

To use the repo:
```
sudo apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys CD4AC0F7DC18C361
echo "deb http://grnds-operations-aptitude.s3-website-us-east-1.amazonaws.com/ubuntu/ trusty main" | sudo tee /etc/apt/sources.list.d/grnds.list
sudo apt-get update
```

To add a package:
* Import the `ops@grnds.com` signing key into your keychain to be able to add new packages, then:
```
cd repo/ubuntu
git checkout master
git pull --rebase
aws s3 sync s3://grnds-operations-aptitude/ . --exclude ".git/*"
reprepro -C main includedeb trusty some-file_0.0.1_all.deb
rm some-file_0.0.1_all.deb
git add .
git commit -m "description of change goes here"
git push
aws s3 sync . s3://grnds-operations-aptitude/ --exclude ".git/*"
```
This keeps all of the metadata in git, but uses the S3 bucket for serving both metadata and content.
