# robię brwi

Landing page for **robię brwi** (Wiktoria Jackowska): eyebrow lamination, tinting, mapping and shaping. It's a single static page linking to Instagram, Booksy, Google Maps and the contact email.

## Structure

```
www/
  index.html    # the whole page: markup, inline CSS, inline SVG icons
  favicon.svg
.github/workflows/deploy.yml
```

There's no build step and no dependencies. Edit `www/index.html` directly.

## Local preview

```sh
python3 -m http.server -d www 8000
```

Then open http://localhost:8000.

## Deployment

Every push to `main` triggers the **Deploy** GitHub Actions workflow, which uploads the contents of `www/` to the server over SFTP (`lftp mirror --reverse`). You can also run it manually from the Actions tab.

> Files removed or renamed in `www/` are **not** deleted from the server, because the mirror runs without `--delete`. Clean them up manually.

### Required configuration

| Name              | Type     | Purpose                                          |
| ----------------- | -------- | ------------------------------------------------ |
| `FTP_SERVER`      | secret   | SFTP host                                        |
| `FTP_USERNAME`    | secret   | SFTP user                                        |
| `FTP_PASSWORD`    | secret   | SFTP password                                    |
| `SSH_KNOWN_HOSTS` | secret   | Host key line(s), e.g. from `ssh-keyscan <host>` |
| `FTP_SERVER_DIR`  | variable | Target directory on the server                   |
