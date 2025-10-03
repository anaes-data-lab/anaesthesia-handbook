# Hugo + Hextra Site

This site is built with [Hugo](https://gohugo.io) and uses the [Hextra](https://github.com/imfing/hextra) starter via **Hugo Modules**.  
By default, you won’t see theme files like `layouts/` or `assets/` in this repo, because Hugo pulls them in as a module during the build.

## Updating the theme

To update Hextra to the latest release:

```bash
hugo mod get -u github.com/imfing/hextra@latest
hugo mod tidy
```

This updates `go.mod` / `go.sum`.  
If you want a specific release:

```bash
hugo mod get github.com/imfing/hextra@vX.Y.Z
```

Check which version is active with:

```bash
hugo mod graph
```

## Using vendoring to inspect theme files

Sometimes you need to see the theme’s source (templates, partials, assets) so you know what you’re overriding. For that, you can **vendor** modules:

```bash
hugo mod vendor
```

This writes the resolved modules into the `_vendor/` directory, e.g.:

```
_vendor/github.com/imfing/hextra@vX.Y.Z/layouts/
_vendor/github.com/imfing/hextra@vX.Y.Z/assets/
```

### Workflow

1. Run `hugo mod vendor`.
2. Browse `_vendor/…` to find the file you want to customize.
3. Copy that file into your project, **mirroring the path** under `layouts/`, `assets/`, etc.
4. Edit only your local copy. Hugo will prefer your file over the module.
5. (Optional) Remove `_vendor/` once you’re done browsing:

   ```bash
   rm -rf _vendor/
   ```

### Notes

- If `_vendor/` is present, Hugo will build from the vendored snapshot first.  
- If you vendor multiple versions of the same module, Hugo still picks **one** (per Go’s [Minimal Version Selection](https://go.dev/ref/mod)). The extra copies are ignored.  
- Use `hugo mod graph` to confirm which version is actually active.

## Overriding files

Hugo’s lookup order means **local files take priority** over module files. Typical overrides:

- **Layouts:** `layouts/_default/baseof.html`, `layouts/partials/head.html`
- **Assets:** `assets/css/custom.scss`, `assets/js/custom.js`
- **Static:** `static/favicon.ico`, `static/icons/…`

Keep overrides minimal and mirror the module’s path exactly.

## Tips

- Clear Hugo’s resource cache if overrides don’t show:

  ```bash
  rm -rf resources/ && hugo server
  ```

- See mounts:

  ```bash
  hugo config mounts
  ```

- If you just want to inspect without vendoring, you can browse the Go module cache:

  ```bash
  go env GOMODCACHE
  ls -la $GOMODCACHE/github.com/imfing/hextra@vX.Y.Z/
  ```

---

## TL;DR

- Update Hextra: `hugo mod get -u github.com/imfing/hextra@latest`  
- Inspect files: `hugo mod vendor` → look in `_vendor/`  
- Override: copy into local `layouts/`, `assets/`, or `static/`  
- Don’t leave `_vendor/` checked in unless you want to lock builds to that snapshot
