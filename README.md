# QVO API Documentation

This is the repository for the QVO API Reference. A fork from the awesome [Slate](https://lord.github.io/slate) project, check it out!

## Getting Started

### Prerequisites

You're going to need:

 - **Linux or OS X** — Windows may work, but is unsupported.
 - **Ruby, version 2.2.5 or newer**
 - **Bundler** — If Ruby is already installed, but the `bundle` command doesn't work, just run `gem install bundler` in a terminal.

### Getting Set Up

1. Clone the repo with `git clone https://github.com/qvo-team/qvo-docs.git`
2. `cd qvo-docs`
3. Initialize and start Slate. You can either do this locally, or with Vagrant:

```bash
# either run this to run locally
bundle install
bundle exec middleman server

# OR run this to run with vagrant
vagrant up
```

You can now see the docs at http://localhost:4567. Whoa! That was fast!

Now that Slate is all set up on your machine, you'll probably want to learn more about [editing Slate markdown](https://github.com/lord/slate/wiki/Markdown-Syntax), or [how to publish your docs](https://github.com/lord/slate/wiki/Deploying-Slate).

If you'd prefer to use Docker, instructions are available [in the wiki](https://github.com/lord/slate/wiki/Docker).


### Deploying

Just run

```bash
bundle exec middleman build --clean
```

And copy the contents of the `build/` folder into any static site hosting and you're done!

Need Help? Found a bug?
--------------------

[Submit an issue](https://github.com/qvo-team/qvo-docs/issues) if you need any help. And, of course, feel free to submit pull requests with bug fixes or changes.
