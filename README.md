Butter Knife
============

View "injection" library for Android which uses annotation processing to
generate code that does direct field assignment of your views.



Introduction
------------

Android developers, like most developers, are lazy and do not want to write a
bunch of code which looks like this:

```java
class ExampleActivity extends Activity {
  TextView title;
  TextView subtitle;
  TextView footer;

  @Override public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.simple_activity);
    title = (TextView) findViewById(R.id.title);
    subtitle = (TextView) findViewById(R.id.subtitle);
    footer = (TextView) findViewById(R.id.footer);
    // ...
  }
}
```

Instead, they turn to helper libraries that they likely do not fully
understand. These libraries might use annotations and magic to allow you to
condense your code to:

```java
class ExampleActivity extends Activity {
  @Magic(R.id.title) TextView title;
  @Magic(R.id.subtitle) TextView subtitle;
  @Magic(R.id.footer) TextView footer;

  @Override public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.simple_activity);
    // ...
  }
}
```

While it looks pretty, magic is for children (and it also comes with a heavy
runtime penalty).

Instead we can leverage a powerful part of the `javac` compiler to generate
the first example's `findViewById` boilerplate while still allowing us to keep
the terseness of the annotations:

```java
class ExampleActivity extends Activity {
  @InjectView(R.id.title) TextView title;
  @InjectView(R.id.subtitle) TextView subtitle;
  @InjectView(R.id.footer) TextView footer;

  @Override public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.simple_activity);
    Views.inject(this);
    // ...
  }
}
```

In place of magic we now call an `inject` method. This method delegates to
generated code that you can see and debug:

```java
public void inject(ExampleActivity activity) {
  activity.subtitle = (android.widget.TextView) activity.findViewById(2130968578);
  activity.footer = (android.widget.TextView) activity.findViewById(2130968579);
  activity.title = (android.widget.TextView) activity.findViewById(2130968577);
}
```

Some people call this view injection and lump it along with traditional
dependency injection frameworks. They are wrong in nomenclature, but perhaps
there exists some necessity for this terseness.

__Remember: A butter knife is like [a dagger][1] only infinitely less sharp.__

*Do not take this library too seriously, but it may be useful for some things.*



Bonus
-----

Also included is a helper method for simplifying code which still has to call
`findViewById` on either a `View` or `Activity`:

```java
View view = LayoutInflater.from(context).inflate(R.layout.thing, null);
TextView firstName = Views.findById(view, R.id.first_name);
TextView lastName = Views.findById(view, R.id.last_name);
ImageView photo = Views.findById(view, R.id.photo);
```

Add a static import for `Views.findById` and enjoy even more fun.



Download
--------

Download [the latest JAR][2] or grab via Maven:

```xml
<dependency>
  <groupId>com.jakewharton</groupId>
  <artifactId>butterknife</artifactId>
  <version>(insert latest version)</version>
</dependency>
```



License
-------

    Copyright 2013 Jake Wharton

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.



 [1]: http://square.github.com/dagger/
 [2]: http://repository.sonatype.org/service/local/artifact/maven/redirect?r=central-proxy&g=com.jakewharton&a=butterknife&v=LATEST