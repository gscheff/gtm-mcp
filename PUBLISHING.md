# Publishing Guide

## Step 1: Update Package Information

Edit `pyproject.toml` and update:

1. **Author information** (line 12):
   ```toml
   authors = [
       {name = "Paolo YourLastName", email = "your.email@example.com"}
   ]
   ```

2. **GitHub repository URLs** (lines 36-38):
   ```toml
   [project.urls]
   Homepage = "https://github.com/YOUR_USERNAME/gtm-mcp"
   Repository = "https://github.com/YOUR_USERNAME/gtm-mcp"
   Issues = "https://github.com/YOUR_USERNAME/gtm-mcp/issues"
   ```

   Or if you don't have a GitHub repo yet, you can remove these lines entirely.

## Step 2: Create PyPI Account

1. Go to https://pypi.org/account/register/
2. Create an account
3. Verify your email

## Step 3: Create API Token

1. Go to https://pypi.org/manage/account/token/
2. Click "Add API token"
3. Token name: "gtm-mcp-upload"
4. Scope: "Entire account" (or create project first)
5. Copy the token (starts with `pypi-`)

## Step 4: Build the Package

```bash
cd /home/paolo/Documents/coding/mcp/gtm-mcp

# Install build tools
pip install --upgrade build twine

# Build the package
python3 -m build
```

This creates:
- `dist/gtm_mcp-0.1.0-py3-none-any.whl` (wheel)
- `dist/gtm-mcp-0.1.0.tar.gz` (source distribution)

## Step 5: Upload to PyPI

```bash
# Upload using your API token
python3 -m twine upload dist/*
```

When prompted:
- Username: `__token__`
- Password: `pypi-YOUR_TOKEN_HERE`

## Step 6: Test Installation

After successful upload:

```bash
# In a new terminal/environment
pip install gtm-mcp

# Test it works
gtm-mcp --help  # Should show no errors
```

## Step 7: Configure in Claude Desktop

Users can now install with:
```bash
pip install gtm-mcp
```

And configure Claude Desktop with:
```json
{
  "mcpServers": {
    "gtm": {
      "command": "gtm-mcp"
    }
  }
}
```

## Optional: Test with TestPyPI First

Before publishing to the real PyPI, you can test with TestPyPI:

1. Create account at https://test.pypi.org/account/register/
2. Create API token at https://test.pypi.org/manage/account/token/
3. Upload:
   ```bash
   python3 -m twine upload --repository testpypi dist/*
   ```
4. Test install:
   ```bash
   pip install --index-url https://test.pypi.org/simple/ gtm-mcp
   ```

## Updating Your Package

To release a new version:

1. Update `version = "0.1.0"` in `pyproject.toml` (e.g., to `0.1.1`)
2. Remove old builds: `rm -rf dist/`
3. Build again: `python3 -m build`
4. Upload: `python3 -m twine upload dist/*`

## Troubleshooting

### "File already exists"
- You can't re-upload the same version
- Increment version number in `pyproject.toml`

### "Invalid token"
- Make sure username is `__token__` (with double underscores)
- Copy the entire token including `pypi-` prefix

### Package name taken
- Change `name = "gtm-mcp"` to something unique like `name = "gtm-mcp-paolo"`
