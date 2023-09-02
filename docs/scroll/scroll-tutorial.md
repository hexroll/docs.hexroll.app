# Scroll Tutorial
__*Scroll*__ offers an efficient syntax for coding text generation models and it enables most of the new capabilities planned for HEXROLL|2.

Some the capabilities __*Scroll*__ has are:

* Rolling random values from lists.
* Rolling random values using dice notations.
* Using entity types to reuse generation rules.
* Defining global constants to reuse lists or static values.
* Creating a directed-acyclic-graph of entities that represent complex constructs (such as a complete sandbox).
* Sharing data between entities using entity collections and data injection.


## Basic __*Scroll*__ Tutorial

__If you are a patron of Hexroll, you can follow along using the REPL editor linked in our Patreon page.__


### Hello __*Scroll*__

Let's begin with a simple example:

```
main {
    greeting! @ [
        * Hello
        * Hey
        * Hi
    ]
    
    <html%
        <p> {{greeting}}, Scroll. </p>
    %html>
}
```


The basic construct in __*Scroll*__ are __classes__. Classes are used to define entity types. In the above example we define a class named `main` that has an attribute named `greeting` and an `HTML` renderer.

Let's examine the `greeting` attribute definition:

```
greeting! @ [
    * Hello
    * Hey
    * Hi
]
```

This expression means: "Define an attribute named `greeting` and make it visible to any renderer (using the `!` symbol), then roll (`@`) a random value from a list of possible values indicated by `*` bulleted items inside matching `[` square brackets `]`"

The `@` operator is a key construct of Scroll and we will use it frequently to generate random values and entities moving forward.

The `!` operator must be used any time we wish to use an attribute to output text from a renderer or use an attribute from another entity (more on this later).


Now let's examine the `HTML` renderer:
```
<html%
    <p> {{greeting}}, World. </p>
%html>
```

Any entity type we define can have a `<html%` `%html>` block that can hold HTML template code. This code is a mixture of standard HTML tags together with template code that can reference attribute values or even execute custom logic.

In our example we render a paragraph using the `<p>` tag and then use the double curly braces notation `{{greeting}}` to output the random value rolled for `greeting`.

Finally, both `greeting` and the `HTML` renderer are placed inside the `{` curly braces `}` of the `main` class. Every Scroll script must have exactly one `main` class definition as the entry point for the generator.


### Classes

Classes in __*Scroll*__ are defined using an identifier, followed by a curly braced block containing the class content:

```
Realm {
    name @ [
        * Ishabor
        * Zeratha
        * Inaba
    ]
}
```

Classes can extend other classes:

```
Kingdom (Realm) {
  title! = "Kingdom of {{name}}"
}
```

Let put this together:

```
Realm {
    name @ [
        * Ishabor
        * Zeratha
        * Inaba
    ]
}

Kingdom (Realm) {
  title! = "Kingdom of {{name}}"
}

main {
    realm! @ Kingdom
    
    <html%
        Our realm is the
        <strong>{{realm.title}}</strong>.
    %html>
}
```

In the above example we've defined a base `Realm` type with a random name. We then extended this class by defining a new class named `Kingdom` and indicating that it will be extending `Realm` using the round parentheses notation `Kingdom (Realm) {...}`.

In the `main` class, you can see how we now use the `@` operator to roll an entity by using a class name.

Finally, note how we do not use the `!` symbol for the `name` attribute since it is not directly used in the HTML rendering code. It is important to use `!` selectively, especially when generating a large number of entities.

Let's take this a step further:


```
Realm {
    name @ [
        * Ishabor
        * Zeratha
        * Inaba
    ]
}

Kingdom (Realm) {
  title! = "Kingdom of {{name}}"
}

Duchy (Realm) {
  title! = "Duchy of {{name}}"
}

main {
    realm! @@ [
        * Kingdom
        * Duchy
    ]
    
    <html%
        Our realm is the
        <strong>{{realm.title}}</strong>.
    %html>
}
```

We've added yet another realm type named `Duchy` and used the double roll operator `@@` to roll a value from a list, and then used that value as the class name to roll an entity from.

