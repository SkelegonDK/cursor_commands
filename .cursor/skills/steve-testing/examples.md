# Steve Testing Examples

Common test scenarios and patterns for macOS and iOS Simulator testing with Steve.

## macOS Application Examples

### Example 1: Settings App Navigation

```bash
# Launch Settings
steve launch "com.apple.systempreferences" --wait

# Wait for Settings window
steve wait --text "Settings" --timeout 5

# Click on a preference pane
steve click --text "Privacy & Security"

# Wait for pane to load
steve wait --text "Privacy & Security" --timeout 5

# Click on a specific setting
steve click --text "Accessibility"

# Verify
steve assert --text "Accessibility" --exists
```

### Example 2: Login Flow

```bash
# Focus app
steve focus "MyApp"

# Wait for login screen
steve wait --text "Email" --timeout 10

# Fill email
steve click --text "Email"
steve type "user@example.com" --delay 50

# Fill password
steve key tab
steve type "password123" --delay 50

# Click login button
steve click --text "Log In"

# Wait for dashboard
steve wait --text "Dashboard" --timeout 10

# Verify login success
steve assert --text "Welcome" --exists
steve screenshot --app "MyApp" -o login-success.png
```

### Example 3: Form Submission with Validation

```bash
# Wait for form
steve wait --text "Contact Form" --timeout 5

# Fill required fields
steve click --text "Name"
steve type "John Doe" --delay 50
steve key tab

steve type "john@example.com" --delay 50
steve key tab

steve type "This is a test message" --delay 50

# Submit form
steve click --text "Submit"

# Wait for success message
steve wait --text "Thank you" --timeout 5

# Or handle error
if steve exists --text "Error"; then
  steve screenshot --app "MyApp" -o form-error.png
  exit 1
fi
```

### Example 4: Menu Interaction

```bash
# Focus app
steve focus "TextEdit"

# Open File menu
steve menu "File" "New"

# Or find menu by text
steve menu --contains "New"

# List menu items
steve menu --list "File"
```

### Example 5: Window Management

```bash
# List all windows
steve windows --format json

# Focus specific window
steve window focus "ax://win/123"

# Resize window
steve window resize "ax://win/123" 1024 768

# Move window
steve window move "ax://win/123" 100 100
```

## iOS Simulator Examples

### Example 1: Launch iOS App in Simulator

```bash
# Boot simulator (if not already running)
xcrun simctl boot "iPhone 15 Pro" 2>/dev/null || true

# Wait a moment for simulator to be ready
sleep 2

# Launch iOS app by bundle ID
steve launch "com.example.myapp" --wait

# Wait for app UI
steve wait --text "Welcome" --timeout 15

# Verify app is running
steve assert --text "Welcome" --exists
```

### Example 2: iOS App Login Flow

```bash
# Ensure simulator is booted
xcrun simctl boot "iPhone 15 Pro" 2>/dev/null || true
sleep 2

# Launch app
steve launch "com.example.myapp" --wait

# Wait for login screen
steve wait --text "Email" --timeout 15

# Tap email field (iOS uses tap, but Steve uses click)
steve click --text "Email"
steve type "user@example.com" --delay 100

# Tap password field
steve click --text "Password"
steve type "password123" --delay 100

# Tap login button
steve click --text "Log In"

# Wait for home screen
steve wait --text "Home" --timeout 15

# Verify
steve assert --text "Home" --exists
steve screenshot --app "MyApp" -o ios-login-success.png
```

### Example 3: iOS Navigation Flow

```bash
# Launch app
steve launch "com.example.myapp" --wait
steve wait --text "Home" --timeout 15

# Navigate to settings
steve click --text "Settings"

# Wait for settings screen
steve wait --text "Settings" --timeout 10

# Change a setting
steve click --text "Notifications"
steve click --text "Enable Notifications"

# Go back
steve key escape

# Verify back on settings
steve assert --text "Settings" --exists
```

### Example 4: iOS Form Filling

