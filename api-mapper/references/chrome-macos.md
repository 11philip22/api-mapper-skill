# Chrome Detection -- macOS

```bash
CHROME=$(ls "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" 2>/dev/null \
  || ls "$HOME/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" 2>/dev/null \
  || which chromium 2>/dev/null \
  || echo "NOT FOUND")
echo "Chrome: $CHROME"
```
