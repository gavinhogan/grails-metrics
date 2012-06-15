Grails Plugin aiming to provide easy usage for the Yammer java metrics project.
=======

The plugin provides a build dependency on the 'metrics-core' java project.  The plugin does not interfer with any
existing usages of those tools, but simply provides an easy way to create the metrics.
For detailed documentation on the yammer metrics package see:

http://metrics.codahale.com/index.html

Annotation Driven Implementations
-------
@Timed

If you want to create a timer on a method simply add @com.yammer.metrics.groovy.Timed to the method.
This will create a timer on the class to time that method and establish the boilerplate to call it around your
method.

Old Way
```
class SomeService{
  private final Timer serveTimer = Metrics.newTimer(SomeService.class, "serveTimer", TimeUnit.MILLISECONDS, TimeUnit.SECONDS);
  public String serve(def foo, def bar) {
    final TimerContext context = responses.time();
    try {
        return "OK";
    } finally {
        context.stop();
    }
  }
}
```

New Way
```
class SomeService{

  @com.yammer.metrics.groovy.Timed
  public String serve(def foo, def bar) {
    return "OK"
  }
}
```


@Metered

If you want to create a meter on a method simply add @com.yammer.metrics.groovy.Metered to the method.
This will create a meter on the class to record calls to that method the the necessary method calls

Old Way
```
class SomeService{
  private final Meter serveMeter = Metrics.newMeter(SomeService.class, "serveMeter", "serveMeter", TimeUnit.MILLISECONDS, TimeUnit.SECONDS);
  public String serve(def foo, def bar) {
    serveMeter.mark();
    return "OK";
  }
}
```

New Way
```
class SomeService{

  @com.yammer.metrics.groovy.Meter
  public String serve(def foo, def bar) {
    return "OK"
  }
}
```

Note: These annotations can be safely combined.

TODO
-------
 * Provide more control on each of the annotations
 * Automatic controller metrics
 * Some gui component (different plugin?)


License
-------
 Published under Apache Software License 2.0, see LICENSE


Acknowledgments
-------
 Thanks to Jeff Ellis for creating the grails-yammer-metrics plugin which got me interested in yammer-metrics
 Thanks to Coda Hale and all the guys/gals at Yammer for creating the metrics project.