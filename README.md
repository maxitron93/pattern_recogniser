# Context
Built a simple pattern recogniser for fun. The program identifies the pattern and predicts the next number in a given sequence. For example:

input: [3, 10, 29, 66, 127]

output: The next number is 218. The pattern is n ^ 3 + 2

To give it a try, clone index.html and load it in the browser. You can try out different sequences by changing `numbersArray`. The program will log the results in the console.

This will only work when:
1. the pattern is n^x+y or n^x*y
2. the first number is 1^x+y or 1^x*y
3. the exponent needs to be a whole number (x cannot be a decimal)
4. it should work for all values of y, but it doesn't because of tiny rounding errors :(
4. there are at least two more numbers in the sequence than the exponent. e.g. if the exponent is 5, there must be at least 7 numbers in the sequence.

# Code

```js
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>

  <script>
    // let numbersArray = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    // let numbersArray = [3, 12, 27, 48, 75, 108, 147, 192, 243, 300]
    // let numbersArray = [0.25, 2, 6.75, 16, 31.25, 54, 85.75, 128, 182.25, 250]
    // let numbersArray = [0.15, 2.4, 12.15, 38.4, 93.75, 194.4, 360.15, 614.4, 984.15, 1500]
    // let numbersArray = [17, 48, 259, 1040, 3141, 7792, 16823, 32784, 59065, 100016]
    // let numbersArray = [0.1, 1.6, 8.1, 25.6, 62.5, 129.6, 240.1, 409.6, 656.1, 1000.0]
    // let numbersArray = [0.1, 1.6, 8.1, 25.6, 62.5, 129.6, 240.1, 409.6, 656.1, 1000.0]
    // let numbersArray = [1.0, 256.0, 6561.0, 65536.0, 390625.0, 1679616.0, 5764801.0, 16777216.0, 43046721.0, 100000000.0]
    // let numbersArray = [17, 48, 259, 1040, 3141, 7792, 16823, 32784, 59065, 100016]
    
    // let numbersArray = [1, 4, 9, 16]
    // let numbersArray = [4, 8, 12, 16, 20]
    let numbersArray = [3, 10, 29, 66, 127]
    
    
    // This will only work when:
    // 1. the pattern is n^x+y or n^x*y
    // 2. the first number is 1^x+y or 1^x*y
    // 3. the exponent needs to be a whole number (x cannot be a decimal)
    // 4. it should work for all values of y, but it doesn't because of tiny rounding errors :(
    // 4. there are at least two more numbers in the sequence than the exponent. e.g. if the exponent is 5, there must be at least 7 numbers in the sequence.
    
    // Calculate the difference between the numbers
    
    let differencesArray = numbersArray.map(function(current, index, array) {
      if (index > 0) {
        return (current - array[index - 1])
      }
    })
    differencesArray.shift()
    
    let differenceCounter = 0;
    
    while (differencesArray[0] !== 0) {
      differencesArray = differencesArray.map(function(current, index, array) {
        if (index > 0) {
          return (current - array[index - 1])
        }
      })
      differencesArray.shift()
      differenceCounter++
    }
    
    const exponent = differenceCounter
    
    const afterExponent = numbersArray.map(function(current, index, array) {
      return (current - (index + 1) ** exponent)
    })
    
    let operator
    let adderOrMultiplier
    
    if (afterExponent[1] - afterExponent[0] === 0) {
       operator = "plus"
       adderOrMultiplier = afterExponent[0]
    } else {
      operator = "multiply"
      adderOrMultiplier = afterExponent[0] + 1
    } 
    
    let nextNumber
    
    if (operator === "plus") {
      nextNumber = (numbersArray.length + 1) ** exponent + adderOrMultiplier;
      console.log(`The next number is ${nextNumber}. The pattern is n ^ ${exponent} + ${adderOrMultiplier}`)
    } else if (operator === "multiply") {
      nextNumber = (numbersArray.length + 1) ** exponent * adderOrMultiplier
      console.log(`The next number is ${nextNumber}. The pattern is n ^ ${exponent} * ${adderOrMultiplier}`)
    } 

  </script>
</body>
</html>
```