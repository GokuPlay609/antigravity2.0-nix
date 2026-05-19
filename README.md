# antigravity2.0-nix

Nix flake for **Google Antigravity 2.0** — the agent-first development platform announced at Google I/O 2026.

Packages both the **desktop app** (Antigravity Hub) and the **CLI** (`agy`), which are not yet available in nixpkgs.

> **Note:** This flake targets Antigravity 2.0, a complete redesign of the product. It is unrelated to the `antigravity` package currently in nixpkgs (which packages the older IDE-based v1.x).

---

## Packages

| Attribute | Binary | Description | Architectures |
|---|---|---|---|
| `antigravity-cli` | `agy` | Terminal-first AI coding agent | `x86_64-linux` `aarch64-linux` |
| `antigravity-desktop` | `antigravity` | Antigravity 2.0 desktop app (Electron) | `x86_64-linux` |
| `default` | `agy` | Alias for `antigravity-cli` | `x86_64-linux` `aarch64-linux` |

> ARM64 support for the desktop app is on Google's roadmap. The CLI already supports aarch64.

---

## Usage

### Standalone (try without installing)

```bash
# Run the CLI
nix run github:briossant/antigravity2.0-nix#antigravity-cli

# Run the desktop app
nix run github:briossant/antigravity2.0-nix#antigravity-desktop
```

### NixOS / Home Manager

Add the flake as an input:

```nix
# flake.nix
inputs = {
  antigravity2.url = "github:briossant/antigravity2.0-nix";
  antigravity2.inputs.nixpkgs.follows = "nixpkgs";
};
```

Then use the packages in your config:

```nix
# In a NixOS module or home-manager module
{ inputs, pkgs, system, ... }:
{
  home.packages = [
    inputs.antigravity2.packages.${system}.antigravity-cli
    inputs.antigravity2.packages.${system}.antigravity-desktop
  ];
}
```

### nix profile (imperative)

```bash
nix profile install github:briossant/antigravity2.0-nix#antigravity-cli
nix profile install github:briossant/antigravity2.0-nix#antigravity-desktop
```

---

## Notes

- The desktop app requires `--no-sandbox` (handled automatically) since NixOS does not set up the Chrome setuid sandbox by default.
- Both packages are proprietary software (`allowUnfree = true` is set automatically within the flake).
- The CLI binary is distributed by Google as `antigravity` but is exposed here as `agy` to avoid conflicting with the desktop app in `$PATH`.

---

## Versioning

| Package | Version |
|---|---|
| Antigravity CLI | 1.0.0 |
| Antigravity Desktop | 2.0.0 |

Version bumps are tracked manually for now. PRs welcome.

---

## License

MIT — see [LICENSE](./LICENSE).

This flake is not affiliated with or endorsed by Google. Google Antigravity is proprietary software owned by Google LLC.
