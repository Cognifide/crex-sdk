SDK documentation can be found under [https://mateuszluczak.github.io/crex-sdk](JSDoc)

This Wiki is an in-depth guide for using command line tool called `ce` which is available when sdk is installed globally

## Installation
```
npm install crex -g
```

## General
### Default environment
When no additional configuration provided/any paramters passed CLI will use `localhost:4502` with credentials `admin`/`admin` as default environment.
### Download/Upload/Create from target environment
Option `-t` can be used to pass environment string

**Example:**
```
$ ce download Brand -t admin:admin@localhost:4502
```
### AuthFile
When file named `auth.json` is created in the working directory of the CLI it will be used for the environment configuration in all the download and upload commands:

**Example**

`auth.json`
```
{
  "user": "admin",
  "password": "admin",
  "url": "localhost",
  "port": 4502 // optional
  "proxy: { // optional
    "*":"http://18.0.01"
  }
}
```

How to configure proxy:
https://visionmedia.github.io/superagent/#forcing-specific-connection-ip-address

### AuthFile with multiple environments
File `auth.json` can contain more environment configurations in format visible below. However option `-e` **must** be provided to specify which configuration should be used

**Example**

`auth.json`
```
{
  "int": {
    "user": "admin",
    "password": "admin",
    "url": "localhost",
    "port": 4502
  },
  "prod": {
    "user": "admin",
    "password": "admin",
    "url": "localhost",
    "port": 6502
  }
}
```
```
$ ce download content/default/en-gb/brand -e prod
```
## Create
### Basic usage
Basic create command takes `content path` and create package on with 
```
$ ce create <path_to_content>
```
**Example**
```
$ ce create content/default/en-gb/brand
```
### Name - `-n`
By providing option `-n` specific directories can be compressed and uploaded as a package.
```
$ ce create <path_to_content> -n <name>
```
**Example**
```
$ ce create content/default/en-gb/brand -n Brand
```

## Download
### Basic usage
Basic `download` command download content from the instance and downloads the `.zip` file containing the package
```
$ ce download <name_of_package>
```

### Download from path - `-p`
When option `-p` is provided you can download package from specific path.
```
$ ce download -p <path_to_content>
```
**Example**
```
$ ce download -p /content/default/en-gb/brand
```

### Extracting - `-x`
When option `-x` is provided package will be extracted. By default package is extracted to the current working directory, however this can be changed by passing `path` as a parameter. After extraction `.zip` is deleted.
```
$ ce download <name_of_package> -x [path]
```
**Example**
```
$ ce download Brand -x /packages
```
### Filtering specific directories - `-f`
When package is extracted by default of the encountered paths are overwritten. There is no way of changing that behaviour, however by providing option `-f` we could limit folders that will be extracted from the package. Filters can be comma separated list of directories.
```
$ ce download <name_of_package> -x [path] -f <paths>
```
**Example**
```
$ ce download Brand -x /packages -f etc
```

## Upload
### Basic usage
Basic upload command takes `.zip` file and uploads and installs it to AEM instance
```
$ ce upload [path_to_zip]
```
**Example**
```
$ ce upload theme-content-default-en-gb-brand.zip
```
### Compressing - `-c`
By providing option `-c` specific directories can be compressed and uploaded as a package.
```
$ ce upload -c <directories>
```
**Example**
```
$ ce upload -c content,etc
```
### Omitting - `-o`
When option `-o` is used with `-c` you can specify list of [globs](https://github.com/isaacs/node-glob) that will be ignored during creating zip.

**Example**
```
$ ce upload -c etc/designs/zg/basic -o **/.sass-cache/**,**/node_modules/**
```

