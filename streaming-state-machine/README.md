Example: Event pattern detection with Apache Flink
==================================================

This example assumes a scenario inspired by IT security or network intrusion detection.
 
Events in streams (generated by devices and services, such as firewalls login-, and
authentication services) are expected to occur in certain patterns. Any deviation from
these patterns indicates an anomaly (attempted intrusion) that the streaming system should
recognize and that should trigger an alert.

The event patterns are tracked per interacting party (here simplified per source IP address)
and are validated by a state machine. The state machine's states define what possible
events may occur next, and what new states these events will result in.

The streaming program that analyzes the event stream is depicted in the diagram below.
The core logic is in the `flatMap` function, which runs the state machines per IP address.

The main class of this example program is `com.dataartisans.flink.streamingdemo.StreamingDemo`


NOTE: The source may be Kafka, but to make this example self-contained, it comes with
a data generator source that produces a sample event stream (with occasional anomalies)
and that can be used without the need to have a Kafka installation.

<pre>
 [ KAFKA-PART-1] --> source --> partition -+---> flatMap(state machine) --> sink
                                            \/
                                            /\
 [ KAFKA-PART-2] --> source --> partition -+---> flatMap(state machine) --> sink
</pre>


The following diagram depicts the state machine used in this example.

<pre>
           +--<a>--> W --<b>--> Y --<e>---+
           |                    ^         |     +-----<g>---> TERM
   INITIAL-+                    |         |     |
           |                    |         +--> (Z)---<f>-+
           +--<c>--> X --<b>----+         |     ^        |
                     |                    |     |        |
                     +--------<d>---------+     +--------+
</pre>


Feedback for this example can be sent to mailto:sewen@apache.org

Docs on Apache Flink can be found at http://flink.apache.org

Check Apache Flink's mailing lists for help on Flink: mailto:user@flink.apache.org
