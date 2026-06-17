# Personal agent skills

## `grill-with-code`

Based on [`grill-me` by Matt Pocock](https://github.com/mattpocock/skills/tree/main/skills/productivity/grill-me)

Use this skill when you want to write code collaboratively and iteratively with an agent.

In addition to what `grill-me` provides, the agent:

- scaffold APIs, interfaces, modules, folders, stubs, or focused tests as decisions crystallize
- keep changes additive by default, avoiding destructive edits or broad refactors
- preserve existing code while drafting a parallel implementation or next-step design
- writes the implementation as decisions get settled

The skill is intentionally collaborative. It assumes you may be editing code at the same time, so it rereads files before touching them and avoids overwriting fresh changes.
