#Produce Client 

Javascript client, part of "Collaborative Contract Driven Design", demonstrating how a non-Java developer can use Spring Cloud Contract to do contract driven development.

Using this project currently.

You will need to clone the produce-service [repo](https://github.com/wkorando/produce-service).

The API for the produce service can be seen here: [Produce API](http://htmlpreview.github.io/?https://github.com/wkorando/collaborative-contract-driven-development-2-0/blob/master/index.html)

##Perquisites: 
Find your local IP address, docker will be making calls out to loca resources and some of hte script files will ne to be updated as a result. 

### Clone Repo

Run: `git clone https://github.com/wkorando/produce-client`

### Update scripts:

If you you are on mac you and running wireless you can run: `ifconfig en0` which should return the needed IP address, alternatively `ifconfig en1` should work if wired. With that ipaddress update the IP addresses in properties `APPLICATION_BASE_URL` and `REPO_WITH_BINARIES_URL` in the file `validate-produce-service.sh` and IP address for the property `STUBRUNNER_REPOSITORY_ROOT` in `run-produce-service-mock.sh`. 

### Start local artifactory instance

Run: `./start-artifactory.sh`

This will start local artifactory instance. This will be used for storing the stubs that Spring Cloud Contract generates when it executes contracts stored here: [produce-contracts](https://github.com/wkorando/produce-contracts)

### Start up RESTful API:

Run: `git clone https://github.com/wkorando/produce-service`

cd to the root of that project and run: `./mvnw spring-boot:run`


##Using Spring Cloud Contract Docker

Spring Cloud Contract has a docker image that can read in YAML contracts, see above, and produce stubs for them. This allows non-Java developers to write contracts and generate stubs without having to interact with the Java stack. Steps below demonstrate how to excute contracts to generate stubs and then run a mock server. The mock server reads in the generated stubs and gives stable base to build and test a client against (instead of connecting to a live service which might go down, be misconfigured, or have test data go missing).

###Execute Contracts anf Generate Stubs

Run: `./validate-produce-service.sh`

Takes roughly 30 seconds to run. If tests pass stubs will be generated and put in ocal artifactory. 

If tests fail check under `build-output/reports/tests/test/index.html` to view report on test run.

###Start up Mock Service

Run: `./run-produce-service-mock.sh`

This will start a docker image serving up the stubs at `http://localhost:9876`. As mentioned above this gives stable endpoint to work against. The response for a given request will always be the same, which is much more stable and easier to work for a client developer. If something is working correctly on the client side, it's because an issue with not matching API behavior, not because backend service is currently down. 

