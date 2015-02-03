# Medio

A highly opinionated code generator library.

Currently provides only a way to generate a brand new class method:

* arguments are documented using PHPdoc (variable names are aligned)
* if the method would be too long on one line (> 120 columns), its arguments are displayed each on their own line
* objects, arrays and callables arguments are typehinted
* can automatically name the arguments for you (handles name collision)

## Installation

Use [Composer](https://getcomposer.org/download):

    composer require gnugat/medio:~0.4.0

## Usage

Let's say that we have an array of variables which represent a method's arguments:

```php
$arguments = array('string', new \ArrayObject(), function () {});
```

Here's a snippet which uses Medio:

```php
$argumentCollection = $variableArgumentCollectionFactory->make($arguments);
$method = new Method($argumentCollection, '__construct');

echo $methodPrinter->dump($method);
```

It should print the following generated code:

```
    /**
     * @param string       $argument1
     * @param \ArrayObject $arrayObject
     * @param callable     $argument2
     */
    public function __construct($argument1, \ArrayObject $arrayObject, callable $argument2)
    {
    }
```

As you can see the arguments have been type hinted and the object has been named
after its type. The non object arguments have been named using a generic name
(`argument`), but name collision has been detected and they've been suffixed by
a number.

For more examples, see:

* [Creating a phpspec extension](./doc/example-phpspec-extension.md)

## Details

![UML class Diagram](http://yuml.me/db33df58)

Medio provides a modelisation of your code (in `Gnugat\Medio\Model`):

* a `Method` has an `ArgumentCollection`
* an `ArgumentCollection` can have 0 to many `Argument`, it also takes care of avoiding name collision
* an `Argument` has a name and type
* a `Type`, takes care of type name uniformization and can detect if it's an object

Creating those models manually can be tedious, so factories are provided (in `Gnugat\Medio\Factory`):

* `TypeArgumentFactory` can create an `Argument` from a given type
* `VariableArgumentFactory` can create an `Argument` from a given variable (guesses the type)
* `VariableArgumentCollectionFactory` can create an `ArgumentCollection` from an array of variables

Once modelized, the code can be generated using "[pretty printers](http://stackoverflow.com/a/5834775/3437428)"
(in `Gnugat\Medio\PrettyPrinter`):

* `ArgumentPrinter` takes care of type hinting
* `InlineArgumentCollectionPrinter` makes an inline list of arguments
* `MultilineArgumentCollectionPrinter` makes a list of arguments, each on its own line
* `MethodPrinter` makes a method (can choose multiline arguments to avoid being lengthier than 120 charactes)
* `MethodPhpdocPrinter` generates a method's PHPDoc, with aligned argument names

> **Note**: those "pretty printers" aren't "fidelity printers", they'll format
> the code based on high opinions (they can be considered as "nice printers").

## Further documentation

You can see the current and past versions using one of the following:

* the `git tag` command
* the [releases page on Github](https://github.com/gnugat/medio/releases)
* the file listing the [changes between versions](CHANGELOG.md)

You can find more documentation at the following links:

* [copyright and MIT license](LICENSE)
* [versioning and branching models](VERSIONING.md)
* [contribution instructions](CONTRIBUTING.md)
