# mini-yaml post 2018
[![Build Status](https://travis-ci.org/onesketchyguy/mini-yaml.svg?branch=master)](https://github.com/onesketchyguy/mini-yaml#build-status)
Single header YAML 1.0 C++11 serializer/deserializer.
Looking for [mini-yaml by jimmiebergman?](https://github.com/jimmiebergmann/mini-yaml)

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

See  [Best practice](https://github.com/onesketchyguy/mini-YAML#best-practice).

## Usage
Put [yaml.hpp](https://github.com/onesketchyguy/mini-yaml/blob/master/yaml.hpp) in your project directory and

```cpp
#define YAML_DEF
#include <yaml.hpp>
```

See [examples/FirstExample.cpp](https://github.com/jimmiebergmann/mini-yaml/blob/master/examples/FirstExample.cpp) for additional examples.

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
```

You can create sequences in the following manors;
```cpp
std::vector<std::string> names {
  "Jarod", "Mark", "Luise", "Felicia"
};

YAML::Node method1; // The standard method, simply manually add each value into the node sequence
for (int i = 0; i < names.size(); i++)
{
  method1["names"].PushBack();
  method1["names"][i] = names.at(i);
}

YAML::Node method2; // The added method, simply call the AddSequence function
YAML::AddSequence(method2["names"], names);

YAML::Node method3; // The operator method, this will call this above AddSequence function
method3["names"] = names;
```

## Build status
Builds are passed if all tests are good and no memory leaks were found.

| Branch | Status |
| ------ | ------ |
| master | [![Build Status](https://travis-ci.org/onesketchyguy/mini-yaml.svg?branch=master)](https://github.com/onesketchyguy/mini-yaml#build-status) |

## Todo
- Parse/serialize tags(!!type).
- Parse anchors.
- Parse flow sequences/maps.
- Parse complex keys.
- Parse sets.


## Changes from [jimmiebergmann-mini-yaml](https://github.com/jimmiebergmann/mini-YAML)
- Moved to a single header with a define instead of a header and cpp combo.
- Added type abstraction layer so you don't have to cast things to strings before serializing them.
- Removed unnecessary work folders.
- Updated gitignore to ignore vscode stuff.
- Added `AddSequence(node, std::vector)` to YAML namespace.
- Added sequence operator to Node