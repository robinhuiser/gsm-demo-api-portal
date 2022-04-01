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
# Let's update and init the submodule with the commit ID persisted in the repo
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

If we want to "bump" to a newer version of the OAS repo...

~~~bash
# Fetch the latest and greatest OAS updates from the submodule
# This assures we commit against main (for demo now)
$ cd ./gsm-demo-oas-repo
$ git pull origin main
$ git checkout main
$ cd ..

## -- Now we have in the project's .git directory an updated 
## -- Commit ID of the latest version

# Now we push our component repo changes 
# which contains a pointer to the just updated submodule repo
$ git add ./gsm-demo-oas-repo
$ git commit -m "Updated submodule gsm-demo-oas-repo"
$ git push -u origin main
~~~

That's all folks!