### Attributes

We've already seen how we can define attributes in classes. We've used the `=` operator to assign a value to an attribute and we've used the `@` operator (the `@@` variant) to roll a random value into an attribute.

There are two additional assignment operators that can be used to choose entities from entity collections. I will cover these in a later post.

Scroll attributes can contain the following data types:

* Strings
* Numbers
* Dice roll values
* Boolean values (true or false)
* Entities
* List of entities

#### Strings

Simple strings can be simply assigned into attributes using the `=` operator:

```
attribute_name = A simple string
```

Strings that has special symbols require surrounding quotes:

```
attribute_name = "a string with {{template}} code"
```

Or surrounding `<` `>` symbols if they span across multiple lines:

```
attribute_name = <a longer
                      multi-line string>
```

Or surrounding `<%` `%>` symbols if they span across multiple lines and contain special symbols:

```
attribute_name = <% a longer multi-line string
                         with {{template}} code and other
                         <symbols> %>
```

Rolling random values from lists is done using the `@` operator. List items can span across multiple lines as well.

```
attribute_name @ [
    * lists can have multi-line strings
      specified without the < > notation.
    * < Unless the text contains the * symbol >
]
```


#### Numbers

```
attribute_name = 42
```

```
attribute_name = 42.0
```

#### Dice roll values

Scroll allows rolling dice values using the `@` operator, followed with a dice roll notation:

```
attribute_name @ 3d6
```

```
attribute_name @ 1d20+2
```

```
attribute_name @ 8d7x2
```


#### Booleans

```
attribute_name = true
```

```
attribute_name = false
```

#### Entities

As demonstrated in the examples above, entities can also be rolled using the `@` or `@@` operators:

```
attribute_name @ EntityClassName
```

```
attribute_name @@ [
    * OneEntityClassName
    * AnotherEntityClassName
]
```


#### List of Entities

Scroll has a special notation for rolling lists of entities:

```
[3..10 attribute_name] @ EntityClassName
```

```
[5..5 attribute_name] @@ [
    * OneEntityClassName
    * AnotherEntityClassName
]
```

Attributes that represent lists are placed inside `[` square brackets `]` and prefixed with a cardinality notation of `min_value..max_value` where `min_value` is the minimum number of items to roll and `max_value` is the maximum number of items to roll.

List items can be rendered using `{% for item in list %} {{item.value} {% endfor %}` template syntax.


### Putting everything together

Let's conclude this short introduction with the following example:

```
Npc {
    first_name @ [
        * Walatus
        * Ajutor
        * Sieggo
        * Milia
        * Petesia
        * Lefsy
        * Latilde
    ]
    last_name @ [
        * Bossinia
        * Brightmer
        * Ermenbald
        * Hildeward
        * Siclebald
        * Zawissius
    ]
    age @ 7d12
    description! = < {{first_name}} {{last_name}} 
                     ({{age}} years old) >
}

Settlement {
    name @ [
        * Akkis
        * Bazuul
        * Devilville
        * Ecrean
        * Frostfyord
        * Khezal
    ]
    <html%
    {{title}} is home for: <ul>
    {% for npc in npcs %}
    <li> {{ npc.description }} </li>
    {% endfor %} </ul> 
    %html>
}

Village (Settlement) {
  title! = "The Village of {{name}}"
  [6..12 npcs!] @ Npc
}

Town (Settlement) {
  title! = "The Town of {{name}}"
  [12..33 npcs!] @ Npc
}


Realm {
    name @ [
        * Ishabor
        * Zeratha
        * Inaba
    ]
    [3..6 settlements!] @@ [
        * Town
        * Village
    ]
}

Kingdom (Realm) {
  title! = "Kingdom of {{name}}"
}

Duchy (Realm) {
  title! = "Duchy of {{name}}"
}

main {
    realm! @@ [
        * Kingdom
        * Duchy
    ]
    
    <html%
        <p>
        Our realm is the
        <strong>{{realm.title}}</strong>.
        </p>
        
        <p>
        The realm has the following settlements:
        {% for s in realm.settlements %}
        <p> {{html(s)}} </p>
        {% endfor %}
        </p>

    %html>
}
```


