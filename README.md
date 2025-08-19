# glance fork, preserves (some) comments

[![Package Version](https://img.shields.io/hexpm/v/glance)](https://hex.pm/packages/glance)
[![Hex Docs](https://img.shields.io/badge/hex-docs-ffaff3)](https://hexdocs.pm/glance/)

A Gleam source code parser, in Gleam!

Glance attempts to be permissive with regards to what it will parse and so it is
able to parse some code that the Gleam compiler will reject as invalid.

> [!IMPORTANT]
> This fork preserves simple comments (//) and puts them into the next
> successfully parsed structure.
>
> This patch allows you to use comments as meaningful pieces of code.
> Whether it's a good programming practice, I will leave up to users to decide.
>
> See example below for some details.

## Usage

Add the package to your Gleam project:

```sh
gleam add glance
```

> [!IMPORTANT]
> Since it's a fork, use path to the git repo directly.

Then get parsing!

```gleam
import glance
import gleam/io

const code = "
  /// Cardinal is a type

  // hey it's a comment
  /// doc comment in the middle
  // another line
  pub type Cardinal {
    // this a comment for the North variant
    North
    East
    South
    West
  }
"

pub fn main() {
  let assert Ok(parsed) = glance.module(code)
  echo parsed.custom_types
}
```

This program print this to the console:

```gleam
[
  Definition(
    [],
    CustomType(
      name: "Cardinal",
      publicity: Public,
      parameters: [],
      variants: [
        Variant("North", [], [], [" this a comment for the North variant"]),
        Variant("East", [], [], []),
        Variant("South", [], [], []),
        Variant("West", [], [], []),
      ],
      comments: [
        " hey it's a comment",
        " another line asht"
      ],
      comments_doc: [
        " Cardinal is a type"
        " doc comment in the middle",
      ]
    ),
  ),
]
```

API documentation can be found at <https://hexdocs.pm/glance>.
