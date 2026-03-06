A "new" concept that guides what kind of questions you should ask to agents.

In science a common concept is that of *falsifiability*. It is the property of a statement/ hypothesis/ question to be tested and falsified. It is at the core of the philosophical question "what is science?". 

I'm introducing a new concept *trivial falsifiability*. It applies to statements, hypotheses and questions where the proof takes the form of a counter example.

## why?

Agents are smart but unreliable.

You'll encounter hallucinations, cheating and reasoning mistakes when you start to use agents. As a result we need to find ways of working with agents that avoid such mistakes. A big topic in regards to this is [[backpressure]]. 

The core idea of trivial falsifiability is to only ask questions that can easily be checked. 

For example:
```
good

Statement: There are no wolves in this town.
Counterexample: Here is a picture taken today of a wolf in this town.


bad

Statement: all animals in this town are dogs
Proof: pictures of a bunch of animals
```

A more code oriented example:
```
good

Statement: I found a formfield that might be vulnerable to XSS there is some escaping can you find me the code that performs the escaping and explain what measures it implements?
Counterexample: link to the code + explanation

bad

Statement: Can you find all locations where user input is displayed on the frontend without proper xss filtering
Proof: some code locations
```

The goal of using agents is to provide leverage, if you need to go in and re-do a lot of the same work the agent did only to make sure it did indeed get all the access points. Then you're not doing it right. To really gain leverage you need to ask questions that will get easy to verify answers. 

## Not a Rule

Non trivially falsifiable questions are dangerous but can also be powerful.

Your primary instrument in co-auditing should be *trivially falsifiable* questions. However, open ended `find me all X` questions can be used as a form of fuzzing. Identifying interesting locations for you to go and have a look at. Providing exploitation [[hypothesis]] for you to verify and potentially include in a report.

Be ware!

The auditing process is largely cognitive, and our brains like shortcuts. Treat the output to such questions as radioactive. Trust the agent to provide you with interesting data. Never trust it to provide you the correct answer to a question. 

## Autonomous Vulnerability Discovery

Note that this concept is not necessarily limited to co-auditing approaches.

An [[autonomous vulnerability discovery]] agent can also leverage these insights. In autonomous discovery processes it's worthwhile reaching for methods that apply [[backpressure]] (such as a semgrep / codeql rule) whenever *non trivially falsifiable* hypothesis are posed.