Follow along using the [SCROLL (alpha version, bugs, no-guarantees, etc) REPL](/scroll/repl.html) environment.

In the [previous post](/posts/introduction-to-scroll), I covered the basics of using `SCROLL` to generate a random hierarchy of entities together with their attributes.

I covered:

- Defining classes of entities
- Defining class attributes of various types
- Rolling values from a list
- Generating a list of entities
- The basics of using templates to generate text

In this post I will continue and explore:

- Collecting and re-using generated entities
- Using data injection to link entities together
- Using scope references to access ancestors entity values

To me, one of the neat things about HEXROLL is its ability to link entities together. This is done in a way that is just enough to prompt you with cool plot hook to further develop.

Let's take a look at how this linking thing actually works.

### Collections

By using collections we can instruct an entity to 'collect' references to other entities generated as part of its hierarchy, regardless to their depth.

For example, in the previous post we generated realms that contain settlements, which in turn, contain NPCs.

Let's extended that model and use a collection directive to store references to all generated NPCs inside the realm:

```
Realm {
  << Npc
  name @ [
    * Ishabor
    * Zeratha
    * Inaba
  ]
  [3..6 settlements!] @@ [
    * Town
    * Village
  ]
}
```

We use the **`<<` operator** followed by a **class name** to tell `SCROLL` to store a reference to any generated entity of that class within its hierarchy.

```
<< Npc
```

This allows us to re-use collected entities in other entities within the same parent class.

For example, we can use the collected NPCs to form factions with members from any part of the realm:

```
Faction {
  name! @ [
    * The Flaming Lambs
    * The Army of Justice
  ]

  [6..13 members!] ? Npc

  <html%
    <h1>{{name}}</h1>
    <h2>Members</h2>
    <ul>
    {% for member in members %}
    <li> {{ member.full_name}} </li>
    {% endfor %}
    </ul>
  %html>
}
```

We use the **`?` operator (pop entity)** instead of the `@` operator to ask `SCROLL` to use one of the referenced NPCs as a faction member.

Note that by using the `?` operator, we tag any randomly selected entity as used. Alternatively, we can use the **`%` operator (pick entity)** to _select_ a random entity from a collection rather than _use_ it.

### Data Injection

Data injection in `SCROLL` is a method to share data between entities - specifically when generated on different "branches" of the directed graph.

For example, we can use data injection to populate faction NPCs with their faction name:

```
Faction {
  name! @ [
    * The Flaming Lambs
    * The Army of Justice
  ]

  [6..13 members!] ? Npc {
    faction = &name
  }

  <html%
    <h1>{{name}}</h1>
    <h2>Members</h2>
    <ul>
    {% for member in members %}
    <li> {{ member.full_name}} </li>
    {% endfor %}
    </ul>
  %html>
}
```

To indicate the attributes we want to inject, we use a `{` curly braced `}` block immediately after the class name we want to reuse (`Npc` in this case). This tells `SCROLL` that for each used entity it picks or pops from a collection, it has to set additional injected values.

```
[6..13 members!] ? Npc {
  faction = &name
}
```

We then specify `faction` as the attribute we want to inject to and `&name` as the value, which means, copy the value from the owning entity attribute `name`. The `&` operator means _copy_ in this context.

Note that there's another way to reference attributes when injecting data by using the _pointer_ operator `*` - but we will cover this in a later post.

Now that we have injected data into selected NPCs, we can use it to enrich their description:

```
Npc {
  first_name @ [
    * Walatus
    * Ajutor
    * Sieggo
    * Milia
    * Petesia
    * Lefsy
    * Latilde
  ]
  last_name @ [
    * Bossinia
    * Brightmer
    * Ermenbald
    * Hildeward
    * Siclebald
    * Zawissius
  ]
  age @ 7d12
  faction = 0
  description! = <%
    {{first_name}} {{last_name}}
    ({{age}} years old)

    {% if faction %}
    {{faction}}
    {% endif %}
  %>
}
```

