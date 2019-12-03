JSON-Compress
=====
# Update to version 1.7.0

[Changelog](/changelog.txt)

## Background

One of the problems you can have developing rich internet applications (RIA) using Javascript is the amount of data being transported to and from the server.
When data comes from server, this data could be GZipped, but this is not possible when the big amount of data comes from the browser to the server.

##### JSON-Compress was born to change the way browser vendors think and become an standard when send information to the server efficiently.


JSON-Compress has two differents approaches to reduce the size of the amount of data to be transported:

* *JSONC.compress* - Compress JSON objects using a map to reduce the size of the keys in JSON objects.
    * Be careful with this method because it's really impressive if you use it with a JSON with a big amount of data, but it
could be awful if you use it to compress JSON objects with small amount of data because it could increase the final size.
    * The rate compression could variate from 7.5% to 32.81% depending of the type and values of data.
* *JSONC.pack* - Compress JSON objects using GZIP compression algorithm, to make the job JSONC uses a modification to
use the gzip library and it encodes the gzipped string with Base64 to avoid url encode.
   * Gzip - @beatgammit - https://github.com/beatgammit/gzip-js
   * Base64 - http://www.webtoolkit.info/
   * You can use pack to compress any JSON objects even if these objects are not been compressed using JSONC
See Usage for more details.

## Install

### Add library to your project

    // Download from GitHub because this is not my original work
    yarn add https://github.com/Alamantus/JSON-Compress.git
    // or
    npm install git+https://github.com/Alamantus/JSON-Compress.git

## Usage

### Load for use in script:

    // Returns the JSONC object with the following methods
    var JSONC = require( 'json-compress' );

### Compress a JSON object:

    // Returns a JSON object but compressed.
    var compressedJSON = JSONC.compress( json );

### Decompress a JSON object:

    // Returns the original JSON object.
    var json = JSONC.decompress( compressedJSON );

### Compress a normal JSON object as a Gzipped string:

    // Returns the LZW representation as string of the JSON object.
    var lzwString = JSONC.pack( json );

### Decompress a normal JSON object from a Gzipped string:

    // Returns the original JSON object.
    var json = JSONC.unpack( gzippedString );

### Compress a JSON object with JSONC before compressing as a Gzipped string:

    // Returns the LZW representation as string of the JSON object.
    var lzwString = JSONC.pack( json, true );

### Decompress a JSON object that was compressed with JSONC from a Gzipped string:

    // Returns the original JSON object.
    var json = JSONC.unpack( gzippedString, true );

# Modify global JSON

### You should probably _NOT_ do this, but it makes it more convenient to use

    // Inject JSONC functions into global JSON object
    require( 'json-compress' ).inject( JSON );
    // Use JSONC functions directly from JSON object
    var compressedJSON = JSON.compress( json );
    var json = JSON.decompress( compressedJSON );
    var lzwString = JSON.pack( json );
    var json = JSON.unpack( lzwString );
