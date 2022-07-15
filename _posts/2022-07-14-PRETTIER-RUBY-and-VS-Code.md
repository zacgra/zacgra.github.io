---
title: "Prettier, Ruby, and VS Code"
author: zacgra
layout: post
categories: Code
tags: [ruby, config, vscode]
---

I recently got back in to Ruby/Rails after spending a while in JS land. One of the biggest things I immediately started missing was the Prettier extension's formatting that worked so quickly and seamlessly with JS. The only problem is that Prettier doesn't come with Ruby support built-in. (keyword 'built-in', but we'll get back to that)

## Other Ruby Formatters

My first approach was Googling "VS Code + Ruby + formatting" and all results pointed to either RuboCop or Solargraph. After going through the install of RuboCop, I was pretty underwhelmed by the lag in formatting on save. Between the long (5+ seconds?) delay and the overly prescriptive linter, RuboCop was out. Next up, Solargraph. Or maybe not. Much as I tried, I wasn't able to get it off the ground. I suspect this had as much to do with me misunderstanding Solargraph as a language server as it did with my config.

## Prettier's Ruby Plugin

There has got to be a way to get Prettier...oh wait? What's this? https://github.com/prettier/plugin-ruby Prettier's Ruby plugin? How did I miss this? No worries, let's just install it real quick and...

```console
    Error:
    We failed to spawn our parser server. Please report this error on GitHub
    at https://github.com/prettier/plugin-ruby.
```

### Gem Install

So, there are some dependencies, which I thought I installed correctly. Nope. The docs present two paths, one using just a gem, and the other an npm package + gem dependencies. No go on the single gem. While I could get it going on the command line, I just got the `We failed to spawn our parser server.` when triggering on save in VS Code.

### NPM Install

On to the npm option. I installed the npm package and gems in my project's root directory, but still had the same issue. Much googling later, and it turns out the npm + gems need to be installed in the .vscode extension directory. For my version, this was `~/.vscode/extensions/esbenp.prettier-vscode-9.5.0`. After quitting and re-opening VS Code, it worked!

## Prettier Plugin Installation Steps

For a more detailed wrap-up, here's what worked for me:

- install the language server VS Code extension: [rebornix.Ruby](https://marketplace.visualstudio.com/items?itemName=rebornix.Ruby)
- install the formatter's VS Code extension: [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- install the Prettier plugin for Ruby: [plugin-ruby](https://github.com/prettier/plugin-ruby)
  - go to the Prettier extensions .vscode directory
  - install the npm package with `npm install --save-dev prettier @prettier/plugin-ruby`
  - install the gem dependencies: `gem install bundler prettier_print syntax_tree syntax_tree-haml syntax_tree-rbs`

## Now For Ludicrous Speed!

I probably lost about 5 hours in at this point, between the various forays in to RuboCop and Solargraph. The format on save had gone from...let's say four seconds with RuboCop to maybe half-a-second with Prettier. All I need to do is save 5,142 times before I'll have made up my time. ;)
