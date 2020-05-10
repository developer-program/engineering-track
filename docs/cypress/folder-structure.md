# Folder Structure

Cypress by default will create 4 folders for you.

- fixtures
- integration
- plugins
- supports

## Integration

The main file you will work on. All your test files will be here.

It creates a default folder of `cypress/integration/examples` for us. These folder contains examples for us to test the installation and also acts as a demo to show you every way you can use cypress. Even more advance implementation like stubs and resizing viewport for testing fluid designs.

## Fixtures

Mock data that you might need. We will not be using fixtures, as we want cypress to act as an E2E test tool, meaning we want actual server implementations to be tested.

## Plugins

Codes that extend cypress functionality. You can extend cypress functionality using plugins such as to send failure reports on slack or add visual tools such as Applitools and Percy.

## Support

### commands.js

Commands.js is where you can add common utility functions that you will use it often in the integration folder, i.e. login or closing modal

### index.js

This is where you put code that you want to run before any other test if you need to clean up some files or setup data.
