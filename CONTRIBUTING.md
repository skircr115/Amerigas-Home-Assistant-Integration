# Contributing to AmeriGas Home Assistant Integration

Thank you for your interest in contributing! This document provides guidelines for contributing to this project.

## Table of Contents

- [Code of Conduct](#code-of-conduct)
- [How Can I Contribute?](#how-can-i-contribute)
- [Development Setup](#development-setup)
- [Submission Guidelines](#submission-guidelines)
- [Coding Standards](#coding-standards)
- [Testing](#testing)

---

## Code of Conduct

### Our Pledge

We are committed to providing a welcoming and inspiring community for all.

### Our Standards

**Examples of behavior that contributes to creating a positive environment:**
- Using welcoming and inclusive language
- Being respectful of differing viewpoints
- Gracefully accepting constructive criticism
- Focusing on what is best for the community
- Showing empathy towards other community members

**Examples of unacceptable behavior:**
- Trolling, insulting/derogatory comments, and personal attacks
- Public or private harassment
- Publishing others' private information without explicit permission
- Other conduct which could reasonably be considered inappropriate

---

## How Can I Contribute?

### Reporting Bugs

**Before submitting a bug report:**
- Check the [Troubleshooting Guide](TROUBLESHOOTING.md)
- Search [existing issues](https://github.com/skircr115/ha-amerigas/issues) to see if it's already reported
- Test with the latest version

**When submitting a bug report, include:**
1. Home Assistant version
2. Pyscript version
3. Full error message from logs
4. Steps to reproduce
5. Expected vs actual behavior
6. Screenshots if applicable

**Example bug report:**
```markdown
**Environment:**
- HA Version: 2024.1.0
- Pyscript Version: 1.5.0
- Integration Version: 2.0.0

**Bug Description:**
Tank level sensor shows unavailable after update

**Steps to Reproduce:**
1. Update integration to 2.0.0
2. Restart HA
3. Check sensor.amerigas_tank_level

**Expected:** Sensor shows tank percentage
**Actual:** Sensor shows "unavailable"

**Logs:**
```
[Paste relevant logs here]
```

**Screenshots:**
[If applicable]
```

### Suggesting Enhancements

**Enhancement suggestions are welcome!**

**Before suggesting:**
- Check if it's already suggested in [Discussions](https://github.com/skircr115/ha-amerigas/discussions)
- Consider if it fits the project scope

**When suggesting, include:**
1. **Clear description** of the enhancement
2. **Use case** - why is this needed?
3. **Examples** - how would it work?
4. **Alternatives considered**

### Contributing Code

**Types of contributions we're looking for:**
- 🐛 Bug fixes
- ✨ New features
- 📝 Documentation improvements
- 🎨 Dashboard card examples
- 🔔 Automation examples
- 🧪 Tests
- 🌐 Translations (if applicable)

---

## Development Setup

### Prerequisites

- Home Assistant development environment
- Python 3.11+
- Git
- Text editor or IDE

### Fork and Clone

```bash
# Fork the repository on GitHub, then:
git clone https://github.com/YOUR_USERNAME/ha-amerigas.git
cd ha-amerigas

# Add upstream remote
git remote add upstream https://github.com/skircr115/ha-amerigas.git
```

### Set Up Development Environment

```bash
# Copy files to your HA test instance
cp pyscript/amerigas.py /config/pyscript/
cp template_sensors.yaml /config/
cp utility_meter.yaml /config/

# Configure test credentials
nano /config/configuration.yaml
```

### Testing Your Changes

1. Make changes to the code
2. Copy to HA test instance
3. Reload Pyscript:
   ```
   Developer Tools → YAML → Reload "Pyscript Python scripting"
   ```
4. Test manually:
   ```
   Developer Tools → Services → pyscript.amerigas_update
   ```
5. Check logs for errors
6. Verify sensor values
7. Test automations

---

## Submission Guidelines

### Pull Request Process

1. **Create a branch** for your changes
   ```bash
   git checkout -b feature/your-feature-name
   ```

2. **Make your changes**
   - Follow coding standards (below)
   - Add/update documentation
   - Test thoroughly

3. **Commit your changes**
   ```bash
   git add .
   git commit -m "feat: add tank monitoring alerts"
   ```

4. **Push to your fork**
   ```bash
   git push origin feature/your-feature-name
   ```

5. **Open a Pull Request**
   - Go to GitHub
   - Click "New Pull Request"
   - Fill out the template
   - Link related issues

### Commit Message Guidelines

Use [Conventional Commits](https://www.conventionalcommits.org/):

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation changes
- `style`: Code style changes (formatting)
- `refactor`: Code refactoring
- `test`: Adding tests
- `chore`: Maintenance tasks

**Examples:**
```
feat(sensors): add next delivery date sensor

fix(login): handle special characters in password

docs(readme): update installation instructions

style(pyscript): format code according to PEP 8
```

### Pull Request Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Documentation update
- [ ] Code refactoring

## Testing
- [ ] Tested in development environment
- [ ] All sensors work correctly
- [ ] No errors in logs
- [ ] Automations tested (if applicable)

## Checklist
- [ ] Code follows project style guidelines
- [ ] Documentation updated
- [ ] No breaking changes (or documented)
- [ ] Commit messages follow guidelines

## Related Issues
Fixes #123
Relates to #456
```

---

## Coding Standards

### Python (Pyscript)

**Follow PEP 8:**
- 4 spaces for indentation (no tabs)
- Max line length: 100 characters
- Two blank lines between functions
- Descriptive variable names

**Good:**
```python
async def amerigas_update():
    """Update all AmeriGas sensors."""
    if not USERNAME or not PASSWORD:
        log.error("AmeriGas: Credentials not configured")
        return
    
    tank_level = safe_int(account_data.get('ForecastTankLevel'), 0)
```

**Bad:**
```python
async def update():
    if not user or not pwd:
        log.error("No creds")
        return
    lvl=account_data.get('ForecastTankLevel')
```

**Error Handling:**
```python
# Always use try/except for external calls
try:
    response = await session.post(url, data=data)
except aiohttp.ClientError as e:
    log.error(f"Network error: {e}")
    return
```

**Logging:**
```python
# Use appropriate log levels
log.debug("Detailed debug info")
log.info("Important status updates")
log.warning("Warning messages")
log.error("Error messages")
```

### YAML

**Template Sensors:**
```yaml
# Use consistent formatting
- sensor:
    - name: "Sensor Name"
      unique_id: sensor_unique_id
      unit_of_measurement: "unit"
      state_class: measurement
      icon: mdi:icon-name
      state: >
        {% set variable = states('sensor.source') | float(0) %}
        {{ variable | round(2) }}
```

**Indentation:**
- 2 spaces per level
- No tabs
- Consistent alignment

### Documentation

**Code Comments:**
```python
# Good: Explain why, not what
# Base64 encode credentials (required by AmeriGas API)
encoded_email = base64.b64encode(username.encode()).decode()

# Bad: State the obvious
# Encode the email
encoded_email = base64.b64encode(username.encode()).decode()
```

**Docstrings:**
```python
async def amerigas_update():
    """
    Update all AmeriGas sensors by scraping the customer portal.
    
    This service can be called from automations or manually.
    It logs into MyAmeriGas, fetches account data, and updates
    all 15 sensors with current information.
    """
```

---

## Testing

### Manual Testing Checklist

Before submitting a PR, verify:

**Installation:**
- [ ] Fresh install works
- [ ] Upgrade from previous version works
- [ ] All files are included

**Sensors:**
- [ ] All 15 AmeriGas sensors populate
- [ ] All template sensors work
- [ ] Utility meters create successfully
- [ ] No "unavailable" sensors (except next_delivery if applicable)

**Functionality:**
- [ ] Login successful
- [ ] Data updates correctly
- [ ] Manual update works
- [ ] Automatic updates work (wait 6+ hours or modify cron)

**Error Handling:**
- [ ] Wrong password handled gracefully
- [ ] Network error handled
- [ ] Missing data handled (doesn't crash)

**Energy Dashboard:**
- [ ] Gas source appears in picker
- [ ] Data shows correctly
- [ ] Cost tracking works (if enabled)

**Logs:**
- [ ] No Python errors
- [ ] No template errors
- [ ] Info messages appear
- [ ] Warnings are appropriate

### Test Environments

**Minimum test:**
- Home Assistant Core 2023.1+
- Pyscript latest version
- Fresh configuration

**Ideal test:**
- Multiple HA versions
- Different account types (auto-pay, will-call)
- With and without tank monitor

---

## Documentation Guidelines

### README Updates

When adding features, update:
- Feature list
- Sensor table
- Configuration examples
- Dashboard cards (if applicable)

### Code Documentation

**Inline comments** for complex logic:
```python
# AmeriGas returns dates in MM/DD/YY format
# Convert to ISO format for HA timestamp sensors
if '/' in date_string:
    parsed = dt.strptime(date_string, '%m/%d/%y')
```

**Function docstrings** for all public functions:
```python
def safe_float(value, default=0.0):
    """
    Safely convert value to float.
    
    Args:
        value: Value to convert (str, int, float, or None)
        default: Default value if conversion fails
        
    Returns:
        float: Converted value or default
    """
```

---

## Review Process

### What We Look For

**✅ Good PRs:**
- Clear description
- Single purpose/feature
- Well-tested
- Documented
- Follows coding standards
- No breaking changes (or clearly documented)

**❌ PRs That Need Work:**
- No description
- Multiple unrelated changes
- Breaks existing functionality
- Missing documentation
- Not tested
- Style violations

### Timeline

- We aim to review PRs within **1 week**
- Complex changes may take longer
- Questions/feedback will be provided
- Multiple review rounds may be needed

### Getting Your PR Merged

1. **Address feedback** promptly
2. **Keep PR updated** with main branch
3. **Be patient** - quality over speed
4. **Be responsive** to questions
5. **Be respectful** in discussions

---

## Community

### Where to Ask Questions

- 💬 [GitHub Discussions](https://github.com/skircr115/ha-amerigas/discussions) - General questions
- 🐛 [GitHub Issues](https://github.com/skircr115/ha-amerigas/issues) - Bug reports
- 🏠 [Home Assistant Forum](https://community.home-assistant.io/) - HA-specific questions

### Getting Help

**Stuck on something?**
1. Check documentation
2. Search existing issues
3. Ask in Discussions
4. Be patient - we're volunteers!

---

## Recognition

### Contributors

All contributors will be:
- Listed in CONTRIBUTORS.md
- Mentioned in release notes
- Appreciated in README

### Hall of Fame

Major contributors may be:
- Given collaborator access
- Invited to project discussions
- Recognized in special thanks

---

## License

By contributing, you agree that your contributions will be licensed under the MIT License.

---

## Questions?

Don't hesitate to ask! We're here to help.

**Contact:**
- Open a [Discussion](https://github.com/skircr115/ha-amerigas/discussions)
- Comment on an existing [Issue](https://github.com/skircr115/ha-amerigas/issues)
- Reach out to maintainers

---

**Thank you for contributing! 🎉**

**[← Back to README](README.md)**
