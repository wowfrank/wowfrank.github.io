---
layout: post
title: "The Factory Method Pattern in Python"
date: "2023-03-31 01:09:00 +0800"
description: "The Factory Method Pattern in Python" # (optional)
img: "2023-03-31-cover-image-the-factory-method-design-pattern-in-python.webp" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['python', 'programming language']
categories: ['python', 'programming language']
---

# Definition

**Factory Method is a creational design pattern used to create concrete implementations of a common interface.** It separates the process of creating an object from the code that depends on the interface of the object. 

Instead of using a complex if/elif/else conditional structure to determine the concrete implementation, the application delegates that decision to a separate component that creates the concrete object. With this approach, the application code is simplified, making it more reusable and easier to maintain.

Imagine an application that needs to convert a Song object into its string representation using a specified format. Converting an object to a different representation is often called serializing. You’ll often see these requirements implemented in a single function or method that contains all the logic and implementation, like in the following code:

```python
# In serializer_demo.py

import json
import xml.etree.ElementTree as et

class Song:
    def __init__(self, song_id, title, artist):
        self.song_id = song_id
        self.title = title
        self.artist = artist


class SongSerializer:
    def serialize(self, song, format):
        if format == 'JSON':
            song_info = {
                'id': song.song_id,
                'title': song.title,
                'artist': song.artist
            }
            return json.dumps(song_info)
        elif format == 'XML':
            song_info = et.Element('song', attrib={'id': song.song_id})
            title = et.SubElement(song_info, 'title')
            title.text = song.title
            artist = et.SubElement(song_info, 'artist')
            artist.text = song.artist
            return et.tostring(song_info, encoding='unicode')
        else:
            raise ValueError(format)
```

In the example above, you have a basic Song class to represent a song and a SongSerializer class that can convert a song object into its string representation according to the value of the format parameter.

The .serialize() method supports two different formats: JSON and XML. Any other format specified is not supported, so a ValueError exception is raised.

Let’s use the Python interactive shell to see how the code works:

```python
>>> import serializer_demo as sd
>>> song = sd.Song('1', 'Water of Love', 'Dire Straits')
>>> serializer = sd.SongSerializer()

>>> serializer.serialize(song, 'JSON')
'{"id": "1", "title": "Water of Love", "artist": "Dire Straits"}'

>>> serializer.serialize(song, 'XML')
'<song id="1"><title>Water of Love</title><artist>Dire Straits</artist></song>'

>>> serializer.serialize(song, 'YAML')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "./serializer_demo.py", line 30, in serialize
    raise ValueError(format)
ValueError: YAML
```

You create a song object and a serializer, and you convert the song to its string representation by using the .serialize() method. The method takes the song object as a parameter, as well as a string value representing the format you want. The last call uses YAML as the format, which is not supported by the serializer, so a ValueError exception is raised.

This example is short and simplified, but it still has a lot of complexity. There are three logical or execution paths depending on the value of the format parameter. This may not seem like a big deal, and you’ve probably seen code with more complexity than this, but the above example is still pretty hard to maintain.

# The Problems With Complex Conditional Code

The example above exhibits all the problems you’ll find in complex logical code. Complex logical code uses if/elif/else structures to change the behavior of an application. Using if/elif/else conditional structures makes the code harder to read, harder to understand, and harder to maintain.

The code above might not seem hard to read or understand, but wait till you see the final code in this section!

Nevertheless, the code above is hard to maintain because it is doing too much. The **single responsibility principle** states that a **module**, a **class**, or even a **method** should have a single, well-defined responsibility. It should do just one thing and have only one reason to change.

The .serialize() method in SongSerializer will require changes for many different reasons. This increases the risk of introducing new defects or breaking existing functionality when changes are made. Let’s take a look at all the situations that will require modifications to the implementation:

- When a new format is introduced: The method will have to change to implement the serialization to that format.
- When the Song object changes: Adding or removing properties to the Song class will require the implementation to change in order to accommodate the new structure.
- When the string representation for a format changes (plain JSON vs JSON API): The .serialize() method will have to change if the desired string representation for a format changes because the representation is hard-coded in the .serialize() method implementation.

The ideal situation would be if any of those changes in requirements could be implemented without changing the .serialize() method. Let’s see how you can do that in the following sections.

# Looking for a Common Interface

The first step when you see complex conditional code in an application is to identify the common goal of each of the execution paths (or logical paths).

Code that uses if/elif/else usually has a common goal that is implemented in different ways in each logical path. The code above converts a song object to its string representation using a different format in each logical path.

Based on the goal, you look for a common interface that can be used to replace each of the paths. The example above requires an interface that takes a song object and returns a string.

Once you have a common interface, you provide separate implementations for each logical path. In the example above, you will provide an implementation to serialize to JSON and another for XML.

