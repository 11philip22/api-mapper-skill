# Chrome Executable Path — Linux

## Common install locations (try in order)

```
/usr/bin/google-chrome
/usr/bin/google-chrome-stable
/usr/bin/chromium
/usr/bin/chromium-browser
/snap/bin/chromium
```

## Auto-detect via shell

```bash
CHROME=$(which google-chrome-stable \
  || which google-chrome \
  || which chromium-browser \
  || which chromium \
  || echo "NOT FOUND")
echo "Chrome: $CHROME"
```
