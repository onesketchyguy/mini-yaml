# mini-YAML
[![Build Status](https://travis-ci.org/jimmiebergmann/mini-YAML.svg?branch=master)](https://github.com/jimmiebergmann/mini-YAML#build-status)  
Single header YAML 1.0 C++11 serializer/deserializer.

## Quickstart
#### file.txt
```
key: foo bar
list:
  - hello world
  - integer: 123
    boolean: true
```
#### .cpp
```cpp
// NOTE: Do not forget to #define YAML_DEF before you import the library!

YAML::Node root;
YAML::Parse(root, "file.txt");

// Print all scalars.
std::cout << root["key"].As<std::string>() << std::endl;
std::cout << root["list"][0].As<std::string>() << std::endl;
std::cout << root["list"][1]["integer"].As<int>() << std::endl;
std::cout << root["list"][1]["boolean"].As<bool>() << std::endl;

// Iterate second sequence item.
Node & item = root[1];
for(auto it = item.Begin(); it != item.End(); it++)
{
    std::cout << (*it).first << ": " << (*it).second.As<string>() << std::endl;
}
```
#### Output
```
foo bar
hello world
123
1
integer: 123
boolean: true
```

See  [Best practice](https://github.com/jimmiebergmann/mini-YAML#best-practice).

## Usage
Put [/YAML](https://github.com/jimmiebergmann/mini-YAML/blob/master/YAML) in your project directory and simply #include "[YAML/YAML.hpp](https://github.com/jimmiebergmann/mini-YAML/blob/master/YAML/YAML.hpp)".
See [examples/FirstExample.cpp](https://github.com/jimmiebergmann/mini-YAML/blob/master/examples/FirstExample.cpp) for additional examples.

## Best practice
Always use references when accessing node content, if not intended to make a copy. Modifying copied node wont affect the original node content.  
See example:
```cpp
YAML::Node root;

YAML::Node & ref = root;  // The content of "root" is not being copied.
ref["key"] = "value";     // Modifying "root" node content.

YAML::Node copy = root;   // The content of "root" is copied to "copy".
                          // Slow operation if "root" contains a lot of content.
copy["key"] = "value";    // Modifying "copy" node content. "root" is left untouched.

// You can also push vectors into a list
YAML::Node list;

std::vector<std::string> names {
  "Jarod", "Mark", "Luise", "Felicia"
};

YAML::Node::AddSequence(list["names"], names);
```

## Build status
Builds are passed if all tests are good and no memory leaks were found.

| Branch | Status |
| ------ | ------ |
| master | [![Build Status](https://travis-ci.org/jimmiebergmann/mini-YAML.svg?branch=master)](https://travis-ci.org/jimmiebergmann/mini-YAML) |
| dev | [![Build Status](https://travis-ci.org/jimmiebergmann/mini-YAML.svg?branch=dev)](https://travis-ci.org/jimmiebergmann/mini-YAML)|

## Todo
- Parse/serialize tags(!!type).
- Parse anchors.
- Parse flow sequences/maps.
- Parse complex keys.
- Parse sets.