Then, you provide a separate component that decides the concrete implementation to use based on the specified format. This component evaluates the value of format and returns the concrete implementation identified by its value.

In the following sections, you will learn how to make changes to existing code without changing the behavior. This is referred to as refactoring the code.

Martin Fowler in his book [Refactoring: Improving the Design of Existing Code](https://realpython.com/asins/0134757599/) defines refactoring as “the process of changing a software system in such a way that does not alter the external behavior of the code yet improves its internal structure.” If you’d like to see refactoring in action, check out the Real Python Code Conversation [Refactoring: Prepare Your Code to Get Help](https://realpython.com/courses/refactoring-code-to-get-help/).

Let’s begin refactoring the code to achieve the desired structure that uses the Factory Method design pattern.

# Refactoring Code Into the Desired Interface

The desired interface is an object or a function that takes a Song object and returns a string representation.

The first step is to refactor one of the logical paths into this interface. You do this by adding a new method .\_serialize\_to\_json() and moving the JSON serialization code to it. Then, you change the client to call it instead of having the implementation in the body of the if statement:

```python
class SongSerializer:
    def serialize(self, song, format):
        if format == 'JSON':
            return self._serialize_to_json(song)
        # The rest of the code remains the same

    def _serialize_to_json(self, song):
        payload = {
            'id': song.song_id,
            'title': song.title,
            'artist': song.artist
        }
        return json.dumps(payload)
```

Once you make this change, you can verify that the behavior has not changed. Then, you do the same for the XML option by introducing a new method .\_serialize\_to\_xml(), moving the implementation to it, and modifying the elif path to call it.

The following example shows the refactored code:

```python
class SongSerializer:
    def serialize(self, song, format):
        if format == 'JSON':
            return self._serialize_to_json(song)
        elif format == 'XML':
            return self._serialize_to_xml(song)
        else:
            raise ValueError(format)

    def _serialize_to_json(self, song):
        payload = {
            'id': song.song_id,
            'title': song.title,
            'artist': song.artist
        }
        return json.dumps(payload)

    def _serialize_to_xml(self, song):
        song_element = et.Element('song', attrib={'id': song.song_id})
        title = et.SubElement(song_element, 'title')
        title.text = song.title
        artist = et.SubElement(song_element, 'artist')
        artist.text = song.artist
        return et.tostring(song_element, encoding='unicode')
```

The new version of the code is easier to read and understand, but it can still be improved with a basic implementation of Factory Method.

# Basic Implementation of Factory Method

**The central idea in Factory Method is to provide a separate component with the responsibility to decide which concrete implementation should be used based on some specified parameter**. That parameter in our example is the format.

To complete the implementation of Factory Method, you add a new method .\_get\_serializer() that takes the desired format. This method evaluates the value of format and returns the matching serialization function:

```python
class SongSerializer:
    def _get_serializer(self, format):
        if format == 'JSON':
            return self._serialize_to_json
        elif format == 'XML':
            return self._serialize_to_xml
        else:
            raise ValueError(format)
```

```
Note: The ._get_serializer() method does not call the concrete implementation, and it just returns the function object itself.
```

Now, you can change the .serialize() method of SongSerializer to use ._get_serializer() to complete the Factory Method implementation. The next example shows the complete code:

```python
class SongSerializer:
    def serialize(self, song, format):
        serializer = self._get_serializer(format)
        return serializer(song)

    def _get_serializer(self, format):
        if format == 'JSON':
            return self._serialize_to_json
        elif format == 'XML':
            return self._serialize_to_xml
        else:
            raise ValueError(format)

    def _serialize_to_json(self, song):
        payload = {
            'id': song.song_id,
            'title': song.title,
            'artist': song.artist
        }
        return json.dumps(payload)

    def _serialize_to_xml(self, song):
        song_element = et.Element('song', attrib={'id': song.song_id})
        title = et.SubElement(song_element, 'title')
        title.text = song.title
        artist = et.SubElement(song_element, 'artist')
        artist.text = song.artist
        return et.tostring(song_element, encoding='unicode')
```

The final implementation shows the different components of Factory Method. The .serialize() method is the application code that depends on an interface to complete its task.

This is referred to as the **client component** of the pattern. The interface defined is referred to as the **product component**. In our case, the product is a function that takes a Song and returns a string representation.

The ._serialize_to_json() and ._serialize_to_xml() methods are concrete implementations of the product. Finally, the ._get_serializer() method is the **creator component**. The creator decides which concrete implementation to use.

Because you started with some existing code, all the components of Factory Method are members of the same class SongSerializer.

Usually, this is not the case and, as you can see, none of the added methods use the self parameter. This is a good indication that they should not be methods of the SongSerializer class, and they can become external functions:

```python
class SongSerializer:
    def serialize(self, song, format):
        serializer = get_serializer(format)
        return serializer(song)


def get_serializer(format):
    if format == 'JSON':
        return _serialize_to_json
    elif format == 'XML':
        return _serialize_to_xml
    else:
        raise ValueError(format)


def _serialize_to_json(song):
    payload = {
        'id': song.song_id,
        'title': song.title,
        'artist': song.artist
    }
    return json.dumps(payload)


def _serialize_to_xml(song):
    song_element = et.Element('song', attrib={'id': song.song_id})
    title = et.SubElement(song_element, 'title')
    title.text = song.title
    artist = et.SubElement(song_element, 'artist')
    artist.text = song.artist
    return et.tostring(song_element, encoding='unicode')
```

```
Note: The .serialize() method in SongSerializer does not use the self parameter.

The rule above tells us it should not be part of the class. This is correct, but you are dealing with existing code.

If you remove SongSerializer and change the .serialize() method to a function, then you’ll have to change all the locations in the application that use SongSerializer and replace the calls to the new function.

Unless you have a very high percentage of code coverage with your unit tests, this is not a change that you should be doing.
```

The mechanics of Factory Method are always the same. A **client** (SongSerializer.serialize()) depends on a concrete implementation of an interface. It requests the implementation from a creator component (get_serializer()) using some sort of identifier (format).

The creator returns the concrete implementation according to the value of the parameter to the client, and the client uses the provided object to complete its task.

You can execute the same set of instructions in the Python interactive interpreter to verify that the application behavior has not changed:

```python
>>> import serializer_demo as sd
>>> song = sd.Song('1', 'Water of Love', 'Dire Straits')
>>> serializer = sd.SongSerializer()

>>> serializer.serialize(song, 'JSON')
'{"id": "1", "title": "Water of Love", "artist": "Dire Straits"}'

>>> serializer.serialize(song, 'XML')
'<song id="1"><title>Water of Love</title><artist>Dire Straits</artist></song>'

>>> serializer.serialize(song, 'YAML')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "./serializer_demo.py", line 13, in serialize
    serializer = get_serializer(format)
  File "./serializer_demo.py", line 23, in get_serializer
    raise ValueError(format)
ValueError: YAML
```

You create a song and a serializer, and use the serializer to convert the song to its string representation specifying a format. Since YAML is not a supported format, ValueError is raised.

# Recognizing Opportunities to Use Factory Method

**Factory Method _should_ be used in every situation where an application (client) depends on an interface (product) to perform a task and there are multiple concrete implementations of that interface**. You need to provide a parameter that can identify the concrete implementation and use it in the creator to decide the concrete implementation.

There is a wide range of problems that fit this description, so let’s take a look at some concrete examples.

**Replacing complex logical code**: Complex logical structures in the format if/elif/else are hard to maintain because new logical paths are needed as requirements change.

Factory Method is a good replacement because you can put the body of each logical path into separate functions or classes with a common interface, and the creator can provide the concrete implementation.

The parameter evaluated in the conditions becomes the parameter to identify the concrete implementation. The example above represents this situation.

**Constructing related objects from external data**: Imagine an application that needs to retrieve employee information from a database or other external source.

The records represent employees with different roles or types: managers, office clerks, sales associates, and so on. The application may store an identifier representing the type of employee in the record and then use Factory Method to create each concrete Employee object from the rest of the information on the record.

**Supporting multiple implementations of the same feature**: An image processing application needs to transform a satellite image from one coordinate system to another, but there are multiple algorithms with different levels of accuracy to perform the transformation.

The application can allow the user to select an option that identifies the concrete algorithm. Factory Method can provide the concrete implementation of the algorithm based on this option.

**Combining similar features under a common interface**: Following the image processing example, an application needs to apply a filter to an image. The specific filter to use can be identified by some user input, and Factory Method can provide the concrete filter implementation.

**Integrating related external services**: A music player application wants to integrate with multiple external services and allow users to select where their music comes from. The application can define a common interface for a music service and use Factory Method to create the correct integration based on a user preference.

All these situations are similar. They all define a client that depends on a common interface known as the product. They all provide a means to identify the concrete implementation of the product, so they all can use Factory Method in their design.

You can now look at the serialization problem from previous examples and provide a better design by taking into consideration the Factory Method design pattern.

# An Object Serialization Example



![Advanced Python Tutorial]({{site.baseurl}}/assets/img/2023-03-30/bc-10.jpg)



#### 源自[从阮晓寰到“编程随想”：一个普通公民和“极客”如何成了“国家的敌人”？](https://ngocn2.org/article/2023-03-29-program-think-enemy-of-the-state/) 发布于 2023-03-29