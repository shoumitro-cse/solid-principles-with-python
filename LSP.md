## Liskov’s Substitution Principle

The formal definition is:
if S is a subtype of T, then objects of type T may be replaced by objects of type S, without breaking the program.


I will begin with a rule special to Python and then show the general example. Why there is a special rule for Python? 
Because no one can stop you from doing this:

```
class SuperClass:
	def check(name: str)-> str:
		return name

class SubClass(SuperClass):
	def check(name: dict) -> dict:
		return name
		
class AnotherSubclass(SuperClass):
	def check(name: str, surname: str) -> tuple:
		return name, surname
```

The program will work buuut… it will eventually crash somewhere. So, this is the special rule for Python because it allows doing this… How to protect yourself from these types of situations? The answer is holy python tool mypy and pylint. Both tools help you to find those mistakes and more!

The second rule comes from the design by contract principle:
A subclass can never make preconditions stricter than they are defined in the parent class
A subclass can never make postconditions weaker than they are defined in the parent class

Let’s say your base class checks a dictionary and raises an Exception if the dictionary doesn’t contain the key “session_id”. A subclass of this class cannot say “The dictionary has to contain a key “class_id” too”. And if your base class checks if the returned value is a dictionary with a key “result”, you can’t lift this restriction in the subclass.

Now let’s give a more general example independent from 🐍 . Let’s say we have a class structure like this:

```
from abc import ABC, abstractmethod

class Member(ABC):
    def __init__(self, name: str, age: int):
        self.name = name
        self.age = age

    @abstractmethod
    def save_database(self):
        pass


class Teacher(Member):
    def __init__(self, name: str, age: int, teacher_id: str):
        super().__init__(name, age)
        self.teacher_id = teacher_id

    def save_database(self):
        print("Saving teacher data to database")


class Student(Member):
    def __init__(self, name: str, age: int , student_id: str):
        super().__init__(name, age)
        self.student_id = student_id

    def save_database(self):
        print("Saving student data to database")
        
# We can successfully run this code with this structure:
from typing import List
members: List[Member] = []
members.apped(Student('nusret',23,"12345"))
members.apped(Teacher('Teacher_nusret',23,"12345"))
for member in members:
	member.save_database()
```
	
Now let’s modify the code and add one more abstract method pay(). For the sake of easiness, 
I will assume students cannot pay because it is free for them.


```
from abc import ABC, abstractmethod


class Member(ABC):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @abstractmethod
    def save_database(self):
        pass

    @abstractmethod
    def pay(self):
        pass


class Teacher(Member):
    def __init__(self, name, age, teacher_id):
        super().__init__(name, age)
        self.teacher_id = teacher_id

    def save_database(self):
        print("Saving teacher data to database")

    def pay(self):
        print("Paying")


class Manager(Member):
    def __init__(self, name, age, manager_id):
        super().__init__(name, age)
        self.manager_id = manager_id

    def save_database(self):
        print("Saving manager data to database")

    def pay(self):
        print("Paying")


class Student(Member):
    def __init__(self, name, age, student_id):
        super().__init__(name, age)
        self.student_id = student_id

    def save_database(self):
        print("Saving student data to database")

    def pay(self):
        raise NotImplementedError("It is free for students!")
        
        
from typing import List
members: List[Member] = []
members.apped(Student('nusret',23,"12345"))
members.apped(Teacher('Teacher_nusret',23,"12345"))
for member in members:
	member.pay()
```


Now this example cannot work! Both classes are a subclass of the Member class but the Student class will throw exceptions or not work as expected. This is against the rule. If a Member has to pay, we can clearly say that a Studentcannot be a Member. To solve this problem, we can remove the pay() method from Member and create a new abstract class Payer. Very sorry for the naming but I couldn’t come up with a better name... Now our structure should be like this:


```
from abc import ABC, abstractmethod

class Payer(ABC):
    @abstractmethod
    def pay(self):
        pass

class Member(ABC):
    def __init__(self, name, age):
        self.name = name
        self.age = age

    @abstractmethod
    def save_database(self):
        pass


class Teacher(Member, Payer):
    def __init__(self, name, age, teacher_id):
        super().__init__(name, age)
        self.teacher_id = teacher_id

    def save_database(self):
        print("Saving teacher data to database")

    def pay(self):
        print("Paying")


class Manager(Member, Payer):
    def __init__(self, name, age, manager_id):
        super().__init__(name, age)
        self.manager_id = manager_id

    def save_database(self):
        print("Saving manager data to database")

    def pay(self):
        print("Paying")


class Student(Member):
    def __init__(self, name, age, student_id):
        super().__init__(name, age)
        self.student_id = student_id

    def save_database(self):
        print("Saving student data to database")


payers: List[Payer] = [Teacher("John", 30, "123"), Manager("Mary", 25, "456")]
for payer in payers:
    payer.pay()
```

Now, this will work and it is appropriate for the principle!


