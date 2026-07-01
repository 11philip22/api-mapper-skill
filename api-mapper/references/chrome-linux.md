# Chrome Detection -- Linux

```bash
CHROME=$(which google-chrome-stable \
  || which google-chrome \
  || which chromium-browser \
  || which chromium \
  || echo "NOT FOUND")
echo "Chrome: $CHROME"
```
