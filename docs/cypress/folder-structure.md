# Folder Structure

Cypress Will create 4 folders for you.

- fixtures
- integration
- plugins
- supports

## Integration

The main file you will be working on. All your test file will be here.

A default folder of `integration/examples` is created for us. These folder contains examples for us to test the installtion and also acts as a demo to show you every possible way you can use cypress. Even more advance implemnetation like stubs and resizing viewport for testing fluid designs.

## Fixtures

Mock data that you might need. We will not be using fixtures, as we want cypress to act as an E2E test tool, meaning we want actual server implementations to be tested.

## Plugins

codes that extend cypress functionality. You can extends will cypress functionality using plugins such as to send failure report on slack or add visual tools such as Applitools and Percy.

## Support

### commands.js

Commands.js is where you can add common utility functions that you will use it often in the integration folder, i.e. login or closing modal

### index.js

This is where you put code that you want to run before any other test if you need to clean up some files or setup data.