```bash
# Launch app
steve launch "com.example.myapp" --wait

# Wait for form
steve wait --text "Sign Up" --timeout 15

# Fill form fields
steve click --text "First Name"
steve type "Jane" --delay 100

steve click --text "Last Name"
steve type "Smith" --delay 100

steve click --text "Email"
steve type "jane@example.com" --delay 100

# Scroll if needed
steve scroll down --amount 3

# Fill remaining fields
steve click --text "Phone"
steve type "5551234567" --delay 100

# Submit
steve click --text "Create Account"

# Wait for confirmation
steve wait --text "Account Created" --timeout 15
```

## Advanced Patterns

### Example 1: Retry Logic

```bash
# Retry clicking with exponential backoff
click_with_retry() {
  local text=$1
  local max_attempts=3
  local attempt=1
  
  while [ $attempt -le $max_attempts ]; do
    if steve click --text "$text"; then
      return 0
    fi
    echo "Attempt $attempt failed, retrying..."
    sleep $((attempt * 2))
    attempt=$((attempt + 1))
  done
  
  echo "Failed to click $text after $max_attempts attempts"
  return 1
}

click_with_retry "Submit"
```

### Example 2: Wait for Multiple Conditions

```bash
# Wait for either success or error
wait_for_result() {
  local timeout=30
  local elapsed=0
  
  while [ $elapsed -lt $timeout ]; do
    if steve exists --text "Success"; then
      return 0
    fi
    if steve exists --text "Error"; then
      return 1
    fi
    sleep 1
    elapsed=$((elapsed + 1))
  done
  
  return 2  # Timeout
}

wait_for_result
```

### Example 3: Element Discovery Workflow

```bash
# Discover all buttons in a window
steve elements --window "Settings" --format json | \
  jq '.data[] | select(.role == "AXButton") | {title: .title, id: .id}'

# Find element by multiple criteria
steve find --role AXButton --title "OK" --window "Dialog"

# Find element containing text
steve find --text "Save" --ancestor-role AXRow
```

### Example 4: Screenshot Workflow

```bash
# Capture full screen
steve screenshot -o fullscreen.png

# Capture app window
steve screenshot --app "MyApp" -o app-window.png

# Capture specific element
ELEMENT_ID=$(steve find --text "Dashboard" --format json | jq -r '.data.id')
steve screenshot --element "$ELEMENT_ID" -o dashboard.png
```

### Example 5: Complete Test Script

```bash
#!/bin/bash
set -e

APP_BUNDLE="com.example.myapp"
TEST_NAME="login_flow"

echo "Starting test: $TEST_NAME"

# Launch app
echo "Launching app..."
steve launch "$APP_BUNDLE" --wait

# Wait for login screen
echo "Waiting for login screen..."
steve wait --text "Email" --timeout 15

# Fill credentials
echo "Filling credentials..."
steve click --text "Email"
steve type "test@example.com" --delay 50

steve click --text "Password"
steve type "testpass123" --delay 50

# Submit
echo "Submitting login..."
steve click --text "Log In"

# Verify success
echo "Verifying login..."
steve wait --text "Dashboard" --timeout 15
steve assert --text "Dashboard" --exists

# Capture evidence
echo "Capturing screenshot..."
steve screenshot --app "MyApp" -o "${TEST_NAME}_success.png"

echo "Test passed: $TEST_NAME"
```

## Common Issues and Solutions

### Issue: Element Not Found

**Solution**: Use broader text matching or check window context

```bash
# Instead of exact match
steve click --text "Submit Button"

# Use partial match
steve click --text "Submit"

# Or scope to window
steve click --window "Dialog" --text "OK"
```

### Issue: Timing Issues

**Solution**: Always wait for elements before interacting

```bash
# Bad: Immediate click
steve click --text "Button"

# Good: Wait first
steve wait --text "Button" --timeout 10
steve click --text "Button"
```

### Issue: iOS Simulator Not Responding

**Solution**: Ensure simulator is booted and app is launched

```bash
# Boot simulator
xcrun simctl boot "iPhone 15 Pro"

# Wait for simulator to be ready
sleep 3

# Launch app
steve launch "com.example.app" --wait
```

### Issue: Permission Denied

**Solution**: Grant Accessibility permissions

1. Open System Settings
2. Go to Privacy & Security > Accessibility
3. Add Terminal (or your shell) to allowed apps
4. Restart terminal
