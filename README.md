# api documentation for  [xml2json (v0.11.0)](https://github.com/buglabs/node-xml2json#readme)  [![npm package](https://img.shields.io/npm/v/npmdoc-xml2json.svg?style=flat-square)](https://www.npmjs.org/package/npmdoc-xml2json) [![travis-ci.org build-status](https://api.travis-ci.org/npmdoc/node-npmdoc-xml2json.svg)](https://travis-ci.org/npmdoc/node-npmdoc-xml2json)
#### Converts xml to json and vice-versa, using node-expat.

[![NPM](https://nodei.co/npm/xml2json.png?downloads=true)](https://www.npmjs.com/package/xml2json)

[![apidoc](https://npmdoc.github.io/node-npmdoc-xml2json/build/screenCapture.buildNpmdoc.browser._2Fhome_2Ftravis_2Fbuild_2Fnpmdoc_2Fnode-npmdoc-xml2json_2Ftmp_2Fbuild_2Fapidoc.html.png)](https://npmdoc.github.io/node-npmdoc-xml2json/build/apidoc.html)

![npmPackageListing](https://npmdoc.github.io/node-npmdoc-xml2json/build/screenCapture.npmPackageListing.svg)

![npmPackageDependencyTree](https://npmdoc.github.io/node-npmdoc-xml2json/build/screenCapture.npmPackageDependencyTree.svg)



# package.json

```json

{
    "bin": {
        "xml2json": "bin/xml2json"
    },
    "bugs": {
        "url": "https://github.com/buglabs/node-xml2json/issues"
    },
    "contributors": [
        {
            "name": "Arek W arek01@gmail.com"
        },
        {
            "name": "Camilo Aguilar camilo.aguilar@gmail.com"
        },
        {
            "name": "Craig Condon craig@spiceapps.com"
        },
        {
            "name": "Daniel Bretoi daniel@bretoi.com"
        },
        {
            "name": "Daniel Juhl danieljuhl@gmail.com"
        },
        {
            "name": "Dmitry Fink github@finik.net"
        },
        {
            "name": "Garvit Sharma garvits45@gmail.com"
        },
        {
            "name": "Julian Duque julianduquej@gmail.com"
        },
        {
            "name": "Karl Böhlmark karl.bohlmark@edgeware.tv"
        },
        {
            "name": "Kevin McTigue firefoxman1@gmail.com"
        },
        {
            "name": "Kirill Vergun github.com@o-nix.me"
        },
        {
            "name": "Maher Beg maherbeg@gmail.com"
        },
        {
            "name": "Nicholas Kinsey pyrotechnick@feistystudios.com"
        },
        {
            "name": "Rob Brackett rob@robbrackett.com"
        },
        {
            "name": "Subbu Allamaraju subbu@ebaysf.com"
        },
        {
            "name": "The Gitter Badger badger@gitter.im"
        },
        {
            "name": "Trotter Cashion cashion@gmail.com"
        },
        {
            "name": "Yan idy0013@gmail.com"
        },
        {
            "name": "Ziggy Jonsson ziggy.jonsson.nyc@gmail.com"
        },
        {
            "name": "andres suarez zertosh@gmail.com"
        },
        {
            "name": "andris9 andris@node.ee"
        },
        {
            "name": "fengmk2 fengmk2@gmail.com"
        }
    ],
    "dependencies": {
        "hoek": "^4.0.1",
        "joi": "^9.0.4",
        "node-expat": "^2.3.15"
    },
    "description": "Converts xml to json and vice-versa, using node-expat.",
    "devDependencies": {
        "code": "^3.0.2",
        "lab": "11.x.x"
    },
    "directories": {},
    "dist": {
        "shasum": "1d54f1d868dbbd04892b845d7cbad94c528e16e4",
        "tarball": "https://registry.npmjs.org/xml2json/-/xml2json-0.11.0.tgz"
    },
    "gitHead": "f2e88e5daa121c61024473f07c6dcf4064f503d5",
    "homepage": "https://github.com/buglabs/node-xml2json#readme",
    "license": "MIT",
    "main": "index",
    "maintainers": [
        {
            "name": "buglabs",
            "email": "camilo@buglabs.net"
        },
        {
            "name": "c4milo",
            "email": "camilo.aguilar@gmail.com"
        }
    ],
    "name": "xml2json",
    "optionalDependencies": {},
    "readme": "ERROR: No README data found!",
    "repository": {
        "type": "git",
        "url": "git://github.com/buglabs/node-xml2json.git"
    },
    "scripts": {
        "test": "lab -a code -v -t 93 test/test.js"
    },
    "version": "0.11.0"
}
```



# <a name="apidoc.tableOfContents"></a>[table of contents](#apidoc.tableOfContents)

#### [module xml2json](#apidoc.module.xml2json)
1.  [function <span class="apidocSignatureSpan">xml2json.</span>toJson (xml, _options)](#apidoc.element.xml2json.toJson)
1.  [function <span class="apidocSignatureSpan">xml2json.</span>toXml (json, options)](#apidoc.element.xml2json.toXml)
1.  object <span class="apidocSignatureSpan">xml2json.</span>sanitize

#### [module xml2json.sanitize](#apidoc.module.xml2json.sanitize)
1.  [function <span class="apidocSignatureSpan">xml2json.</span>sanitize (value, reverse)](#apidoc.element.xml2json.sanitize.sanitize)



# <a name="apidoc.module.xml2json"></a>[module xml2json](#apidoc.module.xml2json)

#### <a name="apidoc.element.xml2json.toJson"></a>[function <span class="apidocSignatureSpan">xml2json.</span>toJson (xml, _options)](#apidoc.element.xml2json.toJson)
- description and source-code
```javascript
toJson = function (xml, _options) {

    _options = _options || {};
    var parser = new expat.Parser('UTF-8');

    parser.on('startElement', startElement);
    parser.on('text', text);
    parser.on('endElement', endElement);

    obj = currentObject = {};
    ancestors = [];
    currentElementName = null;

    var schema = {
        object: joi.boolean().default(false),
        reversible: joi.boolean().default(false),
        coerce: joi.alternatives([joi.boolean(), joi.object()]).default(false),
        sanitize: joi.boolean().default(true),
        trim: joi.boolean().default(true),
        arrayNotation: joi.alternatives([joi.boolean(), joi.array()]).default(false),
        alternateTextNode: [joi.boolean().default(false), joi.string().default(false)]
    };
    var validation = joi.validate(_options, schema);
    hoek.assert(validation.error === null, validation.error);
    options = validation.value;
    options.forceArrays = {};
    if (Array.isArray(options.arrayNotation)) {
        options.arrayNotation.forEach(function(i) {
            options.forceArrays[i] = true;
        });
        options.arrayNotation = false;
    }
    if (!parser.parse(xml)) {
        throw new Error('There are errors in your xml file: ' + parser.getError());
    }

    if (options.object) {
        return obj;
    }

    var json = JSON.stringify(obj);

    //See: http://timelessrepo.com/json-isnt-a-javascript-subset
    json = json.replace(/\u2028/g, '\\u2028').replace(/\u2029/g, '\\u2029');

    return json;
}
```
- example usage
```shell
...
'''javascript
var parser = require('xml2json');

var xml = "<foo attr=\"value\">bar</foo>";
console.log("input -> %s", xml)

// xml to json
var json = parser.toJson(xml);
console.log("to json -> %s", json);

// json to xml
var xml = parser.toXml(json);
console.log("back to xml -> %s", xml)
'''
...
```

#### <a name="apidoc.element.xml2json.toXml"></a>[function <span class="apidocSignatureSpan">xml2json.</span>toXml (json, options)](#apidoc.element.xml2json.toXml)
- description and source-code
```javascript
toXml = function (json, options) {
    if (json instanceof Buffer) {
        json = json.toString();
    }

    var obj = null;
    if (typeof(json) == 'string') {
        try {
            obj = JSON.parse(json);
        } catch(e) {
            throw new Error("The JSON structure is invalid");
        }
    } else {
        obj = json;
    }
    var toXml = new ToXml(options);
    toXml.parse(obj);
    return toXml.xml;
}
```
- example usage
```shell
...
console.log("input -> %s", xml)

// xml to json
var json = parser.toJson(xml);
console.log("to json -> %s", json);

// json to xml
var xml = parser.toXml(json);
console.log("back to xml -> %s", xml)
'''

## API

'''javascript
parser.toJson(xml, options);
...
```



# <a name="apidoc.module.xml2json.sanitize"></a>[module xml2json.sanitize](#apidoc.module.xml2json.sanitize)

#### <a name="apidoc.element.xml2json.sanitize.sanitize"></a>[function <span class="apidocSignatureSpan">xml2json.</span>sanitize (value, reverse)](#apidoc.element.xml2json.sanitize.sanitize)
- description and source-code
```javascript
function sanitize(value, reverse) {
    if (typeof value !== 'string') {
        return value;
    }

    Object.keys(chars).forEach(function(key) {
        if (reverse) {
            value = value.replace(new RegExp(escapeRegExp(chars[key]), 'g'), key);
        } else {
            value = value.replace(new RegExp(escapeRegExp(key), 'g'), chars[key]);
        }
    });

    return value;
}
```
- example usage
```shell
...
ToXml.prototype.openTag = function(key) {
    this.completeTag();
    this.xml += '<' + key;
    this.tagIncomplete = true;
}
ToXml.prototype.addAttr = function(key, val) {
    if (this.options.sanitize) {
        val = sanitizer.sanitize(val)
    }
    this.xml += ' ' + key + '="' + val + '"';
}
ToXml.prototype.addTextContent = function(text) {
    this.completeTag();
    this.xml += text;
}
...
```



# misc
- this document was created with [utility2](https://github.com/kaizhu256/node-utility2)
