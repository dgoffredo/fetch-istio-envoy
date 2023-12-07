# Required files:
#
# - "istio-ref" is a file containing the istioproxy/istio repo branch/tag/commit.
# - "envoy-path" is a file containing the path to a file in Envoy's source tree.
#
# Required command line tools:
#
# - curl
# - jq
# - sed

.PHONY: all
all: envoy-file.url envoy-file envoy-tree.url envoy-commit.url proxy-tree.url

envoy-file.url: envoy-path envoy-commit
	>$@ printf 'https://github.com/envoyproxy/envoy/blob/%s/%s' "$$(cat envoy-commit)" "$$(cat envoy-path)"

envoy-file: envoy-path envoy-commit
	printf 'https://raw.githubusercontent.com/envoyproxy/envoy/%s/%s' "$$(cat envoy-commit)" "$$(cat envoy-path)" | xargs curl --silent >$@

envoy-tree.url: envoy-commit
	>$@ printf 'https://github.com/envoyproxy/envoy/tree/%s' "$$(cat $<)"

envoy-commit.url: envoy-commit
	>$@ printf 'https://github.com/envoyproxy/envoy/commit/%s' "$$(cat $<)"

envoy-commit: proxy-workspace
	>$@ <$< sed -n 's/^\s*ENVOY_SHA\s*=\s*"\([0-9a-fA-F]\+\)".*/\1/p'

proxy-workspace.url: proxy-tree
	>$@ printf 'https://raw.githubusercontent.com/istio/proxy/%s/WORKSPACE' "$$(cat $<)"

proxy-workspace: proxy-tree
	printf 'https://raw.githubusercontent.com/istio/proxy/%s/WORKSPACE' "$$(cat $<)" | xargs curl --silent >$@

proxy-tree.url: proxy-tree
	>$@ printf 'https://github.com/istio/proxy/tree/%s' "$$(cat $<)"

proxy-tree: istio-deps
	>$@ <$< jq --raw-output 'map(select(.name == "PROXY_REPO_SHA"))[0].lastStableSHA'

istio-deps: istio-ref
	printf 'https://raw.githubusercontent.com/istio/istio/%s/istio.deps' "$$(cat istio-ref)" | xargs curl --silent >$@

istio-deps.url: istio-ref
	>$@ printf 'https://github.com/istio/istio/blob/%s/istio.deps\n' "$$(cat istio-ref)"

istio-ref.url: istio-ref
	>$@ printf 'https://github.com/istio/istio/tree/%s\n' "$$(cat istio-ref)"

.DELETE_ON_ERROR:

.PHONY: clean
clean:
	rm -f istio-ref.url istio-deps* proxy-tree* proxy-workspace* envoy-commit* envoy-tree* envoy-file*
