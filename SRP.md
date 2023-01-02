## Single Responsibility Principle(SRP):

For Turkish readers, I will summarize this principle with only one sentence: “Nerede çokluk orada ….”:)

A software component must have only one responsibility. It must have only one reason to change. If you need to modify the class 
for different reasons, this means something( an abstraction) is missing and you need to fix this. If a class has lots of 
responsibilities this class is often called a “God class” because they know too much or do too much, more than they should do. 
This type of class is very hard to maintain and the smaller the class, the better.

What we strive to achieve here is that classes are designed in such a way that most of their properties and their attributes are 
used by their methods, most of the time. When this happens, we know they are related concepts, and therefore it makes sense to 
group them under the same abstraction.

For example, when you look at the methods of the class and see there are some unrelated functions (with each other), you can tell 
that this class has to be broken down.

![](https://github.com/shoumitro-cse/solid-principles-with-python/blob/main/SRP.webp?raw=true)

Single Responsibility Principle Example
Take a loot at the SystemMonitor class. You can see there are three different reasons to make a change in this class.
Maybe we decide to change how we load the activities, parse the events, or how we send the events (maybe we’ve decided to use 
Redis Streams instead of Kafka).

The picture on the right is better and reuseable. If we need to change something in how we load the data, we will only modify one 
class and the other classes will know nothing if we do not break the contract. Additionally, we can create an interface for let’s 
say loading events with a load function and we can use that interface to create classes with different loading codes 
(maybe loading from different sources). And we can use these classes as a plug-and-play.

The classes could have more than one method as long as they serve the same purpose.

Try with one big monolithic class first and then try to separate it. This will help to get a clearer picture of the new abstraction 
that you need to create.

## youtube
https://www.youtube.com/watch?v=wGzHrKI8pjM&list=PLKOt2_LymewDngmqE5oL-7KpQ97k958Iq&index=10
