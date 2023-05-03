# README

## Blueprint

Blueprint is an experimental spec + toolchain to define the organization, structure, and layout of docs. You can use blueprint to scaffold, migrate, analyze, and validate documentation. 

Use blueprint to create **schemas** that describe the structure of common doc types like reference docs, how tos, discussions, and tutorials. 
You can use schemas to create new docs as well as validate that existing docs conform to the given schema. 

Blueprint is meant to work with any language via plugins and will come with built-in support for github flavored markdown.

## Audience

Blueprint is currently meant for anyone that works with markdown flavored docs. 

## Usage

### 1. Initialize your blueprint
> NOTE: the following are meant to give you an idea of what you can do with blueprint. the methods are not imlemented yet

Assuming you are starting a new project
```sh
blueprint init
```

This creates the following files:
```
- blueprint.config.yml
- schemas/
    - readme.yml
```


- `blueprint.config.yml` defines basic configuration of your project
```yml
{
  "organization": "dendron",
  "version": "0.0.1"
}
```

- `readme.yml` defines the layout of your readme. note that this README is based on the readme guide from [The Good Docs Project](https://gitlab.com/tgdp/templates.git)
```yml
title: README
description: schema for the readme
# this defines variables that are used in the schema
# these are passed in via a config or CLI flags at runtime
variables:
  projectName: 
    type: string
    docs: value of project
# this defines what sections are available in the readme doc
sections:
  projectName:
    # we expect the header to exactly match what is passed in 
    value: {{projectName}}
    # this will be added as a comment in the generated doc
    docs: |
        Include the project URL and project owner name underneath the project name if applicable.
  projectDescription:
    # this tells blueprint not to generate a new section header 
    header: false
    docs: |
        The README template guide includes information on how to write a project description and a project description. Here are some examples of effective phrases for describing a project.

        With _{Project Name}_ you can _{verb}_ _{noun}_...

        _{Project Name}_ helps you _{verb}_ _{noun}_...

        Unlike _{alternative}_, _{Project Name}_ _{verb}_ _{noun}_...

        {Include screenshots and/or demo videos if applicable}
  gettingStarted:
    docs: |
        Get started with {Project Name} by {write the first step a user needs to start using the project. Use a verb to start.}.
  contributing:
    value: Contributing Guidelines
required: [projectName, gettingStarted]
```

### 2. Generate your blueprint
You can then use this tmeplate using `blueprint generate`
```sh
blueprint generate readme --variables projectName="Tiny Garden" > README.md
```

This will create the follwoing file at `README.md`
```md
# README

## Tiny Garden
<!-- 
{Include the project URL and project owner name underneath the project name if applicable.} 
-->

<!-- SECTION:Project description-->
<!--
{The README template guide includes information on how to write a project description and a project description. Here are some examples of effective phrases for describing a project.}

With _{Project Name}_ you can _{verb}_ _{noun}_...

_{Project Name}_ helps you _{verb}_ _{noun}_...

Unlike _{alternative}_, _{Project Name}_ _{verb}_ _{noun}_...

{Include screenshots and/or demo videos if applicable} 
-->

## Getting Started
<!-- 
Get started with {Project Name} by {write the first step a user needs to start using the project. Use a verb to start.}. 
-->

## Contributing guidelines
```

### 3. Run your first validation

You can validate your blueprint using

```sh
blueprint validate readme --variables projectName="Tiny Garden" README.md
```

```
 FAIL readme
  README.md
    ✓ projectName
    ✕ projectDescription
    ✕ gettingStarted
    ✕ contributing

  ● projectDescription 
    expect NOT_EMPTY
  ● gettingStarted 
    expect NOT_EMPTY
  ● contributing 
    expect NOT_EMPTY

Validations: 1 passed, 4 total
Time:        0.166 s, estimated 1 s
Ran all validations 
```

### 4. Fix issues and run again

TODO