First, we declared the `faction` attribute and set it to `0` - telling the generator that this attribute is currently holding no value. We could also use the keywords `null` or `false`.

Next, in the faction `description` attribute, we check whether or not `faction` was set externally, and if it holds a value, we use it.

Note that another way of do this is using the template function `maybe`, but I will cover this in the next post.

### Scope References

In cases when an entity needs a value from one of its ancestors, we can use a scope reference:

```
Faction {
  name! @ [
    * The Flaming Lambs
    * The Army of Justice
   ]
  realm_name = :Realm.name
  realm_class = :Realm.class
  objective! = <%
    {{name}} objective is to overthrow
    {{realm_name}}'s ruler and gain control over
    the {{realm_class}}.
  %>

  [6..13 members!] % Npc {
    faction = &name
  }

  <html%
    <h1>{{name}}</h1>
    <p> {{objective}} </p>
    <h2>Members</h2>
    <ul>
    {% for member in members %}
    <li> {{ member.full_name}} </li>
    {% endfor %}
    </ul>
  %html>
}
```

We use the `:` operator followed by an ancestral class name to refer to a parent, and then use `.` to reference a specific attribute:

```
realm_name = :Realm.name
realm_class = :Realm.class
```

In this case, we created to new `faction` attributes, `realm_name` and `realm_class`, and using the parent `Realm` entity to set their values accordingly.

### Putting everything together

Copy and paste the following into the [REPL](/scroll/repl.html) editor:

```
Npc {
  first_name @ [
    * Walatus
    * Ajutor
    * Sieggo
    * Milia
    * Petesia
    * Lefsy
    * Latilde
  ]
  last_name @ [
    * Bossinia
    * Brightmer
    * Ermenbald
    * Hildeward
    * Siclebald
    * Zawissius
  ]
  full_name! = <% {{first_name}} {{last_name}} %>
  age @ 7d12
  faction = 0
  description! = <%
    {{first_name}} {{last_name}}
    ({{age}} years old)
    {% if faction %}
    <br/>
    <strong>Member of {{faction}}</strong>
    {% endif %}
  %>
}

Settlement {
  name @ [
    * Akkis
    * Bazuul
    * Devilville
    * Ecrean
    * Frostfyord
    * Khezal
  ]
  <html%
  {{title}} is home for: <ul>
  {% for npc in npcs %}
  <li> {{ npc.description }} </li>
  {% endfor %} </ul>
  %html>
}

Village (Settlement) {
  title! = "The Village of {{name}}"
  [6..12 npcs!] @ Npc
}

Town (Settlement) {
  title! = "The Town of {{name}}"
  [12..24 npcs!] @ Npc
}

Faction {
  name! @ [
    * The Flaming Lambs
    * The Army of Justice
  ]

  [6..13 members!] % Npc {
    faction = &Name
  }

  <html%
  <h1>{{name}}</h1>
  <h2>Members</h2>
  <ul>
  {% for member in members %}
  <li> {{ member.full_name}} </li>
  {% endfor %}
  </ul>
  %html>
}

Realm {
  << Npc
  name @ [
    * Ishabor
    * Zeratha
    * Inaba
  ]
  [3..6 settlements!] @@ [
    * Town
    * Village
  ]
  faction! @ Faction
}

Kingdom (Realm) {
  title! = "Kingdom of {{name}}"
}

Duchy (Realm) {
  title! = "Duchy of {{name}}"
}

main {
  realm! @@ [
    * Kingdom
    * Duchy
  ]

  <html%
    <p>
    Our realm is the
    <strong>{{realm.title}}</strong>.
    </p>

    <p>
    The realm has the following settlements:
    {% for s in realm.settlements %}
    <p> {{html(s)}} </p>
    {% endfor %}
    </p>
    {{html(realm.faction)}}
  %html>
}
```

The next post on `SCROLL` will be dedicated to template functions and how they can be used to process entity values when rendering HTML, as well as to some of the less frequently used features of the language. Until then.. Enjoy the Crawl!

Ithai
