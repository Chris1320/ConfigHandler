# ConfigHandler

## Description

Create, update, and remove values from a configuration file made by ConfigHandler.

## Usage

- Version 1:

  ```python

  from config_handler import Version1

  # Setup
  config_path = "config.dat"  # This is our configuration file.
  config = config_handler.Version1(config_path, False)

  # Creating a new configuration file
  config.new()  # Create a new configuration file.

  # Adding new variables
  config.add("sampleVariable", "sampleValue")
  config.add("another variable", 123456789)
  config.add("decimals", 1234.5678)
  config.add("lazymode", True)

  # Getting variables
  print(config.get("sampleVariable"))
  print(config.get("decimals") + 987654321)
  if config.get("lazymode"):
      print("The value of `lazymode` is True.")

  else:
      print("You're not lazy.")

  # Updating existing variables
  print("This is the old value: {0}".format(config.get("sampleVariable")))
  config.set("sampleVariable", "NewValue")
  print("This is the new value: {0}".format(config.get("sampleVariable")))
  ```

- Version 2:

  ```python

  from config_handler import Version2

  # "config.conf" is the configuration file path.
  config = Version2("config.conf", "aPasswordHere")  # The password is optional (required if encryption is not `None`)

  # Create a new configuration file
  config.new(name="Test Configuration File",
            author="Chris1320",
            compression="zlib",
            encryption="aes256"
            )

  # Always load the configuration file or else you won't be able to work with it.
  config.load()

  # Adding new variables
  config.add("aVariableName", "str", "Hello, world!")
  config.add("Another name", "int", 645798)

  # Getting variables
  print(config.get("aVariableName"))
  print(config.get("Another name") * 2)

  # Updating existing variables
  config.update("aVariableName", "New string")
  config.update("Another name", 1234)

  # Configuration File Information
  for key in config.info():
      print("{0}: {1}".format(key, config.info()[key])

  # Remove a variable and it's value
  config.remove("Another name")

  # Export and import dictionaries (configuration file dictionaries)
  exported_data = config.export_config()

  # For example, `dictionary_from_another_confighandler` is from another instance of ConfigHandler.
  config.import_dict(dictionary_from_another_confighandler)
  ```

## Configuration File Structure

- Version 1
  - Dictionary [Base64 encoded (optional)]
- Version 2
  - File info (config name, author, version, etc.) [JSON format, Base64 encoded]
  - Dictionary [JSON format, Compressed]

## Configuration Files Documentation

- Version 1:
  The version 1 of the configuration file is pretty straightforward.
  It only contains the main key/value pairs with optional comments
  represented by `#` symbols.

  ```python
  # This is a comment.
  # This returns "value1" as a string.
  key1=value1
  # This automatically converts to an integer.
  aVariable=1234
  # This automatically converts to a float.
  s0m3thing=3.14
  # This automatically converts to a boolean.
  booleans=true
  ```

  - Dictionary:
    The dictionary contains the key/value pairs of the configuration file.
    The dictionary can be optionally encoded using Base64.
- Version 2:
  The version 2 of the configuration file contains more information about
  itself. It contains the configuration file's name, author, version, and
  the actual dictionary. Everything is encoded using Base64 and the dictionary
  is compressed or even be encrypted. The configuration file follows JSON's
  syntax.

  ```json
  {
    "name": "Sample Configuration File",
    "author": "Author Name",
    "version": "0.0.1.0",
    "compression": "zlib",
    "encryption": "aes256",
    "dictionary": {
        "value1": ["str", "Hello, world!"],
        "aVariable": ["str", "Some things here..."],
        "anInt215": ["int", 1452],
        "1Float": ["float", 3.14],
        "isItTRUE": ["bool", 0],
        "anArrayIGuess": [
            "arr",
            "str",
            ["value1", "Hello again!", "Hola", "Bonjour", "another string of text"]
        ]
    }
  }
  ```

  - Configuration Name:
    The configuration name is identified by the `name` key. This piece of
    information is used when Version2().info() is called.
  - Configuration Author:
    The configuration author is identified by the `author` key.
    It contains the name of the author/creator of the configuration file.
    This is optional.
  - Configuration Version:
    The configuration version is identified by the `version` key. It is **NOT**
    to be manually modified.
  - Dictionary Compression Alogrithm:
    The dictionary compression algorithm is identified by the `compression` key.
    It contains the compression algorithm name used to compress the dictionary.
  - Dictionary Encryption Algorithm:
    The dictionary encryption algorithm is identified by the `encryption` key.
    It contains the encryption algorithm name used to encrypt the dictionary.
  - Dictionary
    The dictionary is the last part of the configuration file.
    The dictionary contains the key/value pairs of the configuration file.
    It can also be optionally encrypted. Currently, the supported data types
    are strings (str), integers (int), decimals (float), booleans (bool),
    arrays (arr), and binary (bin).
