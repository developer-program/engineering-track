# Folder Structure

Cypress Will create 4 folders for you.

- fixtures
- integration
- plugins
- supports

## Fixtures

Mock data that you might need.

## Integration

The main file you will be working on. All your test file will be here. Feel free to delete the demo created. We will mainly work in this folder.

## Plugins

codes that extends cypress functionality

## Support

### commands.js

Commands.js is where you can add common utility functions that you will use it often, i.e. login or closing modal

### index.js

This is where you put code that you want run before any other test, if you need to clean up some files or setup data.
