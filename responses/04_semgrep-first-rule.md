# Writing Our First Rule

## Semgrep Playground Overview

For these exercises we're going to be using the Semgrep playground: https://semgrep.dev/editor, as it's a convenient way to iterate on rules right from your browser, without installing anything.

If you'd prefer, you can also write Semgrep rules offline in your IDE of choice. After all, they're just YAML!

This is the rule we're goingn to start on, open it in a separate browser tab: https://semgrep.dev/s/clintgibler:juice-shop-eval-try.

### UI Overview

In the top left, you can select a "Language", which is currently "TypeScript."

The "code is" section is where you write your Semgrep rule. 
* This rule will then be ran against the "Test Code" section at the bottom.
* Any matches will be shown in the right hand side column.

The `// ruleid:juice-shop-eval` comments you see in the Test Code are a special syntax - they're telling Semgrep, "Hey, I expect Semgrep to find a match here." 
* This makes it easy to write unit tests for your Semgrep rules, to ensure they're working as you expect. See [the docs](https://semgrep.dev/docs/writing-rules/testing-rules/) for more details.

If you click on the "Advanced" tab (next to "Simple" under the "Semgrep Rule" header on the left hand side), you'll see the raw YAML for the Semgrep rule you're writing. The "Simple" view is a just a simplified interface so you don't have to write raw YAML and mess with indentation, etc.

## Rule Writing Basics

At a high level, Semgrep rules are just the code you're targeting + a few abstractions.

### The Ellipsis Operator

Sometimes you want to abstract away some details from the code you're matching, to make it more generic. 

The [ellipsis operator](https://semgrep.dev/docs/writing-rules/pattern-syntax/#ellipsis-operator) lets you match zero or more arguments, statements, and more.

You can think of the ellipsis operator (`...`) like `.*` in regular expressions.

### Metavariables

Sometimes you want to match something, but you don't know what it is ahead of time.

For example, the name of a function, the value of an argument, and so forth.

[Metavariables](https://semgrep.dev/docs/writing-rules/pattern-syntax/#metavariables) let you do that by using an identifier that starts with a `$` and is only uppercase letters, `_`, or digits. `$X` or `$FOO` for example.

You can think of metavariables kind of like capture groups in regular expressions.

### Combining Patterns

Sometimes you want to combine Semgrep patterns, like:
* Find calls to `a()` or `b()`
* Find calls to `foo()` but *not* if the first parameter is a string literal
* Find calls to `bar()` but only if it occurs inside the `MyClass` class.

You can add additional pattern clauses in the simple editor by clicking the `+` button on the right hand side of the pattern. 

Currently on a few Semgrep operators are available in the simple editor.See the [rule syntax docs](https://semgrep.dev/docs/writing-rules/rule-syntax/) for all of the tools in your Semgrep rule writing toolbelt.

We'll cover a number of Semgrep's capabilities in this lab, but there's many we won't!

## ⌨️ Activity: Write Your First Custom Rule

1. Navigate to https://semgrep.dev/s/clintgibler:juice-shop-eval-try and update the pattern (currently `TODO`) to match all calls to `my_eval()`, regardless of the passed in arguments.
2. Update the pattern to only match calls to `my_eval()` with only 1 argument.
3. Update the pattern to only match calls to `my_eval()` when the first argument is *not* a string literal.
4. Save the rule, click the "Add to Policy" button in the top right, and select "Starter Policy."
   1. Navigate to the [checks page]({{ checks_page_url }}) for this PR and click the "Re-run jobs" button, to scan the changes with the new rule you wrote.

**Hints**

<details>
 <summary>1: Match all calls to `my_eval()`</summary>
 <br>
  Try using the ellipsis operator, `...`.
</details>

<details>
 <summary>2: Match all calls to `my_eval()` with 1 argument</summary>
 <br>
  Try using a metavariable, like <code>$ARG</code>.
</details>

<details>
 <summary>3: Match all calls to `my_eval()` where the first argument is not a string literal</summary>
 <br>
  In Semgrep, <code>"..."</code> will match any string, regardless of its value ([docs](https://semgrep.dev/docs/writing-rules/pattern-syntax/#strings)).

  And [`pattern-not`](https://semgrep.dev/docs/writing-rules/rule-syntax/#pattern-not) filters out matches.

  Try clicking the <code>+</code> button to add a new pattern and select "and is not", which if you switch to the Advanced view, you can see is represented by `pattern-not` under the hood.
</details>

<hr>
<h3 align="center">Comment on this Pull Request once you've re-scanned this PR with your new `my_eval()` rule.</h3>