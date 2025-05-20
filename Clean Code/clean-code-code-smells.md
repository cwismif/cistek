# Clean Code Summararise
## Code Smells
### General
#### Duplication
Duplication is difficult to maintain, leads to bugs and bloat.  If identified its an opportunity to refactor, use the strategy pattern or use polymorphism instead of if/else stacks.
#### Wrong Abstraction
Base classes/modules/packages should know nothing of their parents.  Do not mix high and low abstraction.
#### Too much Information
The smaller and simpler an API the better, this applied to classes and any public variables or functions.  This applies to packages, modules, libraries, web apps.  Hide as much as possible.  Exposing too much confuses the user and complicates the use of the API.
#### Vertical Separation
In the class file, keep related functions vertically close together.  Variables should be declared in as small a scope as possible and only just before they are used.  Private functions shuold be declared just before their first use.  
#### Consistency
Be consistent with naming.  Use the same name for the same concepts.  Similar variables and functions should be named similarly. 
#### Clutter
Delete unused function, variables, commented out code, unused imports, unused configuration, old documentation, etc..  Keep the codebase as lean as possible.
#### Artifical Coupling
General utils for example should be in a general utils class.  Not defined in one class and just made public to be shared with a completely different class. 
#### Feature Envy
"The methods of a class should be interested in the variables and functions of the class they belong to, not the variables and functions of other classes."  When a method of a class uses the accessors and mutators of that other class to manipulate the data in that other class, its overreach.  We want to internals of one class to be encapsulated.  This is a balancing act though.  Coupling between 2 classes is sometimes avoided by one class uses the exposed internal state of another class.
#### Selector Arguments
The bad practice of using arguments to allow a larger function to behave differently according to those arguments instead of having multiple, smaller functions.
#### Obscured Intent
Code should be readable and expressive - not codified and opaque.  Use variable, function, class and module names with that in mind.  Conciseness is not the biggest priority.
#### Misplaced Responsibility
When deciding where to put code, the principle of least surprise often serves well.  Where would most people expect to find it? Or at least where would the fewest people be surprised to find it!
#### Inappropriate static
Some static functions make sense, eg `Math.max(a,b)` but others do not.  For example `HourlyPayCalculator.calculatePay(employee, overTimeRate)` might be better as a non-static method of the `Employee.class`  When in doubt, make the function non-static, if you make it static, make sure you would never want it to behave polymorphically.
#### Use explanatory variables
Break up calculations into intermediate values assigned to variables that hold meaningful names.
#### Function Names should say what they do
For example, `Date newDate = date.add(5)` is hard to understand.  Does the 5 refer to days or months?  Does it even return a new Date or is it updated in place.  `date.addDays(5)` or `Date newDate = date.daysLater(5)` would be more readable.
#### Understand the algorithm
Don't be complacent.  Attempt to really understand an algorithm.  Tests can help but another technique is breaking down the algorithm into simple blocks.
#### Make Logical Dependencies Physical
Other classes should not assume the values held in other classes, they should share common values through mutually accessable means.
#### Prefer polymorphism to if/else switch/case
Easier to extend in terms of adding more cases but also adding more functionality in each case.
#### Follow standard conventions
The team should share a formatting document and a set of other standards (naming, class structure)  
Project templates and shared libraries are another good way to enforce this consistency.

#### Replace magic numbers with named constants
Name these numbers using descriptive variable names.  Even values that are calculated and used in a single place eg instead of `int s = 24*60*60` use `int secondsInADay = 24*60*60` or even `int secondsInADay = hoursInADay * minutesInAnHour * secondsInAMinute` (but this might probably be overkill)
Even non number values can be assigned to descriptive variables.  For example a supermarket company might have well known test store with the storeName of `london-nw1-234` so assigning it to `TEST_STORE_NAME` might be a good idea. 
#### Be precise
With floats, with rounding, with locking, closing resources, passing and returning null.
#### Structure over Convention
Enforce design decisions with structure over conventions.  Structure enforces, convention does not.
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

