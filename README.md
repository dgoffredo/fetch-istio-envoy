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

    $ nl -ba envoy-file | grep -w 'set_name'
        44    span_->set_name(operation);
```
