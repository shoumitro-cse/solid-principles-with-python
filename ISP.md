## Interface Segregation Principle:

Instead of one fat interface, create interfaces as small as possible based on a group of methods each one serving one submodule. 
For example, this is a bad example:

```
from abc import ABCMeta, abstractmethod

class EventParser(metaclass=ABCMeta):
	
	@abstractmethod
	def from_xml(xml_data: str):
		"Parse an event from XML"
	
	@abstractmethod
	def from_json(json_data: str):
		"Parse an event from JSON"
```

Why this is bad? Because what if a class that implements this interface doesn’t need to use the from_json method? Here we force a class to implement an interface they don’t want to use. The true approach is to create 2 different interfaces for those functions.


```
from abc import ABCMeta, abstractmethod
class XMLEventParser(metaclass=ABCMeta):
	
	@abstractmethod
	def from_xml(xml_data: str):
		"Parse an event from XML"
	
class JSONEventParser(metaclass=ABCMeta):
	
	@abstractmethod
	def from_json(json_data: str):
		"Parse an event from JSON"


class RestClient(JSONEventParser):
	...

class SoapClient(XMLEventParser):
	...

```

