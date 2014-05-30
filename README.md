# Varys
Varys allows data-parallel *frameworks* (e.g., Hadoop, Spark, YARN) to use coflows through a simple API and performs coflow scheduling for the entire cluster. **User jobs do not require any modification** to take advantage of Varys; only the framework needs a few changes.

To learn more, visit <http://varys.net/>

## What is a Coflow?
Communication in data-parallel applications often involves a collection of parallel flows. Traditional techniques to optimize flow-level metrics do not perform well in optimizing such collections, because the network is largely agnostic to application-level requirements. A *coflow* represents such collections of parallel flows to convey job-specific communication requirements – for example, minimizing completion time or meeting a deadline – to the network and enables application-aware network scheduling. 
More information on the coflow abstraction can be found at <http://www.mosharaf.com/wp-content/uploads/coflow-hotnets2012.pdf>

## Building Varys
Varys is built on `Scala 2.9.3`. To build Varys and example programs using it, run

	./sbt/sbt package

## Example Programs
Varys also comes with several sample programs in the `examples` directory. For example, the `SenderClient*` and their counterparts `ReceiverClient*` programs show how to use coflows on different data sources like disk or memory. 

To run any pair of them (e.g., one creating random coflows) in your local machine, first start Varys by typing

	./bin/start-all.sh

Now, go to <http://localhost:16016/> in your browser to find the `MASTER_URL`.

Next, start the sender and the receiver

	./run varys.examples.SenderClientFake <MASTER_URL>
	./run varys.examples.ReceiverClientFake <MASTER_URL> COFLOW-000000

Finally, stop Varys by typing

	./bin/stop-all.sh

While these examples by themselves do not perform any task, they show how a framework might use them (e.g., a mapper being the sender and reducer being the receiver in MapReduce). Note that, we are manually providing the `CoflowId` (`COFLOW-000000`) to the reciever, which should be provided by the framework driver (e.g., `JobTracker` for Hadoop or `SparkContext` for Spark).

Take a look at the `BroadcastService` example that does a slightly better job in containing everything (driver, sender, and receiver) in the same application.

## Dependency Information
TBA