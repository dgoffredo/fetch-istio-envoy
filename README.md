```console
$ bin/fetch-istio-envoy --help

fetch-istio-envoy - Retrieve a file from Envoy as used by Istio.

usage:

    fetch-istio-envoy ISTIO_REF [ENVOY_PATH]

        Create a temporary directory and fill it with information
        about the relationship between the specified ISTIO_REF
        (which can be a branch, tag, or commit hash) and the underlying
        Envoy code that the Istio uses.

        Optionally specify an ENVOY_PATH, which is an unqualified path
        to a file within the relevant Envoy git tree, to download
        from GitHub as the file `envoy-file` in the temporary directory.
        If ENVOY_PATH is not specified, then it defaults to
        source/extensions/tracers/datadog/span.cc.

        Print the path to the temporary directory to standard output.

example:

    $ fetch-istio-envoy 1.20.0 source/extensions/tracers/datadog/span.cc
    /tmp/tmp.A7FFDYU52Z

    $ cd /tmp/tmp.A7FFDYU52Z

    $ ls -l
    -rw-rw-r-- 1 david david   41 Dec  6 20:04 envoy-commit
    -rw-rw-r-- 1 david david   83 Dec  6 20:04 envoy-commit.url
    -rw-rw-r-- 1 david david 2992 Dec  6 20:04 envoy-file
    -rw-rw-r-- 1 david david  123 Dec  6 20:04 envoy-file.url
    -rw-rw-r-- 1 david david   42 Dec  6 20:04 envoy-path
    -rw-rw-r-- 1 david david   81 Dec  6 20:04 envoy-tree.url
    -rw-rw-r-- 1 david david  335 Dec  6 20:04 istio-deps
    -rw-rw-r-- 1 david david    7 Dec  6 20:04 istio-ref
    -rw-rw-r-- 1 david david 1881 Dec  6 20:04 Makefile
    -rw-rw-r-- 1 david david   41 Dec  6 20:04 proxy-tree
    -rw-rw-r-- 1 david david   76 Dec  6 20:04 proxy-tree.url
    -rw-rw-r-- 1 david david 4101 Dec  6 20:04 proxy-workspace

    $ nl -ba envoy-file | grep -w 'set_name'
        44    span_->set_name(operation);
```
