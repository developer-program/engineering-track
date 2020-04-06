# Docker Volume

Volume allow a docker container to use a folder outside of the container without the need to COPY it in. This is especially useful if you want a docker container to persist data(in a database) or watch for local changes. 

## Running a test using Docker Container

Say we want to run a test file
```javascript
describe("test", () => {
    it("1 is 1", () => {
        expect(1).toBe(1);
    })
});
```

In `package.json`
```json
  "scripts": {
    "test": "jest --watchAll"
  },
```

We can run test in docker container by overwrrite the starting command.

1. `docker build .`
2. Copy the id of the generated image
3. `docker run -it <image id> npm run test` and we can overwrite the default starting command of `CMD ["npm", "start"]` commands with `npm run test` which runs the test.

The test should run successfully.

The above function runs but is troublesome to copy the id before we can run the test. We can add a tag when build which allow us to referrence the image by it's tag name.

`docker build -t unit-test . && docker run -it unit-test npm run test`

Next we can look at what happen when we change our testcse. 

Try updating a test case and you will realise that the test does not rerun, a non typical behavior on the default watch mode by jest.

The reson why the test didn't rerun when we update is due to the watch mode watching at the files within the container but not the file on your local system that you are modifying.

We can solve that by adding a docker volume which maps the directory to your working dir instead.

`docker build -t unit-test . && docker run -it -v $(pwd):/app unit-test npm run test`

The above command we add a new flag `-v $(pwd):/app`. The format for volumne flag is `-v <lcoal dir>:<docker container dir>`.

If you have a node_modules on your local dir, this will work and if you don't have one(you never `npm i`) before, the test will not run properly. 

When the docker container looks for dependcies, it now will look for the node_modules in our local dir rather than in docker container. This might work but defeats the purpose of using docker as we have to build our package locally meaning that we need `node` to be installed locally. 

A better way is to have docker container install the dependencies for us and we use the dependencies in the container. add another flag `-it -v /app/node_modules`. This tells the container that for node_modules, we want to use the folder in the container.

The full command
`docker build -t unit-test . && docker run -it -v /app/node_modules -v $(pwd):/app unit-test npm run test"`

Now everything should work as expected.

## Docker Volume in docker compose

Similar to above, we can use docker volume to view life changes on our react app or when doing development work in express using nodemon. 
Simply add the following to the service you want to map the volume to. 

```yml
    volumes:
      - /app/node_modules
      - .:/app
```




