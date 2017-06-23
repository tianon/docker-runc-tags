# wut???

See https://github.com/tianon/docker-runc-tags/tags.

# how?

## 17.06 and newer

https://github.com/docker/docker/releases

Choose a version:

```console
$ git ls-remote --tags https://github.com/docker/docker-ce.git 'refs/tags/v*' \
	| cut -d/ -f3 \
	| cut -d'^' -f1 \
	| sort -uV
```

Get the relevant commit:

```console
$ version='v17.06.0-ce-rc5'
$ wget -qO- "https://github.com/docker/docker-ce/raw/$version/components/engine/hack/dockerfile/binaries-commits" \
	| grep -E '^RUNC_COMMIT=' \
	| cut -d= -f2
```

## 17.05 and older

https://github.com/docker/docker/releases

Choose a version:

```console
$ git ls-remote --tags https://github.com/docker/docker.git 'refs/tags/v*' \
	| cut -d/ -f3 \
	| cut -d'^' -f1 \
	| sort -uV
```

Get the relevant commit:

```console
$ version='v17.05.0-ce'
$ wget -qO- "https://github.com/docker/docker/raw/$version/hack/dockerfile/binaries-commits" \
	| grep -E '^RUNC_COMMIT=' \
	| cut -d= -f2
```

## making the tag

Get the commit object:

```console
$ commit='9df8b306d01f59d3a8029be411de015b7304dd8f'
$ git fetch --no-tags https://github.com/opencontainers/runc.git refs/heads/*:refs/remotes/oci-runc/*
$ git fetch --no-tags https://github.com/docker/runc.git refs/heads/*:refs/remotes/docker-runc/*
$ git show "$commit" # to verify it actually exists
```

Push it!

```console
$ tag="${version#v}"; tag="${tag//-ce/}"; tag="docker-$tag"
$ echo "$tag"
$ git push origin "$commit":"refs/tags/$tag"
```
