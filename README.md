# Notes from Clean Code Course

This is a summary of the main points from the Clean Code Course by Robert C. Martin for quick reference while working.

## Naming

- **Reveal Intention.** The name of a variable function, or class should tell you three things: why it exists, what it does, and how it is used. If a name requires a comment, then the name does not reveal its intent.
- **No Magic Numbers.** Use constants or enums instead.
- **Don't be Cute.** Choose Clarity over Humor. Say what you mean. Mean what you say.
- **Searchable Names.** Names should be easy to locate across a body of text. If a name occurs in multiple places, it is imperative to give it a search-friendly name. Single-letter names should ONLY be used as local variable inside short methods.
- **One Word, One Meaning.** Be consistent thourghout your code by using one word per abstract concept. For example, get, fetch, retrive.
- **Meaningful Distinctions.** When attempting to differentiate code, make distinctions that are meaningful without changing the searchability or intention of the code. For example, don't change the spelling because the name is already taken - Class, Klass, Clazz.
- **User Pronounceable Names.** For example: genymdhms versus generationTimeStamp.
- **Class and Method Names:**
  - Use nouns or noun phrases for classes and objects.
  - Use verbs or verb phrases for functions and methods.
- **Variables and Parameters Name:**
    - Long Scope = Long Name.
    - Short Scope = Short Name.
    - Single-letter names should be used only for:
      - Counter varibales for simple loops.
      - Exception instances in Catch blocks.
      - Arguments of very short functions.
- **Class and Method Length:**
    - Long Scope = Short Name. (public methods)
    - Short Method Scope = Long Name. (private methods)

## Functions

- **Functions Should be As Small As Possible**. **1** to **10** lines, should not cross **20 at worst case**.
- **Function Should do Only One Thing**. If a function does more than one thing, extract the code blocks into separate functions.
- **Indent Level Shouldn’t Exceed One or Two**. Use early returns or guard clauses to avoid nested if statements.
- **Functions Should Use Exceptions, Not Error Codes**. Error codes force the caller to check the return value and handle the error, which makes the code more complex and error-prone. Exceptions allow the caller to ignore the error if it is not relevant, or catch it and handle it appropriately.
- **Extract Try/Catch Blocks into Functions of Their Own. And that function should end with the try/catch/finally block**. This way, the normal flow of the code is not interrupted by the error handling logic, and the function name can indicate the purpose of the try/catch block.
- **Functions Should Have One Level of Abstraction**. A function should not mix high-level and low-level details. If a function contains a lower-level detail, extract it into a separate function and call it from the original function. This way, the function name can describe the high-level action, and the lower-level details are hidden in the called functions.
- **Functions Should Be a Command or a Query—Not Both**. Command-Query Separation is a principle that states functions should either “do” something, a command, or “answer” something, a query. If your function is changing the state of an object and returning information about that object—that can be confusing. The solution is to separate the command from the query, by creating two functions from the original function.
- **Functions Should Not Have Side Effects**. A side effect is any change in the state of the system that is not part of the function’s output or return value. Side effects make the code harder to understand, test, and debug. They also introduce dependencies and couplings between functions. A function should only depend on its input parameters and should only affect its output or return value.

## Parameters

- **Number of parameters should not exceed 2, worst case 3**. If a function needs more than 3 parameters, consider using an object or a data structure to group them together, or refactor the function into smaller functions.
- **Do Not Use boolean flag as parameter, split the function into two instead**. A boolean flag indicates that the function is doing more than one thing, depending on the value of the flag. This violates the single responsibility principle and makes the function harder to read and maintain. It is better to create two functions, one for each branch of the conditional, and name them accordingly.
- **Do Not Pass null as parameter**. Passing null as a parameter is a bad practice that can lead to unexpected errors and null pointer exceptions. Instead of passing null, use default values, optional parameters, or overloaded functions. Alternatively, use the null object pattern or the special case pattern to handle the null case explicitly and gracefully.
- **Do Not return null as value**. Returning null from a function is also a bad practice that can cause errors and confusion for the caller. Throw an exception or return an empty collection to indicate the absence of a value.
- **Avoid output parameter**. An output parameter is a parameter that is passed by reference and modified by the function. Output parameters are confusing and misleading, because they violate the principle of least surprise. They make the function look like a query, but in fact it is a command that changes the state of the parameter. Output parameters also introduce unnecessary dependencies and couplings between the function and the caller. If a function needs to return more than one value, use an object or a data structure to encapsulate them, or refactor the function into smaller functions.

## Side Effects

- Side Effects are often caused if a function does not do only one thing. A function that does more than one thing is likely to have side effects, because it is changing the state of the system in unexpected ways. To avoid side effects, follow the single responsibility principle and make sure that each function does only one thing.
- The Issue of Temporal Coupling: The root of the problem is that side effects usually occur in paired functions. Here are some simple, common examples:

  - Begin and commit
  - Open and close
  - Start process and Join/Finish
  - New and Delete

  The relationship between these pairs is called “temporal coupling.” Temporal coupling is problematic because it forces one function to be called before another—“begin” must be called before “commit,” “open” must be called before “close,” etc. If the functions aren’t called in the correct order, your code won’t work.

