# Clean Code Summary
## Code Smells
### General
#### Duplication
Duplication is difficult to maintain, leads to bugs and bloat. If identified it's an opportunity to refactor, use the strategy pattern or use polymorphism instead of if/else stacks.
#### Wrong Abstraction
Base classes/modules/packages should know nothing of their parents. Do not mix high and low abstraction.
#### Too much Information
The smaller and simpler an API the better, this applies to classes and any public variables or functions. This also applies to packages, modules, libraries and web apps. Hide as much as possible. Exposing too much confuses the user and complicates the use of the API.
#### Vertical Separation
In the class file, keep related functions vertically close together. Variables should be declared in as small a scope as possible and only just before they are used. Private functions should be declared just before their first use.  
#### Consistency
Be consistent with naming. Use the same name for the same concepts. Similar variables and functions should be named similarly. 
#### Clutter
Delete unused function, variables, commented out code, unused imports, unused configuration, old documentation, etc. Keep the codebase as lean as possible.
#### Artificial Coupling
General utils or constants shared between classes should be in their own class. Not bound to one and imported by the other one class.  It confuses responsibility and leads to unnecessary coupling.
#### Feature Envy
"The methods of a class should be interested in the variables and functions of the class they belong to, not the variables and functions of other classes." When a method of a class uses the accessors and mutators of that other class to manipulate the data in that other class, it's overreach. We want the internals of one class to be encapsulated. I've heard that some engineers judge this to be a balancing act. For example, coupling between two classes is sometimes avoided by one class using the exposed internal state of the another class.  But I would argue that this is a bad practice. This does couple the two classes, not through accessors and mutators but through the exposed internal state.
#### Selector Arguments
The bad practice of using arguments to allow a larger function to behave differently according to those arguments instead of having multiple, smaller functions.
#### Obscured Intent
Code should be readable and expressive - not codified and opaque. Use variable, function, class and module names with that in mind. Conciseness is not the biggest priority.
#### Misplaced Responsibility
When deciding where to put code, the principle of least surprise often serves well. Where would most people expect to find it? Or at least where would the fewest people be surprised to find it!
#### Inappropriate static
Some static functions make sense, eg `Math.max(a,b)` but others do not. For example `HourlyPayCalculator.calculatePay(employee, overTimeRate)` might be better as a non-static method of the `Employee.class`. When in doubt, make the function non-static, if you make it static, make sure you would never want it to behave polymorphically.
#### Use explanatory variables
Break up calculations into intermediate values assigned to variables that hold meaningful names.
#### Function Names should say what they do
For example, `Date newDate = date.add(5)` is hard to understand. Does the 5 refer to days or months? Does it even return a new Date or is it updated in place. `date.addDays(5)` or `Date newDate = date.daysLater(5)` would be more readable.
#### Understand the algorithm
Don't be complacent. Attempt to really understand an algorithm. Tests can help but another technique is breaking down the algorithm into simple blocks.
#### Make Logical Dependencies Physical
Other classes should not assume the values held in other classes, they should share common values through mutually accessible means.
#### Prefer polymorphism to if/else switch/case
Easier to extend in terms of adding more cases but also adding more functionality in each case.
#### Follow standard conventions
The team should share a formatting document and a set of other standards (naming, class structure). Project templates and shared libraries are another good way to enforce this consistency.

#### Replace magic numbers with named constants
Name these numbers using descriptive variable names. Even values that are calculated and used in a single place eg instead of `int s = 24*60*60` use `int secondsInADay = 24*60*60` or even `int secondsInADay = hoursInADay * minutesInAnHour * secondsInAMinute` (but this might probably be overkill).
Even non-number values can be assigned to descriptive variables. For example a supermarket company might have well known test store with the storeName of `london-nw1-234` so assigning it to `THE_TEST_STORE_NAME` might be a good idea. 
#### Be precise
With floats, with rounding, with locking, closing resources, passing and returning null.
#### Structure over Convention
Enforce design decisions with structure over conventions. Structure enforces, convention does not.
#### Encapsulate conditionals
Encapsulate conditional logic with well-named functions eg `if(itShouldBeDeleted(timer))` instead of `if(timer.hasExpired() && !timer.isRecurrent())`
#### Avoid negative conditionals
Use `if(buffer.shouldCompact())` instead of `if(!buffer.shouldNotCompact())`
#### Functions should do just 1 thing
More readable, more optimisable
#### Hidden Temporal Couplings
```
public class MoogDiver {      
  Gradient gradient;      
  List<Spline> splines;      
  public void dive(String reason) {        
    saturateGradient();        
    reticulateSplines();        
    diveForMoog(reason);      
    } 
}
```

Allows for disaster as `reticulateSplines()` could be mistakenly called before `saturateGradient()`.

Alternatively, the snippet below would enforce ordering:

```
public class MoogDiver {      
  Gradient gradient;      
  List<Spline> splines;      
  public void dive(String reason) {        
    gradient = saturateGradient();        
    splines = reticulateSplines(gradient);        
    diveForMoog(splines, reason);      
    } 
}
```
#### Don't be arbitrary
Follow conventions and structure

#### Encapsulate boundary conditions
```
if(level + 1 < tags.length){
  parts = new Parse(body, tags, level + 1, offset + endTag);      
  body = null;    
}
```
better to encapsulate in a variable for readability and bug prevention
```
var nextLevel = level + 1;
if(nextLevel < tags.length){
  parts = new Parse(body, tags, nextLevel, offset + endTag);      
  body = null;    
}
```
#### Functions should descend only 1 level of abstraction
The following code mixes two levels of abstraction:
```
public String render() throws Exception 
{      
  StringBuffer html = new StringBuffer("<hr");      
  if(size > 0) {
    html.append(" size=\"").append(size + 1).append("\"");
  }  
  html.append(">");
  return html.toString();    
}
```
This mixes two levels of abstraction:
1. A horizontal rule (hr) has a size
2. The syntax of the rule itself  
  
The refactored code is clearer and easier to change:
```
public String render() throws Exception    
{      
  HtmlTag hr = new HtmlTag("hr");      
  if (extraDashes > 0){
    hr.addAttribute("size", hrSize(extraDashes)); 
  }     
  return hr.html();    
}    

private String hrSize(int height){      
  int hrSize = height + 1;      
  return String.format("%d", hrSize);    
}
```  
Two levels of abstraction, one is creating an HTML element, the lower abstraction calculates the height and returns a numbered string.

#### Keep configurable data at high levels
Default constants or a configuration value that is known and expected at a high level of abstraction should not be buried in a low level function.  
Pass it as an argument (in an object if needed) they are easier to spot and easier to change.

#### Negative Transitive Navigation
Law of Demeter. Chains of getting fields or calling functions means you have bad encapsulation.  
You are also possibly breaking the Single Responsibility Principle, plus the function can have a descriptive name that suits the level of abstraction of the class.  

For example: If a neighbor wants to complain to another neighbor via post, they do not wait for the postman, get the details of the post office, drop the letter at the post office which validates the stamp, wait for the next postman who will deliver mail to the target neighbor, then follows that postman and finally puts the letter through the mailbox. Perhaps the analogy is over-the-top but this demonstrates what the law of Demeter is concerned with, interact with only your immediate neighbors.  

What should happen is just what happens in the real world (if the complaint has to be sent via mail). The complainer addresses and stamps the letter and drops it off at a postbox. This could be represented in code by:  
```
TODO: Add code example   
```