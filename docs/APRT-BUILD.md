# APRT Windows x64 and Linux x64 builds

Run **Build APRT Windows and Linux x64** from the fork's Actions tab.
The workflow is manual only and uploads build artifacts; it does not publish
GitHub releases or modify the upstream release workflows.

The source checkout is pinned to RustDesk 1.5.3, commit
`95cca3f661f635b3cc85c04935fe8d96d8f432ef`, including its submodules.
The recipes and toolchain versions come from that release's Flutter workflow.
Only the native Windows x64 and Linux x64 targets and their prerequisites run.

Each source checkout is patched with these compiled defaults:

- ID server: `rustdesk.aprt.info`, standard port 21116.
- Public key: `Hag3BO1Z4qjSWIstKuNueXedZPL9CKJ4FWbxFC4nfRQ=`.
- Relay: advertised by the ID server; hbbr is assumed to use the same host.
- No Server Pro API address or account credentials are embedded.

Existing client settings can override compiled defaults. Test on a clean
profile and check Network settings on upgraded installations.

Download artifacts after a successful run:

- `rustdesk-aprt-windows-x64`: self-extracting unsigned EXE.
- `rustdesk-unsigned-windows-x86_64`: unpacked Windows application bundle;
  keep the executable, DLLs and data together.
- `rustdesk-aprt-linux-x64`: DEB and RPM packages.

Unsigned Windows binaries may trigger SmartScreen. No signing certificate is
required to build. No unattended access password is embedded.

Before distributing, verify ID registration and direct and forced-relay
connections between two machines. A successful build alone does not verify the
server, its public key, firewall configuration, or OS-specific remote control.
The generated packages are not installed by this workflow.

Regression surface: existing repository files and upstream workflows are
unchanged. The custom workflow patches only `RENDEZVOUS_SERVERS` and `RS_PUB_KEY`
in its temporary hbb_common checkout, which changes the default registration
server and the server signature verification key for these custom binaries.
