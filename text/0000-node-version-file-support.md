- Feature Name: node-version-file-support
- Start Date: 2023-04-24
- RFC PR: (leave this empty)
- Volta Issue: (leave this empty)

# Summary

[summary]: #summary

This RFC proposes adding support for loading and pinning a project's Node version to the widely used `.node-version` file. Supporting `.node-version` makes it easier to maintain consistency across different environments and among team members who may use other Node version managers.

# Motivation

[motivation]: #motivation

Today, [many tools exist](https://github.com/shadowspawn/node-version-usage#supporting-products) to manage what version of Node a project should use. On projects worked on by many people, such as popular open source projects, configuring each of these tools is cumbersome. Fortunately, many of these tools support the `.node-version` file, which is a simple text file that contains the Node version to use.

However, Volta is notably missing from the [list of projects that support `.node-version`](https://github.com/shadowspawn/node-version-usage#compatibility-testing). Adding support for this file will make it easier for users to adopt Volta on projects that already use the `.node-version` file.

By accepting and implementing this RFC, users of Volta will be able to easily work in projects that use the `.node-version` file without having to ask maintainers of large projects to specially configure Volta.

# Pedagogy

[pedagogy]: #pedagogy

- [ ] Teach pinning node versions in `.node-version` files
- [ ] Teach pinning other tools in `package.json`
- [ ] New thinking: Specification precedence

<!-- How should we explain and teach this feature to users? How should users understand this feature? This generally means:

- Introducing new named concepts.
- Explaining the feature largely in terms of examples.
- Explaining how users should _think_ about the feature, and how it should impact the way they use Volta. It should explain the impact as concretely as possible.
- If applicable, describe the differences between teaching this to existing Node programmers and new Node programmers.

It is not necessary to write the actual feature documentation in this section, but it should establish the concepts on which the documentation would be built. -->

# Details

[details]: #details

- [ ] Backwards compatiable
- [ ] Node loading precedence: `package.json` > `.node-version` > `global` (i.e. tool specific config takes precedence over general config)
  - i.e. if `package.json` does not specify a Node version, then we'll look for a `.node-version` file in the project root
- [ ] Node pinning precedence: `.node-version` > `package.json` > `global`
- [ ] Warn if both `.node-version` and `package.json` configuration for Node are present?
- [ ] Note: Project root is still the directory with a `package.json`. This is the same as the current behavior. It's just when resolving the node version, we look for `.node-version` in the project root.
- [ ] If volta config in `package.json` is extended and no Node version is specified, then we'll look for a Node version in the parent configuration.
  - [ ] Should we look for a `.node-version` file before going to the parent `package.json` configuration? Should we allow local `.node-version` files to override parent `package.json` configurations and possibly parent `.node-version` files?

<!-- This is the technical portion of the RFC. Explain the design in sufficient detail that:

- Its interaction with other features is clear.
- It is reasonably clear how the feature would be implemented.
- Corner cases are dissected by example.

This section should return to the examples given in the previous section, and explain more fully how the detailed proposal makes those examples work. -->

# Critique

[critique]: #critique

- [ ] Add critique for how this introduces multiple configurations to configure Volta
- [ ] Possibly more file system operations to support backwards compatibility

<!-- This section discusses the tradeoffs considered in this proposal. This must include an honest accounting of the drawbacks of the proposal, as well as list out all the alternatives that were considered and rationale for why this proposal was chosen over the alternatives. This section should address questions such as:

- Why is this design the best in the space of possible designs?
- What other designs have been considered and what is the rationale for not choosing them?
- What is the impact of not doing this? -->

# Unresolved questions

[unresolved]: #unresolved-questions

<!-- - What parts of the design do you expect to resolve through the RFC process before this gets merged?
- What parts of the design do you expect to resolve through the implementation of this feature before stabilization?
- What related issues do you consider out of scope for this RFC that could be addressed in the future independently of the solution that comes out of this RFC? -->

- [ ] Out of scope: How do we handle the case where a user has `.node-version` in their home directory. Could come later.
- [ ] Out of scope? Changing workspace support for tools other than Node
- [ ] Think through workspaces and how they should interact with this feature
  - [ ] How do we handle the case where `package.json` doesn't have a `volta` field and there is no `.node-version` file?
  - [ ] One workaround: symlink `.node-version` from the root of the workspace to the workspace's `package.json` directory
- [ ] We should handle the case where `.node-version` is a symlink
