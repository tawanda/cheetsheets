Git global setup
```
git config --global user.name "Tawanda"
git config --global user.email "tawanda@example.com"
```

Create a new repository
```
git clone git@gitlab.com:siege-software/myrepo/myrepo.git
cd saleor
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```

Push an existing folder
```
cd existing_folder
git init
git remote add origin git@gitlab.com:siege-software/myrepo/myrepo.git
git add .
git commit -m "Initial commit"
git push -u origin master
```

Push an existing Git repository
```
cd existing_repo
git remote rename origin old-origin
git remote add origin git@gitlab.com:siege-software/myrepo/myrepo.git
git push -u origin --all
git push -u origin --tags
```
