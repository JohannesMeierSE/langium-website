---
title: "3. Write the grammar"
weight: 400
url: /docs/learn/workflow/write_grammar
---
Your Langium project is now setup and ready to be used. The next step is to define the grammar of your language. The grammar is the most important part of your language definition. It defines the syntax of your language and how the language elements are structured.

The grammar is defined in a `.langium` file. Make sure that you have installed the VS Code extension for Langium. This extension provides syntax highlighting and code completion for `.langium` files. Here's the grammar from the Hello-World example that was generated by the Yeoman generator:

```langium
grammar HelloWorld

hidden terminal WS: /\s+/;
terminal ID: /[_a-zA-Z][\w]*/;

entry Model: (persons+=Person | greetings+=Greeting)*;

Person:
    'person' name=ID;

Greeting:
    'Hello' person=[Person] '!';
```

Let's go through this one by one:

```langium
grammar HelloWorld
```

Before we tell Langium anything about our grammar contents, we first need to give it a name - in this case it's `HelloWorld`. The `langium-cli` will pick this up to prefix any generated services with this name.

```langium
hidden terminal WS: /\s+/;
terminal ID: /[_a-zA-Z][\w]*/;
```

Here we define our two needed terminals for this grammar: The whitespace `WS` and identifier `ID` terminals. Terminals parse a part of our document by matching it against their regular expression. The `WS` terminal parses any whitespace characters with the regex `/\s+/`. This allows us to consume whitespaces in our document. As the terminal is declared as `hidden`, the parser will parse any whitespace and discard the results. That way, we don't have to care about how many whitespaces a user uses in their document. Secondly, we define our `ID` terminal. It parses any string that starts with an underscore or letter and continues with any amount of characters that match the `\w` regex token. It will match `Alice`, `_alice`, or `_al1c3` but not `4lice` or `#alice`. Langium is using the JS regex dialect for terminal definitions.

```langium
entry Model: (persons+=Person | greetings+=Greeting)*;
```

The `Model` parser rule is the `entry` point to our grammar. Parsing always starts with the `entry` rule. Here we define a repeating group of alternatives: `persons+=Person | greetings+=Greeting`. This will always try to parse either a `Person` or a `Greeting` and add it to the respective list of `persons` or `greetings` in the `Model` object. Since the alternative is wrapped in a repeating group `*`, the parser will continue until all input has been consumed. 

```langium
Person: 'person' name=ID;
```

The `Person` rule starts off with the `'person'` keyword. Keywords are like terminals, in the sense that they parse a part of the document. The set of keywords and terminals create the tokens that your language is able to parse. You can imagine that the `'person'` keyword here is like an indicator to tell the parser that an object of type `Person` should be parsed. After the keyword, we assign the `Person` a name by parsing an `ID`.

```langium
Greeting: 'Hello' person=[Person] '!';
```

Like the previous rule, the `Greeting` starts with a keyword. With the `person` assignment we introduce the _cross reference_, indicated by the brackets `[]`. A cross reference will allow your grammar to reference other elements that are contained in your file or workspace. By default, Langium will try to resolve this cross reference by parsing the terminal that is associated with its `name` property. In this case, we are looking for a `Person` whose `name` property matches the parsed `ID`.

That finishes the short introduction to Langium! Feel free to play around with the grammar and use `npm run langium:generate` to regenerate the generated TypeScript files. To go further, we suggest that you continue with our [tutorials](/docs/learn/minilogo/).
