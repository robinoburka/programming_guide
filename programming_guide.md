# Programming guide

## Intro

*Software engineering is what happens to programming when you add time and other programmers.*

You've heard it many times: *You write the code once, but you read it over and over again.* That's the metric I want to optimise.

## Basic principles

### The best code is a boring code.
- If I had to think about it, it can be done better.
- In a good codebase, there should be zero surprises.

You don't have to show how smart you are (We already know it!). Make the code easily digestible. My rule of thumb is, "It should be understandable even to a first-semester student".

**Wrong:**
```python
class RateLimit(object):
    expiration_window = 10
    remaining = property(lambda x: x.limit - x.current)
    over_limit = property(lambda x: x.current >= x.limit)
```

### Naming
- It should be clear what a function does from its name.

**Wrong:**
```python
def is_ip_address(value):
    try:
        return ipaddress.ip_address(value)
    except ValueError:
        return False
```

### Be strict with the tools you have
- It's okay to not use a language's feature if it harms readability.
- There might be a design pattern, but that design pattern may not necessarily be a good design pattern.

**Example**: Don't use `functools.partial`. It's always hard to read.

## Code structure

### Minimise public interface
- Every entity - package(s), sub-packages, modules, classes, even functions - should expose just the bare minimum of components.
- Export well-prepared pieces with complete functionality (avoid packing 20 helper functions à la "Figure it out yourself").
- Cover public interface by tests. Public interface is the contract that needs to be validated and kept.

### Clear data on borders
- Data types on code borders should be well-defined.
- No loose defined dicts, no variable length tuples.
- Ideally use `dataclass` or some model we can rely on.
- Also, data and data manipulation should be ideally separated. Dataclass and function is always better than class that is partially data and some random behaviour stuck to it.

**Example**: A service client returns its own model, not `requests.Response` object.

**Note**: I'm not alone with this philosophy. Here is a quote from Linus Torvalds (https://lwn.net/Articles/193245/):
> …git actually has a simple design, with stable and reasonably well-documented data structures. In fact, I'm a huge proponent of designing your code around the data, rather than the other way around, and I think it's one of the reasons git has been fairly successful […] I will, in fact, claim that the difference between a bad programmer and a good one is whether he considers his code or his data structures more important. Bad programmers worry about the code. Good programmers worry about data structures and their relationships.

### Use clients/repositories and DI
- Clients or repositories should have interface based on business requests. A client with variable length of parameters is a wrong client.
- We can use dummy implementation of such clients/repositories in test. It reduces the need of using mocks.
- Assemble these dependencies using DI, to keep good testability.

### Keep your codebase in "easy to switch" state
- Use proper isolation for very uncertain decision. A library could be wrapped in own repository. A tool could be wrapped by command definition in `Makefile`.
- Changing pieces of your code must be easy all the time. Once you've locked yourself in one tool/library, you have a problem.

## Philosophy
- All dogmas are wrong!
    - DRY. Yeah, but rather write something two times than having crazy, hard to read structures adding 100+ LoC.

## Practical advice

### Relative imports
- Use relative imports. Multiple dots on the beginning of the import statement emphasises import across borders.

### Explicit comparison
- Use explicit comparisons - e.g. `val is not None`. It can prevent mistakes. It can save you some performance troubles.

### No code in `__init__` file
- Keep thing as clear as possible. Use `__init__` files for imports, not for programming.

## Don't let pass in MRs
- Obviously wrong functions
    - Super long functions
    - Functions with too many and/or inappropriate side-effects

***
[Programming guide](https://github.com/robinoburka/programming_guide) © 2024 by Robin Obůrka is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/)

