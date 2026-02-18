* I like to create sections in my PoCs using comments `// == [ Set Up ] ==`, `// == [ Build Payload ] ==`, etc.
    * How many sections really depends on the vulnerability, but in general less is more.
    * the goal of these "sections" is to allow readers to quickly skim the issue and distinguish pre-amble from the actual meat of the PoC 
* For smart contract fork test proof of concepts it's really important to make sure the required interfaces are available
    * the skill is currently missing some instruction w.r.t. how an agent might approach this
* For smart contracts it's sometimes necessary to build an attacker contract. This is the case for example when we want to demonstrate a re-entrancy issue.
    * Prefer realistic proof of concepts using such a contract over simulating aspects of the attack scenario using cheat codes
* I want to change the references layout so the context of agents is better leveraged. The poc-formats document should serve as a hub. Move all the actual content
  into isolated files, and have links going from poc-formats to each file. 
