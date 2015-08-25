# Cattlog #

## Introduction ##

A CLI tool for Laravel 5 to manage translation files and syncing entries with view files. Allows translations to be synced with views, get/set values, and list empty values.

## Installation ##

Still very much in the development stages. To use for now, add the following to composer.json:

    "repositories": [
        {
            "url": "https://github.com/martynbiz/cattlog-l5.git",
            "type": "git"
        }
    ],
    "require": {
        .
        .
        .
        "martynbiz/diff": "dev-master"
    },

Once a stable release is available, run the following:

    composer require martynbiz/cattlog

## Config ##

### Initialize config ###

Cattlog requires a cattlog.json config file at the root of the project to know where to scan source, save to file, filter to use etc:

    ./vendor/bin/cattlog init

This will create a cattlog.json file in the root of your project.

### Options ###

dest (string)

This is the destination path from the project root to where the keys are stored. It should include a {lang} which will be replaced by the given language code (e.g. "en").

    "dest": "lang/{en}.php",

src (string|array)

This is where the source files (view templates, controller etc) to scan for keys.

    "src": ["app/views", "app/controller"],

pattern (string|array)

This is the pattern to extract keys from source code. It can accept multiple patterns in an array. You can need to remember to escape escaped regular expression characters (e.g. \\s).

    "pattern": [
        "/xlate\\s*\\(\\s*[\\'|\\\"]([A-Za-z0-9_\\-\\.]*)[\\'|\\\"]/"
    ],

valid_languages (array)

This is an array of valid languages that the app provides. If a command (e.g. scan th) is run and the language code (th) is not in valid_languages then the process will halt.

    "valid_languages": ["en", "fr", "de"],

## Commands ##

### Scan ###

Will scan source code and find any new or old keys. Doesn't update any files.

./vendor/bin/cattlog scan en

### Update ###

Running this command will update all the keys from source files for the en file. If it finds new keys in source, it will save keys with empty values. It will also remove any keys which as no longer used. It will run `scan` first so you can see what keys will be added/removed before updating the destination file.

    ./vendor/bin/cattlog update en

### List ###

List all key / values currently stored for a given language

    ./vendor/bin/cattlog list en

### Count keys ###

Output total number of keys stored for a given language.

    ./vendor/bin/cattlog count en

### Setting values ###

Set an existing key value. If a key doesn't exist, run "cattlog update <lang>" to populate empty keys.

    ./vendor/bin/cattlog set_value en MY_KEY=something

Also, accepts json strings to be set (be sure to use single quotes though in the CLI).

    ./vendor/bin/cattlog set_value en MY_KEY='["%1$s Comment","%1$s Comments"]'

### Help ###

    Passing no parameters will list all options

    ./vendor/bin/cattlog
