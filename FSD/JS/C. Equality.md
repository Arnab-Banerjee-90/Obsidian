
## 1. Abstract equality comparison
---

### 1.1. Two basic principles about the `==` operator
---
- When x and y are of the same type then it performs the Strict Equality Comparison `===`
- When x and y are of different types it allows for coercion following the rules mentioned below

### 1.2. Coercion Rules for the `==` Operator
---
- `null == undefined` or `undefined == null` returns `true`
- `null` and `undefined` are not comparable with any other datatype.
- The `NaN` value is not equal to any other value, not even another `NaN` 
-  If the operands are **Number** and **String**, then the `ToNumber` abstract operation is called on the String and it is converted to a number.
- If any of the operand is Boolean then it is converted into number using the `ToNumber` Abstract operation.
- If any of the operand is `object` type and the other one is either Number, String or Symbol, then the object type is converted into primitive type using the `ToPrimitive` Abstract Operation
- So we can observe from the above discussion that `==` compares
	- Numbers with Numbers
	- Or Strings with Strings
- If any one of the operands are `number` or `boolean` then a numeric comparison will occur

### 1.3. Some corner cases of the `==` operator
---
#### 1.3.1. \[ ] == ! \[ ] returns `true`
---
- To evaluate !\[ ] , \[ ] is going to be first be converted into boolean using the `ToBoolean` abstract operation lookup. which converts it to true as [ ] is an array. so `!true` becomes `false`
- now we have \[ ] == false, now according to the == coercion rule false is going to be coerced into 0
- now we have \[ ] == 0, now \[ ] is going to be coerced into its primitive value "".
- now we have "" == 0, so now "" will be converted to number using `ToNumber` --> 0
- So finally we get 0 == 0, so 0 === 0 is invoked.

#### 1.3.2. \[ ] == true returns `false`
---
- true is first coerced into 1
- \[ ] is then coerced into “” ⇒ 0
- now we have 0 == 1 which get converted to 0 === 1 which returns false

### 1.4. What to avoid when working with the == operator
---
- Avoid == with 0 or with strings with only white spaces in it
- Avoid == with non-primitives
- Avoid == with true or false values
- Avoid using == when the types of the operands are not known, or cannot be narrowed down