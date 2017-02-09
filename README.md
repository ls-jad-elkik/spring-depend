# Spring-depend

Tool for analyzing spring dependencies. Such tools exist but they seemed to be bundled with complicated IDE plugins, which makes standalone usage a bit hard. This is a standalone thing. All you need is a spring application context.

```
// any instance of spring's GenericApplicationContext or one of the sub classes should work
SpringDependencyAnalyzer analyzer = new SpringDependencyAnalyzer(context);
analyzer.printReport();
```

Features:
  - `Map<String, Set<String>> getBeanDependencies()` returns a map of all the beans in your context and their dependencies. Needing lots of things is a sign of low coherence and high coupling.
  - `Map<String, Set<String>> getReverseBeanDependencies()` returns reverse dependencies. Being used a lot is a good thing; it indicates usefulness. Things that are rarely used might not need to be beans on the other hand.
  - `SimpleGraph<String> getBeanGraph()` a graph of the dependencies. (TODO: technically this is a reverse dependency graph that is generated from both the dependency and reverse dependency map. I need to add the ability to invert it.)
  - `SimpleGraph<Class<?>> getConfigurationGraph(Class<?> configurationClass)` return a graph of your `@Configuration` classes by following the imports from the specified root class.
  - `Map<Integer, Set<Class<?>>> getConfigurationLayers(Class<?> configurationClass)` returns a tree map with the configuration classes ordered in layers by their dependencies on each other. The more layers you need, the more complex your spring dependencies are. Consider refactoring them to have less interdependencies. Untangling the the most coupled beans will likely clear this up.

# Future work
When time allows, I might work on these topics a bit. Pull requests are welcome of course.

  - Neo4j export to support visulizations, querying and other goodness that comes with neo4j. Basically, spitting out some cypher should be doable.
  - Better graph implementation than the rather limited `SimpleGraph` currently included. This was a quick and dirty job. 
  - Fix the bean graph to not be a reverse dependency graph.
  - Simple metrics for coherence and coupling.
  - Test framework support so you can assert constraints on your dependencies and related metrics from a simple unit test.

# Get it from Maven Central

```
<dependency>
    <groupId>com.jillesvangurp</groupId>
    <artifactId>spring-depend</artifactId>
    <!-- check maven central for latest, Readme's get out of date so easily ... -->
    <version>0.1</version>
</dependency>
```

Note. the spring dependency in the pom is optional. This is so you can specify which spring version you use and avoid nasty conflicts. Anything recent (4.x and up) should pretty much work with this and perhaps older versions too. 

# License

[MIT](LICENSE)
