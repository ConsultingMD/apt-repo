git repo for our apt repository

To use the repo:
```
sudo apt-key adv --keyserver ha.pool.sks-keyservers.net --recv-keys CD4AC0F7DC18C361
echo "deb https://consultingmd.github.io/apt-repo/ubuntu/ trusty main" | sudo tee /etc/apt/sources.list.d/grnds.list
sudo apt-get update
```

To add a package:
* Import the `ops@grnds.com` signing key into your keychain to be able to add new packages, then:
```
git checkout master
git checkout -b $deb_name_for_branch
cd repo/ubuntu
reprepro -C main includedeb trusty some-file_0.0.1_all.deb
rm some-file_0.0.1_all.deb
git add .
git commit -m "description of change goes here"
git push origin $deb_name_for_branch
```
Once it's committed in its own branch, go through the PR process to get your content merged into master.

After your content is in master, merge it into the `gh-pages` branch and push it back to GitHub to go live.
