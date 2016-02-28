# Travis JVM Docker Image for Debugging

Utilises the [travis-jvm](https://quay.io/repository/travisci/travis-jvm) image along with [travis-cli](https://github.com/travis-ci/travis.rb) and [travis-build](https://github.com/travis-ci/travis-build) to create an environment where you can debug/test your Travis JVM builds.

## Building

To build from [Dockerfile](https://github.com/tellisnz/travis-jvm-debug) do the following:
```
docker build -t tellisnz/travis-jvm-debug .
```

## Running

When started with:
```
docker run -it --name travis-debug tellisnz/travis-jvm-debug
```
You'll arrive at a bash prompt in `/home/travis/build`. In here, you can clone your project, ensuring you include your username in the full path, like:
```
git clone https://github.com/myuser/myrepo.git myuser/myrepo
```
This is because the generated travis build script relies on the full path. You then compile your build script with:
```
cd myuser/myrepo && travis compile > ~/build/build.sh && chmod 755 ~/build/build.sh && cd ~/build
```
or equivalent. You can then try running your build with the ~/build/build.sh script.

## Rerunning Builds

If you'd like to be able to re run your builds (as dependencies can take a while to download), ensure your scripts tidy up your file system. Then, after exiting from a previous container run, restart and attach to the container with:
```
docker start travis-debug
docker attach travis-debug
```
You'll then be starting afresh, but hopefully with your dependencies from the previous run retained.
