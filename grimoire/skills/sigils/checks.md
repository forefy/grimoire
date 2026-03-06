The checks skill enables pure agentic flaw detection.

## File Structure

```markdown
---
name: debug assertions
description: Flags security critical debug assertions which should be regular assertions.
severity-default: low
tools: [Grep, Read]
---

Look for these patterns:

- debug_assert!(...);
  
Not all debug_assert! usage is problematic.
```




## Resources
* ampcode.com/manual#code-review