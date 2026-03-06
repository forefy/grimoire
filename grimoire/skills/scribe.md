The [[agents/scribe|scribe]] agent's task is to learn from your findings and conversations.

It builds detection modules and documentation for autonomous detection of vulnerabilities. To do this grimoire has a couple of scribe related skills. 

Currently scribe focuses on static analysis, but will extend this to pure agentic detectors in the future.

Static Analysis Skills:
* semgrep
* slither
* codeql

These skills teach your agent how to use static analysis tools. 

Scribe Skills:
* distill
	* takes a finding / conversation and determines if there are opportunities for automation
* garbage collection
	* over time it's likely that duplication will occur, we need a skill to reduce duplication
	* if there is a static analysis issue triaging skill then it might observe duplicates between static analysis modules, this is probably a good trigger for garbage collection
* indexing
	* just some simple utilities to index static analysis tools

ideas:

I built napalm a while ago:
https://github.com/ConsenSysDiligence/napalm

 It is a tool for orchestrating use of many different detection modules.
## Static Analysis

Sta

### Evaluation

* consolidate
* de-duplicate
* garb


## Additional Notes

* sometimes you build static analysis modules that are quite specific to a given project. Semgrep rules that regex match on function variables are a good example.
* 