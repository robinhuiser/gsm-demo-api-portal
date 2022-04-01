# gsm-demo-api-portal

Demo repo using Git Submodule to centrally retrieve OAS 3.x files.

## Initial setup (one time only)

~~~bash
# Add central OAS repo as Git Submodule
# This is a one time setup
$ git submodule add \
   https://github.com/robinhuiser/gsm-demo-oas-repo.git

# Let's commit this change to the repo
$ git add .gitmodules
$ git add gsm-demo-oas-repo
$ git commit -m "Added git submodule"
$ git push -u origin main
~~~

## Do a release / build

A pipeline needs to checkout the Git repository for the component AND submodules, see:

~~~bash
# Check out the component (api-portal) repo
$ git clone https://github.com/robinhuiser/gsm-demo-api-portal.git 

# You will notice the ./gsm-demo-oas-repo is empty...
# Let's update and init the submodule with the commit ID present in the .gitsubmodule file
$ git submodule update --init --recursive
~~~

Next, we can "release" the API portal based upon the commit ID reference in the repo!

~~~bash
# Let's "build" our project to generate the API portal
$ mkdir ./target

# For each spec in our API portal list, copy to ./target
$ for spec in $(cat ./oas-specs.txt); do
   cp ./gsm-demo-oas-repo/$spec ./target
done
~~~

That's all folks!
