TOML parser
==============

A PHP parser for [TOML](https://github.com/mojombo/toml) compatible with [TOML v0.1.0](https://github.com/mojombo/toml/blob/master/versions/toml-v0.1.0.md).

Installation
------------

Use [Composer](http://getcomposer.org/) to install Yosyfmony Toml package:

Add the following to your `composer.json` and run `composer update`.

    "require": {
        "yosymfony/toml": "0.1.*@dev"
    }

More informations about the package on [Packagist](https://packagist.org/packages/yosymfony/toml).

Usage
-----
You can use this package to parse TOML string inline or from a file with only one method:

    use Yosymfony\Toml\Toml;
    
    $array = Toml::Parse('key = [1,2,3]');
    
    print_r($array);

From a file:

    use Yosymfony\Toml\Toml;
    
    $array = Toml::Parse('example.toml');
    
    print_r($array);

### TomlBuilder
You can create inline TOML string with TomlBuilder. TomBuilder used Fluent interface for more readable code:

    use Yosymfony\Toml\TomlBuilder;
    
    $tb = new TomlBuilder();
        
    $result = $tb->addComment('Toml file')->
        addGroup('data.string')->
        addValue('name', "Toml", 'This is your name')->
        addValue('newline', "This string has a \n new line character.")->
        addValue('winPath', "C:\\Users\\nodejs\\templates")->
        addValue('unicode', 'unicode character: ' . json_decode('"\u03B4"'))->
        
        addGroup('data.bool')->
        addValue('t', true)->
        addValue('f', false)->
        
        addGroup('data.integer')->
        addValue('positive', 25, 'Comment inline.')->
        addValue('negative', -25)->
        
        addGroup('data.float')->
        addValue('positive', 25.25)->
        addValue('negative', -25.25)->
        
        addGroup('data.datetime')->
        addValue('datetime', new \Datetime())->
        
        addComment('Related to arrays')->
        addGroup('data.array')->
        addValue('simple', array(1,2,3))->
        addValue('multiple', array( 
            array(1,2), 
            array('abc', 'def'), 
            array(1.1, 1.2), 
            array(true, false), 
            array( new \Datetime()) ))->
        
        getTomlString();    // Generate the TOML string

The result of this example:

    #Toml file
    
    [data.string]
    name = "Toml" #This is your name
    newline = "This string has a \n new line character."
    winPath = "C:\\Users\\nodejs\\templates"
    unicode = "unicode character: δ"
    
    [data.bool]
    t = true
    f = false
    
    [data.integer]
    positive = 25 #Comment inline.
    negative = -25
    
    [data.float]
    positive = 25.25
    negative = -25.25
    
    [data.datetime]
    datetime = 2013-06-10T21:12:48Z
    #Related to arrays
    
    [data.array]
    simple = [1, 2, 3]
    multiple = [[1, 2], ["abc", "def"], [1.1, 1.2], [true, false], [2013-06-10T21:12:48Z]]


Unit tests
----------
This package are in compliance with BurntSushi [test suite for TOML parsers](https://github.com/BurntSushi/toml-test).

You can run the unit tests with the following command:

    $ cd your-path/vendor/yosymfony/toml
    $ composer.phar install --dev
    $ phpunit