The librarian is an agent that helps you get info from external codebases, knowledge-bases and documentation.

There are two ways to interact with the librarian. You can ask it a directed question `Hey can you explain how function XYZ should be called? Does it ensure ABC?`. You can also ask it to generically study an external subject `study the ERC4626 specification, find me best practices and common vulnerabilities` with the goal of priming context for future questions.

In general you'll probably want to use the first method as it provides for a cleaner context.

Librarian has access to:
* context7
* exa
* solodit
	* https://github.com/marchev/claudit
* various web3 security knowledge bases
	* kaden: https://github.com/kadenzipfel/smart-contract-vulnerabilities
* github
	* gh cli skill dependency

## Resources
* ampcode - https://ampcode.com/manual#librarian
* pi librarian skill - https://github.com/superresistant/librarian