# mosh-static

An unofficial static binary distribution of [mobile-shell/mosh](https://github.com/mobile-shell/mosh) which can be dropped into a remote server. Extends [dtinth/mosh-static](https://github.com/dtinth/mosh-static) to add support for arm64 and armv7 architectures on Linux, using QEMU when building, and also support for macOS on arm64 and x86_64.

Releasing is automated by GitHub Actions on [this GitHub Actions workflow](https://github.com/freitas-renato/mosh-static-multiarch/blob/main/.github/workflows/autobuild.yml). For Linux, it clones `mosh`’s source code, compiles it inside an Alpine Docker image with QEMU for arm64, armv7 and amd64 targets. For macOS, it installs dependencies via Macports and compiles inside a macOS environment, linking protobuf statically. In the end, it creates a [GitHub release](https://github.com/freitas-renato/mosh-static-multiarch/releases/latest) with `.tar.gz` bundles for each supported platform and architecture.

Each bundle contains:

- `mosh`
- `mosh-client`
- `mosh-server`

`mosh` is the normal entrypoint on the client side. It is the upstream wrapper script that starts `mosh-server` and then invokes `mosh-client`.

## Using the bundle

```sh
# On the server (ARM64)
curl -LO https://github.com/freitas-renato/mosh-static-multiarch/releases/latest/download/mosh-1.4.0-linux-arm64.tar.gz
tar -xzf mosh-1.4.0-linux-arm64.tar.gz

# On the client
mosh --server="$PWD/mosh-1.4.0-linux-arm64/mosh-server" <username>@<hostname>
```
