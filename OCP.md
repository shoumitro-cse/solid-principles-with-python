## Open-Closed Principle

A class must be open to extension but closed to modification. When we want to add new things to our model, we only want to add new things and not change anything existing that is closed to modification. This principle is very easy to understand with a concrete example. Let’s start with a bad example:

```
class Employee:
    
    def __init__(self, name: str, salary: str):
        self.name = name
        self.salary = salary
    
    
class Tester(Employee):
    
    def __init__(self, name: str, salary: str):
        super().__init__(name, salary)
    
    def test(self):
        print("{} is testing".format(self.name))

class Developer(Employee):
    
    def __init__(self, name: str, salary: str):
        super().__init__(name, salary)
    
    def develop(self):
        print("{} is developing".format(self.name))


class Company:
    
    def __init__(self, name: str):
        self.name = name
    
    def work(self, employee):
        if isinstance(employee, Developer):
            employee.develop()
        elif isinstance(employee, Tester):
            employee.test()
        else:
            raise Exception("Unknown employee")  
```


Why this is a bad example? Think about it for a second. How can you add an Analyst to this "Company"? Well, you can create a new class as a subclass of the Employee and then add one more if to Company’s work function. Well yes, it works but did you really like the solution? What will you do when new types of Employees come? This if-elif chain eventually will become hell and very hard to maintain. Now let’s see how we can fix this:


```
from abc import ABC, abstractmethod


class Employee(ABC):

    def __init__(self, name: str, salary: str):
        self.name = name
        self.salary = salary

    @abstractmethod
    def work(self):
        pass


class Tester(Employee):

    def __init__(self, name: str, salary: str):
        super().__init__(name, salary)

    def test(self):
        print("{} is testing".format(self.name))

    def work(self):
        self.test()

class Developer(Employee):

    def __init__(self, name: str, salary: str):
        super().__init__(name, salary)

    def develop(self):
        print("{} is developing".format(self.name))

    def work(self):
        self.develop()

class Company:

    def __init__(self, name: str):
        self.name = name

    def work(self, employee: Employee):
        employee.work()



carbon = Company("Carbon")
developer = Developer("Nusret", "1000000")
tester = Tester("Someone", "1000000")
carbon.work(developer) # Will print Nusret is developing
carbon.work(tester) # Will print Someone is testing
```

What changed? Now Employee is an abstract class and it has an abstract method called work. All subclasses of this class have to implement a work function. Developer calls its develop method and Tester calls its test method. In the company, all we had to do is calling the work() method of the given employee. If I need to add a new Employee like an Analyst, all I need to do is implement the work() method and done!


