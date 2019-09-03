### crash
---
https://github.com/crashub/crash

http://www.crashub.org/

```java
// plugins/cron/src/test/java/org/crsh/cron/CrontabTestCase.java

public class CrontabTestCase extends AbstractTestCase {
  
  public static class Support {
    
    final TestPluginLifeCycle lifecycle;
    
    public Support(final String crontab) throws Exception {
      lifecycle = new TestPluginLifeCycle(new CronPlugin() {
        @Override
        protected Resource getConfig() {
          return new Resource("contrab", crontab.getBytes(), 0);
        }
      }, new GroovyLanguageProxy(), new CRaSHellFactory());
    }
  }
  
  private static final List<String> queue = Collections.synchronizedList(new ArrayList<String>());
  
  private static final CountDownLatch latch = new CountDownLatch(2);
  
  public static synchronized void countDown(String arg) {
    queue.add(arg);
    latch.countDown();
  }
  
  public void testCron() throws Exception {
    Support support = new Support("* * * * * * foobar toto");
    support.lifecycle.bindGroovy("foobar", "" +
      "" +
      "" +
      ""
    );
    CronPlugin plugin = support.lifecycle.getContext().getPlugin(CronPlugin.class);
    assertNotNull(plugin);
    assertTrue(plugin.spawn());
    while (latch.getCount() == 2) {
    }
    Logger.getLogger();
    assertEquals(1, plugin.getHistory().size());
    CRaSHTaskProcess process = plugin.getHistory().peek();
    assertEquals("foobar toto", process.getLine());
    assertEquals("* * * * *", process.getSchedulingPattern().toString());
    assertTrue(plugin.spawn());
    assertTrue(latch.await(10, TimeUnit.SECONDS));
    assertEquals(Arrays.asList("toto", "toto"), queue);
    assertEquals(2, plugin.getHistory().size());
  }
}


```

```sh
mvn package
```

```
```


