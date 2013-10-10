Merge Variables
===============

Text Replacement Package for Sublime Text 2 and 3

Merge Variables allows you define sets of replaceable strings. When you run the `Merge Variables` command, the package expands each of the strings with a replacement. For example, you can turn this:

```html
<a href="http://{{HOSTNAME}}/page">{{TITLE}}</a>
```

into:

```html
<a href="http://www.myexamplesite.com/page">My Example Site</a>
```

## How to Use

### Creating Sets

To define the your variables and their values, open the user settings for the package. Create a `"sets"` member. This member will be an object, and each of its members will be the name of a "set". Each of the sets will also be an object containin key-value pairs for the variables and their values. Here's an example:

```json
{
    "sets": {
        "mysite": {
            "{{HOSTNAME}}": "www.myexamplesite.com",
            "{{TITLE}}": "My Example Site"
        }
    }
}
```

**Note:** There's no need to use double curly braces around your variables names. This is my convention, but it isn't required. You may want to use some symbols on either side to safeguard against unwanted replacements. %PERCENTS%, [brackets], =Equals=, etc. could all be good choices.

Next you'll need to make this an active set. Add an `"active_sets"` member to the user settings file. This member is a list of all of the names of sets you would like to use.

```json
{
    "active_sets": ["mysite"],
    "sets": {
        "mysite": {
            "{{HOSTNAME}}": "www.myexamplesite.com",
            "{{TITLE}}": "My Example Site"
        }
    }
}
```


### Merging Variables

Once you have your variables defined, open the Command Palette (Shift + Command + P) and enter `Merge Variables`.

### Cascading Sets

But wait! There's more! You can define multiple sets, and merge them together. In this example, there are three sets: `"mysite"`, `"mysite-development"`, and `"some other set we're not going to use yet"`.

```json
{
    "active_sets": ["mysite", "mysite-development"],
    "sets": {
        "mysite": {
            "{{HOSTNAME}}": "www.myexamplesite.com",
            "{{TITLE}}": "My Example Site"
        },
        "mysite-development": {
            "{{HOSTNAME}}": "dev.myexamplesite.com"
        },
        "some other set we're not going to use yet": {
            "{{HOSTNAME}}": "www.not-this-site.com",
            "{{TITLE}}": "Won't See This Title"
        }

    }
}
```

I've specified the list of active sets as `["mysite", "mysite-development"]`. This tells the package to start wth the key-value pairs from `"mysite"` first, then override those values with the ones in `"mysite-development"`. Here's what happens:

- `{{HOSTNAME}}` is defined in both sets. The value from the last set takes precedence.
- `{{TITLE}}` is defined in the first set, but not the second. That's okay. The value from the first carries through.
- The set `"some other set we're not going to use yet"` is not in the list of active sets, so its values are ignored.

The result:

```html
<a href="http://dev.myexamplesite.com/page">My Example Site</a>
```

## Programatically

If you're super cool, and you write Sublime packages too, you may want to call `Merge Variables` in code. When you do, you can pass the list of active sets at call time. Here's an example:

```python
view.run_command("merge_variables", active_sets=["this_set", "that_set"])
```

## Installation

### Sublime Package Control

You can install Merge Variables using the excellent [Package Control][] package manager for Sublime Text:

1. Open "Package Control: Install Package" from the Command Palette (`Shift` + `Command` + `P`).
2. Select the "Merge Variables" option to install Merge Variables.

[Package Control]: http://wbond.net/sublime_packages/package_control

### Git Installation

To install manually using Git, clone to your "Packages" directory.

```bash
git clone git@github.com:pjdietz/sublime-merge-variables.git "Merge Variables"
```

**Note:** Some features such as the menu command to open the default settings will not work if the package is not installed to a directory called "Merge Variables".

## Author

**PJ Dietz**

+ [http://pjdietz.com](http://pjdietz.com)
+ [http://github.com/pjdietz](http://github.com/pjdietz)
+ [http://twitter.com/pjdietz](http://twitter.com/pjdietz)

## Copyright and license
Copyright 2013 PJ Dietz

[MIT License](LICENSE)
