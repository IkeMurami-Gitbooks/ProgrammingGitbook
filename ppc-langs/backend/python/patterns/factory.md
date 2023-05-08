# Factory

```python
class A(object):
   ...
   
class B(A):
   ...
   
class C(B):
   ...
   
// Not
a = A()
b = B()
c = C()

// Factory
class Factory:
   def create(self, what: str):
      if what == 'a':
         return A()
      if what == 'b':
         return B()
      ...
      
factory = Factory()
a = factory.create('a')
```
