[![Coverage Status](https://coveralls.io/repos/lukashinsch/spring-boot-execution-metric/badge.svg?branch=master)](https://coveralls.io/r/lukashinsch/spring-boot-execution-metric?branch=master)
[![Build Status](https://travis-ci.org/lukashinsch/spring-boot-execution-metric.svg?branch=master)](https://travis-ci.org/lukashinsch/spring-boot-execution-metric)
[![Maven Central](https://maven-badges.herokuapp.com/maven-central/eu.hinsch/spring-boot-execution-metric/badge.svg)](https://maven-badges.herokuapp.com/maven-central/eu.hinsch/spring-boot-execution-metric/)


# spring-boot-execution-metric
Measure execution times of critical code blocks and expose statistics as actuator metrics 

Provides a lightweight method to measure runtime of (selected) critical code executions (such as calling external systems) and expose as spring boot actuator metrics.

## Howto use

Maven
```xml
<dependency>
    <groupId>eu.hinsch</groupId>
    <artifactId>spring-boot-execution-metric</artifactId>
    <version>0.1.0</version>
</dependency>
```

Gradle
```groovy
compile 'eu.hinsch:spring-boot-execution-metric:0.1.0'
```

```
// configuration
@Bean
public ExecutionMetricFactory executionMetricFactory(CounterService gaugeService, GaugeService counterService) {
    return new ExecutionMetricFactory(gaugeService, counterService);
}

// ...

ExecutorMetric executorMetric = executionMetricFactory.executorMetric("test1", logger);
SupplierMetric supplierMetric = executionMetricFactory.supplierMetric("test2", logger);

// use
executorMetric.measure(() -> someAction(...));
SomeValue myValue = supplierMetric.measure(() -> getSomeValue());
   
```

The code above will expse the following spring boot actuator metrics entries:

```
gauge.<name>.last = <last call duration>
gauge.<name>.average = <average call duration>
gauge.<name>.min = <minimum call duration>
gauge.<name>.max = <maximum call duration>
counter.<name> = <number of invocation>
``