Allows for disaster as `saturateGradient()` could be mistakenly be called after `saturateGradient()`

Alternatively, the snippet below would enfore ordering.

```
public class MoogDiver {      
  Gradient gradient;      
  List<Spline> splines;      
  public void dive(String reason) {        
    gradient = saturateGradient();        
    splines = reticulateSplines();        
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
better to encapsulate in a variable for readabililty and bug prevention
```
var nextLevel += 1;
if(nextLevel < tags.length){
  parts = new Parse(body, tags, nextLevel, offset + endTag);      
  body = null;    
}
```
#### Functions should descend only 1 level of abstraction
The snippet above uses the size (horizonatal width of the html page) to draw a line.  
This mixes 2 levels of abstraction.  
1. a horizontal rule (hr) has a size and   
2. the syntax of the rule itself.
The refactored code is clearer and easier to change.
```
public String render() throws Exception 
{      
  StringBuffer html = new StringBuffer(“<hr”);      
  if(size > 0) {
    html.append(” size=\“”).append(size + 1).append(”\“”);
  }  
  html.append(“>”);
  return html.toString();    
}
```
The snippet above uses the size (horizonaltal width of the html page) to draw a line.  This mixes 2 levels of abstraction.  1. a horizontal rule (hr) has a size and 2. the syntax of the rule itself.
The refactored code is clearer and easier to change.
```
public String render() throws Exception    
{      
  HtmlTag hr = new HtmlTag(“hr”);      
  if (extraDashes > 0){
    hr.addAttribute(“size”, hrSize(extraDashes)); 
  }     
  return hr.html();    
}    

private String hrSize(int height){      
  int hrSize = height + 1;      
  return String.format(“%d”, hrSize);    
}
```  
Two levels of abstration, one is creating a HTML telement, the lower abstraction calculates the height and returns a string

#### Keep configurable data at high levels
Default constants or a configuration value that is known and and expected at a high level of abstraction should not be buried in a low level function.  
Pass it as an argument (in an object if needed) they are easier to spot and easier to change.

#### Negative Transitive Navigation
Law of Demeter.  Chains of getting fields or calling functions means you have bad encapsulation.  
You are also possibly breaking the Single Responsibility Principle, plus the function can have a descriptive name that suits the level of abstraction of the class.  
For example: If a neighbour wants to complain to another neightbour via post, they do not wait for the postman, get the details of the post office, drop the letter at the post office which validates the stamp, wait for the next postman who will deliver mail to the target neightbour, then follows that postman and finally puts the letter through the mailbox.  Perhaps the analogy is OTT but this is breaking the law of Demeter which is to only interact with neightbours.  
What should happen is just what happens in the real work (if the complaint has to be sent via mail).  The complainer addresses and stamps the letter and drops it off at a postbox.  This could be represented in code by:  
```
// contains all the postboxes
Neightbour complaintSender = new Neighbour(name, address);
PostBox nearestPostBox = new PostBoxRegistry().getPostBoxClosestTo(complaintSender.formatAddress());
ComplaintReceiver complaintReceiver = new Neighbour(name, address);
Letter compaintLetter = complaintSender.letterComplaint("Hey, \n" + complaintReceiver.getName() + "turn down the music RIGHT NOW", complainReceiver.formatAddress());
nearestPostBox.post(complaintLetter);
PostOffice postOffice = new PostOfficeRegistry(PostOffices.US).getPostOfficeFor(nearestPostBox);
PostBoxCollection postBoxCollection = postOffice.blockUntilNextPostBoxCollectionCompletes();
DistributionRound distributionRound = postOffice.validate(postBoxCollection.validate()).blockUntilDistributionBegins(postOffice.blockUntilNextDistrubutionRound());
distributionRound.formattedAddresses().stream().filter(addr -> complainReceiver.matchesFormattedAddress(addr)).


Source: https://github.com/jeffrey-xiao/clean-code-javascript/blob/master/README.md