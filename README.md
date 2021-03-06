# fmt - Command Line Output Formatting #

Note: this library is considered stable. It hasn't been changed much since 2012 yet it is still being used across the
npm ecosystem. If you don't see any updates, it isn't because it is unmaintained but because it just works. Raise an
issue for bug fixes or feature requests.

## Example ##

```
const fmt = require('fmt');

fmt.sep();
fmt.title('The Truth about Cats');
fmt.field('Name', 'Felix');
fmt.field('Description', 'Regal. Mysterious with a wise look in her eyes.');
fmt.field('Legs', 4);
fmt.title('The Truth about Dogs');
fmt.field('Name', 'Fido');
fmt.field('Description', 'Bouncy. Out there, shiny, long brown coat.');
fmt.field('Legs', 2);
fmt.sep();
fmt.title('A List');
fmt.li('item 1');
fmt.li('the second item');
fmt.li('the third and final item');
fmt.separator();
```

Has the output:

```
===============================================================================
--- The Truth about Cats ------------------------------------------------------
Name                 : Felix
Description          : Regal. Mysterious with a wise look in her eyes.
Legs                 : 4
--- The Truth about Dogs ------------------------------------------------------
Name                 : Fido
Description          : Bouncy. Out there, shiny, long brown coat.
Legs                 : 2
===============================================================================
--- A List --------------------------------------------------------------------
* item 1
* the second item
* the third and final item
===============================================================================
```

Can also be used with [chalk]() and all indentation will still work fine:

```
const fmt = require('fmt');
const chalk = require('chalk');

fmt.sep();
fmt.title(chalk.magenta('The Truth about Cats'));
fmt.field(chalk.green('Name'), chalk.yellow('Felix'));
fmt.sep();
```

## Usage ##

### fmt.separator() (alias: fmt.sep()) ###

Makes a double line on the screen which is 79 chars long.

e.g.

```
fmt.separator();
fmt.sep();

->

===============================================================================
```

### fmt.line() ###

Makes a line on the screen which is 79 chars long.

e.g.

```
fmt.line()

->

-------------------------------------------------------------------------------
```

### fmt.title(title) ###

Renders a title with three '-', then the title, then more (until the 79th char).

e.g.

```
fmt.title('The Truth about Cats');

->

--- The Truth about Cats ------------------------------------------------------
```

### fmt.field(key, value) ###

Renders a line with a key and then the value, but with the key padded to 20 chars so that each field lines up.

```
fmt.field('Name', 'Fido');
fmt.field('Description', 'Bouncy. Out there, shiny, long brown coat.');
fmt.field('Legs', 2);

->

Name                 : Fido
Description          : Bouncy. Out there, shiny, long brown coat.
Legs                 : 2
```

### fmt.subfield(key, value) ###

Renders a line with a key (preceded by '- ') and then the value, but with the key padded to 20 chars so that each field
lines up. This can be helpful when you have a field, then other fields related to that one.

e.g.

```
fs.stat(__filename, function(err, stats) {
    fmt.field('File', __filename);
    fmt.subfield('size', stats.size);
    fmt.subfield('uid', stats.uid);
    fmt.subfield('gid', stats.gid);
    fmt.subfield('ino', stats.ino);
    fmt.subfield('ctime', stats.ctime);
    fmt.subfield('mtime', stats.mtime);
});

->

File                 : /home/user/path/to/cats-and-dogs.js
- size               : 1003
- uid                : 1000
- gid                : 1000
- ino                : 17567406
- ctime              : Sun Aug 19 2012 17:08:52 GMT+1200 (NZST)
- mtime              : Sun Aug 19 2012 17:08:52 GMT+1200 (NZST)
```

### fmt.li(msg) ###

Prints the msg preceded with a '* ', so that it looks like a list.

e.g.

```
fmt.title('A List');
fmt.li('item 1');
fmt.li('the second item');
fmt.li('the third and final item');
fmt.line()

->

--- A List --------------------------------------------------------------------
* item 1
* the second item
* the third and final item
-------------------------------------------------------------------------------
```

### fmt.dump(data[, name]) ###

Prints a dump of the data, with the optional name beforehand. This is basically a shortcut for :
console.log(util.inspect(data, false, null, true));

e.g.

```
fs.stat(__filename, function(err, stats) {
    fmt.separator();
    fmt.title('Dump (with name)');
    fmt.dump(stats, 'stats');
    fmt.separator();
    fmt.title('Dump');
    fmt.dump(stats);
    fmt.separator();
});

->

===============================================================================
--- Dump (with name) ----------------------------------------------------------
stats : { dev: 2049,
  ino: 17567406,
  mode: 33188,
  nlink: 1,
  uid: 1000,
  gid: 1000,
  rdev: 0,
  size: 1025,
  blksize: 4096,
  blocks: 8,
  atime: Sun Aug 19 2012 17:28:54 GMT+1200 (NZST),
  mtime: Sun Aug 19 2012 17:28:51 GMT+1200 (NZST),
  ctime: Sun Aug 19 2012 17:28:51 GMT+1200 (NZST) }
===============================================================================
--- Dump ----------------------------------------------------------------------
{ dev: 2049,
  ino: 17567406,
  mode: 33188,
  nlink: 1,
  uid: 1000,
  gid: 1000,
  rdev: 0,
  size: 1025,
  blksize: 4096,
  blocks: 8,
  atime: Sun Aug 19 2012 17:28:54 GMT+1200 (NZST),
  mtime: Sun Aug 19 2012 17:28:51 GMT+1200 (NZST),
  ctime: Sun Aug 19 2012 17:28:51 GMT+1200 (NZST) }
===============================================================================
```

### fmt.msg(msg) ###

Prints the msg as-is! :)

e.g.

```
fmt.title('Example');
fmt.msg('Output as-is!');
fmt.line();

->

--- Example -------------------------------------------------------------------
Output as-is!
-------------------------------------------------------------------------------
```

## Example 2 ##

You can also do a Heroku build style output, such as the following:

```
const fmt = require('../fmt.js');

fmt.arrow('Deploying ...');
fmt.indent('Found 20 files:');
fmt.spacer();

const files = [
    'filename1.txt',
    'doc.doc',
    'image.jpg',
    'document.pdf',
]
for(let i = 0; i < files.length; i++) {
    fmt.li(files[i], true);
}

fmt.spacer();

fmt.msg('Quote:', true);
fmt.quoteblock(
  [
    'Another quote by someone famous',
    'who lived in the past, but the',
    'quote is about the internet.',
    ' - Abe Lincoln',
  ].join('\n'),
  true
);

fmt.spacer();

fmt.arrow('Building ...');
fmt.indent('Package is 32,001 bytes.');

fmt.spacer();

fmt.arrow('Finished');
```

Has the output:

```
----->  Deploying ...
        Found 20 files:

        * filename1.txt
        * doc.doc
        * image.jpg
        * document.pdf

        Quote:
        | Another quote by someone famous
        | who lived in the past, but the
        | quote is about the internet.
        |  - Abe Lincoln

----->  Building ...
        Package is 32,001 bytes.

----->  Finished
```

# Author #

|               | Website                                     | Twitter                                               |
|:-------------:|:-------------------------------------------:|:-----------------------------------------------------:|
| Author        | [Andrew Chilton](https://chilts.org/)       | [@andychilton](https://twitter.com/andychilton)       |
| Company       | [AppsAttic](https://appsattic.com/)         | [@AppsAttic](https://twitter.com/AppsAttic)           |

# License #

* [Copyright 2012-2018 Andrew Chilton. All rights reserved.](http://chilts.mit-license.org/2012/)

(Ends)
