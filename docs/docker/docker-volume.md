# Docker Volume

Docker Volume allows a Docker container to use a folder outside of the container without the need to `COPY` it in.
This is especially useful if you want a Docker container to persist data (e.g. in a database) or watch for local changes.

## Running a test using Docker Container

Say we want to run a test file:
```javascript
describe("test", () => {
    it("1 is 1", () => {
        expect(1).toBe(1);
    })
});
```

In `package.json`:
```json
"scripts": {
  "test": "jest --watchAll"
}
```

We can run the test in a Docker container by overwriting the default command.

1. Run the build command
   ```sh
   $ docker build .
   ```
2. Copy the id of the generated image
3. To overwrite the default command, i.e. `CMD ["npm", "start"]`, and run the test as default command
   ```sh
   $ docker run -it <image id> npm run test
   ```
4. The test should run successfully

## Tagging a Docker image

The previous example runs, but it is troublesome to copy the image ID before we can run the test. As such,
we can add a tag to the built image, which allow us to reference the image by its tag name.

```sh
$ docker build -t unit-test .
$ docker run -it unit-test npm run test
```

The above example tags the built image as `unit-test`.

## Volume

Next we can look at what happens when we change our test case.

Try updating the test case and you will realise that the updated test does not re-run - a non-typical behaviour compared
to the default `watch` mode by jest.

The reason why the test didn't re-run when we update it is due to the fact that the `watch` mode is watching the files
_within_ the container but not the files on your local system that you have modified.

We can solve that by adding a Docker Volume which maps the container directory to your working directory.

```sh
$ docker build -t unit-test .
$ docker run -it -v $(pwd):/app unit-test npm run test
```

In the above command, we added a new flag `-v $(pwd):/app`.
The format for volumne flag is `-v <local dir>:<docker container dir>`.

### Dependencies

When the Docker container looks for dependencies, it will look for the `node_modules` folder in our local directory
rather than in the Docker container. While it will work (i.e. run the tests properly), it defeats the purpose of using
Docker as we have to build our package locally, meaning that we need to install `node` locally.

Note that if you do not have the `node_modules` folder in your local directory, you probably missed out doing
`npm install` and the test would not be running properly.

A better way is to have the Docker container install the dependencies for us and we use the dependencies _in_
the container. To do so, add another flag `-it -v /app/node_modules`. This tells the container that when looking for
dependencies, refer to the `node_modules` folder _in_ the container.

The full command will be:
```sh
$ docker build -t unit-test .
$ docker run -it -v /app/node_modules -v $(pwd):/app unit-test npm run test
```

Now everything should work as expected.

## Docker Volume in Docker Compose

Similar to above, we can use Docker Volume within a docker-compose file to view live changes on our react app
or when doing development work in express using nodemon.

Simply add the following to the service in the docker-compose file you want to map the volume to:

```yml
volumes:
  - /app/node_modules
  - .:/app
```
