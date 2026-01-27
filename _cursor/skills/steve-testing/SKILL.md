---
name: steve-testing
description: Automates testing of macOS applications and iOS Simulator apps using Steve CLI via Accessibility API. Use when writing automated tests, UI automation scripts, or testing macOS/iOS applications.
---

# Steve Automated Testing

Automate macOS and iOS Simulator applications using Steve CLI. Steve uses the macOS Accessibility API to discover and interact with UI elements.

## Installation

Install via Homebrew:

```bash
brew tap mikker/tap
brew install steve
```

**Required permissions**: Grant Accessibility permissions in System Settings > Privacy & Security > Accessibility.

## Output Format

Always use `--format json` (or `-j`) for programmatic access:

```bash
steve apps --format json
# {"ok":true,"data":[...]}
```

Errors go to stderr with non-zero exit codes.

## Application Control

### List and Focus Apps

```bash
steve apps                    # List running apps
steve focus "AppName"        # Focus by name
steve focus --pid 1234       # Focus by process ID
steve focus --bundle "com.example.app"  # Focus by bundle ID
```

### Launch and Quit

```bash
steve launch "com.example.app" --wait  # Launch and wait for ready
steve quit "AppName" --force            # Force quit
```

### iOS Simulator Apps

For iOS Simulator, use the simulator's bundle ID:

```bash
# Boot simulator first (if needed)
xcrun simctl boot "iPhone 15 Pro"

# Launch iOS app
steve launch "com.example.iosapp" --wait

# Or focus if already running
steve focus --bundle "com.example.iosapp"
```

## Element Discovery

### Find Elements

```bash
steve find "Button"                          # By role
steve find --title "Submit"                 # By title
steve find --text "Dictation Mode"          # By visible text (case-insensitive)
steve find --role AXButton --title "OK"     # Combined criteria
steve find --identifier "loginButton"       # By accessibility identifier
steve element-at 100 200                    # Element at coordinates
```

### List Elements

```bash
steve elements                              # All elements
steve elements --depth 5                    # Limit depth
steve elements --window "Settings"          # Scope to window
```

### Reliability Patterns

- `--text` matches via `AXValue`, `AXDescription`, and `AXStaticText` (case-insensitive substring)
- `--window "Title"` scopes searches to specific windows
- `--ancestor-role AXRow|AXCell|AXButton --click` clicks nearest ancestor after text match

## Interactions

### Clicking

```bash
steve click "ax://1234/0.2.5"                    # By element ID
steve click --title "Submit"                     # By title
steve click --text "Dictation Mode"              # By text
steve click --window "Settings" --text "Save"    # Scoped to window
steve click-at 100 200 --double                 # Coordinate click (double)
```

### Typing and Keys

```bash
steve type "hello world" --delay 50              # Type with delay (ms)
steve key cmd+shift+p                            # Keyboard shortcuts
steve key f12                                    # Function keys
steve key fn+f12                                 # With fn modifier
steve key --raw 122                              # Raw key code
steve set-value "ax://1234/0.1" "new text"      # Set value directly
```

### Scrolling

```bash
steve scroll down --amount 5                     # Scroll down
steve scroll --element "ax://1234/0.4" up        # Scroll specific element
```

## Assertions

### Existence Checks

```bash
steve exists --title "Welcome"                  # Check if exists
steve exists --text "Ready" --window "Settings" # Scoped check
```

### Waiting

```bash
steve wait --title "Results" --timeout 5         # Wait for element
steve wait --title "Loading..." --gone --timeout 10  # Wait for disappearance
steve wait --text "Loading..." --window "Settings" --timeout 10
```

### Assertions

```bash
steve assert --title "Submit" --enabled          # Check enabled state
steve assert --title "Checkbox" --checked        # Check checked state
steve assert --title "Input" --value "expected text"  # Check value
```

## Windows

```bash
steve windows                                    # List windows
steve window focus "ax://win/123"               # Focus window
steve window resize "ax://win/123" 800 600      # Resize
steve window move "ax://win/123" 100 100        # Move
```

## Menus

```bash
steve menus                                      # List menus
steve menu "File" "New"                         # Click menu item
steve menu --contains --case-insensitive "settings..."  # Find by text
steve menu --list "File"                        # List menu items
steve statusbar --list                          # List status bar items
steve statusbar "Wi-Fi"                         # Click status bar item
```

## Screenshots

```bash
steve screenshot                                 # Full screen (PNG to stdout)
steve screenshot --app "AppName" -o screenshot.png  # App window
steve screenshot --element "ax://1234/0.2" -o element.png  # Element
```

## Global Options

```bash
--app "Name"        # Target app by name
--pid 1234          # Target app by PID
--bundle "id"       # Target app by bundle ID
--timeout 5         # Operation timeout (seconds)
--verbose           # Verbose output
--quiet             # Quiet mode
--format text|json  # Output format
-j                  # Short for --format json
```

## Exit Codes

| Code | Meaning |
|------|---------|
| 0 | Success |
| 1 | Element not found |
| 2 | App not found / not running |
| 3 | Timeout |
| 4 | Permission denied (accessibility) |
| 5 | Invalid arguments |

## Testing Workflows

### Basic Test Pattern

1. **Launch/Focus app**
   ```bash
   steve launch "com.example.app" --wait
   ```

2. **Wait for UI ready**
   ```bash
   steve wait --text "Ready" --timeout 10
   ```

3. **Interact**
   ```bash
   steve click --text "Login"
   steve type "username" --delay 50
   ```

4. **Assert**
   ```bash
   steve assert --text "Welcome" --exists
   ```

5. **Capture evidence**
   ```bash
   steve screenshot --app "AppName" -o test-result.png
   ```

### Form Filling Pattern

```bash
# Wait for form
steve wait --text "Email" --timeout 5

# Fill fields
steve click --text "Email"
steve type "user@example.com" --delay 50
steve key tab
steve type "password123" --delay 50

# Submit
steve click --text "Submit"

# Verify
steve wait --text "Success" --timeout 5
steve assert --text "Success" --exists
```

### iOS Simulator Workflow

```bash
# Boot simulator (if needed)
xcrun simctl boot "iPhone 15 Pro"

# Launch app
steve launch "com.example.iosapp" --wait

# Wait for app UI
steve wait --text "Welcome" --timeout 10

# Interact
steve click --text "Get Started"
steve type "test@example.com" --delay 50

# Verify
steve assert --text "Logged In" --exists
```

## Error Handling

### Check Exit Codes

Always check exit codes after operations:

```bash
steve click --text "Button"
if [ $? -ne 0 ]; then
  echo "Button not found"
fi
```

### Retry Pattern

For flaky operations, implement retries:

```bash
for i in {1..3}; do
  steve click --text "Submit" && break
  sleep 1
done
```

### Timeout Handling

Use appropriate timeouts for async operations:

```bash
steve wait --text "Loading..." --gone --timeout 30
```

## Best Practices

1. **Use JSON format** for programmatic access: `--format json`
2. **Scope searches** with `--window` when multiple windows exist
3. **Use text matching** for better reliability than coordinates
4. **Wait before interactions** to ensure UI is ready
5. **Use ancestor-role clicking** when text is in a container
6. **Capture screenshots** for debugging and evidence
7. **Check exit codes** to handle errors gracefully

## Notes

- Coordinates are in screen space (0,0 = top-left of main display)
- Element IDs are stable within a session but not across app restarts
- Text matching is case-insensitive substring matching
- iOS Simulator apps require the simulator to be booted first

## Additional Resources

- For detailed examples, see [examples.md](examples.md)
- Steve repository: https://github.com/mikker/steve
