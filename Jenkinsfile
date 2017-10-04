pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh '''#!/bin/bash -e

git clone git@github.com:keycloak/keycloak.git $WORKSPACE/checkout
cd $WORKSPACE/checkout

git config user.email "keycloak.bot@gmail.com"
git config user.name "keycloak-bot"

if (git tag --list | grep "^$VERSION$"); then
	echo "Tag for $VERSION already exists"
    exit 1
fi

if [ "$BRANCH" != "master" ]; then
	echo "Checkout branch $BRANCH"
	git checkout origin/$BRANCH -b $BRANCH
fi

# Update versions
echo "Setting POM versions"
mvn versions:set -DnewVersion=$VERSION -DgenerateBackupPoms=false -DgroupId=org.keycloak* -DartifactId=* -DoldVersion=*

echo "Committing POM version updates"
git commit -a -m "Set version to $VERSION"

# Build
echo "Building"
mvn -Pjboss-release -DretryFailedDeploymentCount=10 -DskipTests deploy

# Tag
echo "Creating tag"
git tag $VERSION


# Set next version
echo "Setting POM versions for next release"
NEXT_VERSION=$NEXT_VERSION"-SNAPSHOT"
mvn versions:set -DnewVersion=$NEXT_VERSION -DgenerateBackupPoms=false -DgroupId=org.keycloak* -DartifactId=* -DoldVersion=*

echo "Committing POM version updates for next release"
git commit -a -m "Set version to $NEXT_VERSION"

echo "Push to Github"
git push --tags origin $BRANCH:$BRANCH

echo "Setting current release"
echo "$VERSION" > $WORKSPACE/../../lastRelease'''
      }
    }
  }
}