- **Solution : Passing the block - try to pass implementation as a parameter(Functional Interfaces)**. If you’re going to use a side effect, you need to make it as smooth and organized as possible. “Passing the block” is a useful technique to eliminate the issues caused by temporal coupling. Consider the example below: Instead of using the function “open” you could use this small function “processFile.”

```java
public void processFile(File p, Action a) {
	file.open(p);
	a.perform(f);
	f.close();
}
```

The function encapsulates the temporal coupling operations and keeps them close together. Now, all operations with files can be done through the function “processFile.”

```java
processFile(myFile, (f) -> {...});
```

## Structure

- Switch statements can often be replaced with polymorphisms or with a simple map data structure. You can determine which one to use based on the complexity of the switch statement. A complex switch statement will be replaced by a polymorphic interface and each case will be in a separate interface implementation. A simple switch statement—where each case returns some value—can be replaced with a map data structure.
- Internally called method should be placed in the sequence they are called. Example : Here methodB is called from methodA thus we should place methodB right after methodA. Similarly for methodC, it should be placed after methodB.

```java
methodA() {
	if() {
		methodB();
	}
}
methodB() {
	return methodC();
}
methodC() {
	return value;
}
```

- **Keep number of lines in a file small**. A large file is hard to read, understand, and maintain. It also increases the risk of conflicts and merge errors when working with version control systems. A good rule of thumb is to keep the file size under 500 lines, and preferably under 200 lines. If a file is too large, consider splitting it into smaller files based on logical modules or components.
- **Horizontal length should not exceed the length of your screen**. A long line of code is hard to read and comprehend. It also makes the code less readable and consistent when it is wrapped or scrolled. A good rule of thumb is to keep the line length under 80 characters, and preferably under 60 characters. If a line is too long, consider breaking it into multiple lines using appropriate indentation and alignment.

## Comments

- **The only good comment is the comment you found a way not to write!** Comments are often used as a crutch to compensate for bad code. Comments can be misleading, outdated, redundant, or unnecessary. Comments can also clutter and distract from the code. The best code is self-explanatory and does not need comments. Instead of writing comments, write clean code that expresses your intent and logic clearly and concisely.
- The Good:

  - **Legal Comments**: These are comments that contain legal information or license terms, such as the author, date, copyright, etc. These comments are usually required or recommended by the organization or the project.
  - **Informative Comments**: These are comments that provide useful information or context that is not obvious from the code, such as the purpose, usage, or behavior of a variable, function, class, etc. These comments should be accurate and up-to-date, and should not repeat what the code already says.
  - **Explanation of Intent**: These are comments that explain why the code is written in a certain way, or what problem it is trying to solve, or what assumption it is making, etc. These comments can help the reader understand the rationale and the trade-offs behind the code, and can prevent future changes that might break the code or introduce bugs.
  - **TODO Comments**: These are comments that indicate a task or a feature that needs to be implemented or improved in the future. These comments should be marked with a keyword like TODO or FIXME, and should include a brief description of the task and the person responsible for it. These comments should be tracked and resolved as soon as possible.
  - **Improving the Code**: These are comments that suggest a better way of writing the code, or a possible optimization, or a potential bug, etc. These comments should be marked with a keyword like NOTE or HACK, and should include a clear explanation of the issue and the proposed solution. These comments should be reviewed and verified before applying the changes.

- The Bad:

  - **Irrelevant Comments**: These are comments that have nothing to do with the code, or that are out of date, or that are incorrect, etc. These comments are misleading and confusing, and should be removed or updated as soon as possible.
  - **Bad Code**: These are comments that try to explain or justify bad code, such as code that is poorly written, poorly structured, poorly formatted, poorly named, etc. These comments are a sign of laziness and incompetence, and should be replaced by clean code that does not need comments.
  - **Redundant Comments**: These are comments that repeat what the code already says, or that state the obvious, or that add no value, etc. These comments are unnecessary and distracting, and should be removed or simplified as much as possible.
  - **Commented Out Code**: These are comments that contain code that is not used or needed, or that is obsolete, or that is experimental, etc. These comments are a source of confusion and clutter, and should be deleted or moved to a separate file or branch as soon as possible.
  - **Journaling Comments**: These are comments that record the history or the changes of the code, such as the date, the author, the version, the reason, etc. These comments are redundant and outdated, and should be replaced by a version control system that can track and manage the code history more efficiently and accurately.
  - **Noise Comments**: These are comments that add no information or value, but only noise and clutter, such as empty or filler comments, or decorative or fancy comments, or excessive or verbose comments, etc. These comments are annoying and unprofessional, and should be avoided or minimized as much as possible.
  - **Closing Brace Comments**: These are comments that indicate the end of a block or a scope, such as a function, a class, a loop, an if statement, etc. These comments are usually unnecessary and redundant, and should be replaced by proper indentation and formatting that can make the code structure more clear and readable.
