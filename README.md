[![Build Status](https://travis-ci.org/estan/protoc-gen-doc.svg?branch=master)](https://travis-ci.org/estan/protoc-gen-doc)

# Google Protocol Buffers<br>Documentation Generator

This is a documentation generator plugin for the Google Protocol
Buffers compiler (`protoc`). The plugin can generate HTML, DocBook
or Markdown documentation from comments in your `.proto` files.

## Installing the Plugin

### Linux
Package repositories for Ubuntu, openSUSE, Fedora and Arch are
available at the [Open Build Service][obs]. For CentOS 7 there's
an x64 RPM in the [Releases](https://github.com/estan/protoc-gen-doc/releases)
section (it requires Qt and protobuf packages from the [EPEL repository](https://fedoraproject.org/wiki/EPEL)).

### Windows
Use the standalone ZIP distribution available in the
[Releases](https://github.com/estan/protoc-gen-doc/releases) section.

### From Source
See the build instructions in [BUILDING.md](BUILDING.md).

## Invoking the Plugin

The plugin is invoked by passing the `--doc_out` option to the
`protoc` compiler. The option has the following format:

    --doc_out=docbook|html|markdown|<TEMPLATE_FILENAME>,<OUT_FILENAME>:<OUT_DIR>

The format may be one of the built-in ones ( `docbook`, `html` or
`markdown`) or the name of a file containing a custom
[Mustache][mustache] template. For example, to generate HTML
documentation for all `.proto` files in the `proto` directory into
`doc/index.html`, type:

    protoc --doc_out=html,index.html:doc proto/*.proto

The plugin executable must be in `PATH` or specified explicitly
using the `--plugin` option in order for `protoc` to find it. If
you need support for a custom output format, see the built-in
templates in [templates](templates) for how to write your
own. If you just want to customize the look of the HTML output,
put your CSS in `stylesheet.css` next to the output file and it
will be picked up.

## Writing Documentation

Use `/** */` or `///` comments to document your files. Comments
for enumerations and messages go before the message/enumeration
definition. Comments for fields or enum values can go either
before or after the field/value definition. If a documentation
comment begins with `@exclude`, the message, enum, enum value or
field will be excluded from the generated documentation.

## Using with the universe

The magic incantation for one of the projects:

```bash
~/bin/protob/bin/protoc --doc_out=html,index.html:/home/tjhunter/tmp/doc -I ./api/src/main/protobuf/ -I ./api/target/protobuf_external/ ./api/src/main/protobuf/cluster/*.proto
```

NOTE: you need a recent version of protobuf (> 2.6). The version on `inception` is not recent enough and you will need to compile your own version.



## Output Example

With the input `.proto` files

* [Booking.proto](examples/proto/Booking.proto)
* [Customer.proto](examples/proto/Customer.proto)
* [Vehicle.proto](examples/proto/Vehicle.proto)

the plugin gives the output

* [Markdown](examples/doc/example.md)
* [HTML][html_preview]
* [DocBook](examples/doc/example.docbook)
* [PDF](examples/doc/example.pdf?raw=true) (Using [Apache FOP][fop])

Look in [examples/Makefile](examples/Makefile) to see how these
outputs were built.

[mustache]: http://mustache.github.io/ "Mustache - Logic-less templates"
[fop]: http://xmlgraphics.apache.org/fop/ "Apache™ FOP (Formatting Objects Processor)"
[html_preview]: https://rawgit.com/estan/protoc-gen-doc/master/examples/doc/example.html "HTML Example Output"
[obs]: https://software.opensuse.org/download.html?project=home%3Aestan%3Aprotoc-gen-doc&package=protoc-gen-doc
