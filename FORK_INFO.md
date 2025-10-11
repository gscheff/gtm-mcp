# GTM MCP Fork

This fork is maintained for cosmic-app integration and version pinning.

**Fork of:** https://github.com/paolobtl/gtm-mcp
**Maintained by:** gscheff for cosmic-app integration
**Author (Upstream):** Paolo Bietolini

---

## Why Fork?

Unlike our google-ads-mcp fork (which requires a patch), this gtm-mcp fork **requires no patches**. The upstream version 0.2.0 already includes customEvent trigger support.

**Reasons for forking:**
1. **Version pinning** - Control exactly which version cosmic-app uses
2. **Consistency** - Match our google-ads-mcp fork approach
3. **Team alignment** - Everyone uses the same version
4. **Future customizations** - Ready if we need custom features
5. **Stability** - Avoid unexpected breaking changes from upstream

---

## Changes from Upstream

### Currently: No Changes

The fork is currently **identical to upstream**. Version 0.2.0 includes all features we need:

- ✅ customEvent trigger support (added in v0.2.0)
- ✅ Full GTM API coverage
- ✅ OAuth authentication
- ✅ stdio transport

**No patches applied** - The upstream package works perfectly for our use case.

---

## Installation

### From pyproject.toml

```toml
[dependency-groups]
dev = [
    "gtm-mcp @ git+https://github.com/gscheff/gtm-mcp.git",
]
```

### Direct Installation

```bash
uv pip install git+https://github.com/gscheff/gtm-mcp.git
```

---

## Version History

### v0.2.0 (Current)
- **Status:** ✅ Fully functional for cosmic-app
- **Features:**
  - customEvent trigger support
  - Full CRUD operations for tags, triggers, variables
  - Container publishing
  - OAuth authentication

**Important:** Version 0.2.0 was released on Oct 7, 2025 and includes customEvent trigger support that was previously missing. Earlier versions (0.1.0) required manual patching.

---

## Syncing with Upstream

Keep the fork up-to-date with upstream changes:

```bash
# Fetch upstream changes
git fetch upstream

# Update main branch
git checkout main
git merge upstream/main
git push origin main

# Update docs branch (if needed)
git checkout docs/cosmic-app-integration
git rebase main
git push origin docs/cosmic-app-integration --force
```

---

## Testing

### Verify Installation

```bash
# Check version
uv pip show gtm-mcp

# Should show:
# Name: gtm-mcp
# Version: 0.2.0
# Location: .venv/lib/python3.13/site-packages
```

### Test customEvent Trigger Support

```bash
# Verify customEvent support in installed package
grep -A 7 'elif.*customEvent' \
  .venv/lib/python3.13/site-packages/gtm_mcp/tools.py

# Should show the customEvent handler code
```

### Test with Claude Code

```bash
# Configure in .mcp.json
{
  "mcpServers": {
    "google-tag-manager-atumka": {
      "command": "uv",
      "args": ["run", "-m", "gtm_mcp.server"],
      "env": {
        "GTM_CREDENTIALS": "/path/to/credentials.json"
      }
    }
  }
}

# Restart Claude Code
# Check for MCP tools: mcp__google-tag-manager-atumka__*
```

---

## Used By

- **cosmic-app** - Multi-brand AI astrological SaaS platform
  - AtumKa, Qoraya, Astryla brands
  - 3 MCP server instances (one per brand)
  - Configured in `.mcp.json`

---

## Comparison with google-ads-mcp Fork

| Aspect | gtm-mcp | google-ads-mcp |
|--------|---------|----------------|
| **Patches needed?** | ❌ No | ✅ Yes (stdio transport) |
| **Why fork?** | Version control | Required patch + version control |
| **Branch** | `main` or `docs/cosmic-app-integration` | `patches/stdio-transport` |
| **Maintenance** | Low (sync only) | Medium (patch + sync) |

---

## Future Enhancements

### Potential Custom Features
- [ ] Batch operations for multiple tags/triggers
- [ ] Template generation for common patterns
- [ ] Enhanced error handling
- [ ] Custom validation rules

**Note:** These are not currently needed. The upstream package fully satisfies our requirements.

---

## Submitting Changes Upstream

If we add features that benefit the community:

```bash
# Create feature branch from main
git checkout main
git checkout -b feature/batch-operations

# Make changes, commit
git add .
git commit -m "feat: Add batch operations for tags"
git push origin feature/batch-operations

# Submit PR to upstream: https://github.com/paolobtl/gtm-mcp
```

---

## Documentation References

### cosmic-app Documentation
- **GTM Setup:** `docs/setup/GTM_SETUP_GUIDE.md`
- **GTM Configuration:** `docs/setup/GTM_CONFIGURATION_STATUS.md`
- **GTM Triggers:** `docs/setup/GTM_TRIGGER_FIX_SOLUTION.md`
- **GA4 Reference:** `docs/setup/GA4_QUICK_REFERENCE.md`

### Upstream Documentation
- **GitHub:** https://github.com/paolobtl/gtm-mcp
- **PyPI:** https://pypi.org/project/gtm-mcp/

---

## Contact

For questions about this fork:
- **Project:** cosmic-app
- **Repository:** https://github.com/gscheff/cosmic-app

For questions about the original package:
- **Upstream:** https://github.com/paolobtl/gtm-mcp
- **Author:** Paolo Bietolini
- **Issues:** https://github.com/paolobtl/gtm-mcp/issues

---

## Fork Maintenance Status

- ✅ **Active** - Maintained for cosmic-app
- 🔄 **Synced** - Regularly updated from upstream
- 📦 **No patches** - Uses upstream code as-is
- 🎯 **Purpose** - Version control and team consistency

---

**Last Updated:** 2025-10-11
**Fork Created:** 2025-10-11
**Current Version:** 0.2.0 (matching upstream)
**Patches Applied:** None (upstream v0.2.0 is fully functional)
