Grails plugin providing annotation driven usage of yammer metrics for grails projects.
=======
The primary objective of the plugin is to remove boiler plate code when adding metrics to your project.

Annotations
-------
@Timed
This annotation can be added to any method you wish to be timed.  @Timed uses sensible defaults to create an instance of
com.yammer.metrics.core.Timer and the associated code to update it from within your method body.

Before
```
class SomeService{
  public String serve(def foo, def bar) {
     return "OK";
  }
}
```

Timed the Java Way
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

Timed the Grails Way
```
class SomeService{
  @com.yammer.metrics.groovy.Timed
  public String serve(def foo, def bar) {
    return "OK"
  }
}
```

@Metered
This annotation can be added to any method you wish to be metered.  @Metered uses sensible defaults to create an instance of
com.yammer.metrics.core.Meter and the associated code to update it from within your method body.


@Metered
If you want to create a meter on a method simply add @com.yammer.metrics.groovy.Metered to the method.
This will create a meter on the class to record calls to that method the the necessary method calls


Before
```
class SomeService{
  public String serve(def foo, def bar) {
    return "OK";
  }
}
```

Metered the Java Way
```
class SomeService{
  private final Meter serveMeter = Metrics.newMeter(SomeService.class, "serveMeter", "serveMeter", TimeUnit.MILLISECONDS, TimeUnit.SECONDS);
  public String serve(def foo, def bar) {
    serveMeter.mark();
    return "OK";
  }
}
```

Metered the Grails Way
```
class SomeService{

  @com.yammer.metrics.groovy.Meter
  public String serve(def foo, def bar) {
    return "OK"
  }
}
```

Note: Annotations can be safely combined.

TODO
-------
 * Support more of the underlying metric parameters via annotations
 * Annotations to support other yammer metrics
 * Automatic controller or service metrics
 * Some gui component (different plugin?)


License
-------
 Published under Apache Software License 2.0, see LICENSE

Notes
-------
The plugin provides a build dependency on the 'metrics-core' java project.  The plugin does not interfeer with any
existing usages of those tools, but simply provides an easy way to create the metrics.
For detailed documentation on the yammer metrics package see:

http://metrics.codahale.com/index.html


Acknowledgments
-------
 Thanks to Jeff Ellis for creating the grails-yammer-metrics plugin which got me interested in yammer-metrics
 Thanks to Coda Hale and all the guys/gals at Yammer for creating the metrics project.