# Understand JVM GC

For us to understand the JVM GC control Java applications are of great help. Let me from the operation and maintenance point of view, the JVM online relevant information are summarized as follows, in order to deepen the understanding of the JVM GC. 
If the wrong place, please correct me Tell me.

###JVM memory usage classification

JVM's memory partition relationship:

* [JVM whole heap memory] = the young generation + old generation
* [JVM] = entire memory (heap memory) + non-heap memory = (the young generation + old generation) + permanent generation

###About the young generation, the old generation, lasting generations

For the JVM, the memory is divided into three areas: the young generation, the old generation and permanent generation. The young generation and the old generation used to store Java process variables, lasting generations to put Java class information. 
We generally focus on the young generation and the old generation. JVM generational can use the following figure shows this: