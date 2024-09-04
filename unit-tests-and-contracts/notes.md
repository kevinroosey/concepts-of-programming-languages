## Unit tests

> Unit tests provide sample inputs and test expected outputs of code in isolation

- Specify expected behavior
- Documentation
    - how to init and use code
    - what to expect from legal and illegal outputs
- Prevent quality regression


### Unit tests using ScalaTest
```
def factorial(n: Int) : Int = 
  var result = 1
  for i <- 1 to n do
    result *= i    
  result  
```

```
class FactorialTests 
  extends AnyFlatSpec with should.Matchers:
  "Factorial" should "compute 0! correctly" in {
    factorial(0) should be (1)
  }
  it should "compute 1! correctly" in {
    factorial(1) should be (1)
  }
  it should "compute 3! correctly" in {
    factorial(3) should be (6)
  }
```


## Contracts

> Contracts are formal and verifiable interface specs
    - Preconditions 
    - Postconditions
    - Invariants


```
def factorial(n: Int) : Int = {
    require (n >= 0)
    var result = 1
    for i <- 1 to n do
        result *= i
    result
} ensuring {
    _ == factorialContract(n)
}
```

```
def factorialContract(n: Int) : Int = {
  var result = 1
  for i <- 1 to n do
    result *= i    
  result
}
```


### Summary

- Unit tests execute code with some inputs and test for expected outputs
    - Benefits: documents known and expected use and behavior of software
    - Drawbacks: finite number of tests
    - Best for: regression testing
- Contracts
    - Benefits: show correctness for all possible inputs and executions
    - Drawbacks: runtime verification adds computational overhead, theorem proving and model checking are extra effort
    - Best for: correctness-critical applications
