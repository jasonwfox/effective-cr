# Box CRE-U: Effective Code Review

This is a set of programming exercises to demonstrate various code review activities in action. Fork this repository to begin.

## Part I: Salespeople

The Sales floor of Rhombus.biz is organized into a strict hierarchy
of killer salesmanship. At the top of the food chain are the Sociopaths -
they have ABC tatooed onto their chests and get the best leads. As
a result they have the highest chance of closing at 85%.

In the middle are Clueless guys trying to make it to the top. They have a picture
of a new Cadillac taped to the back of their desks and they're working
hard to be more charismatic - although not always successfully. They
have an okay close rate (45%) which gets better if they have a Sociopath above them (65%).

At the bottom are the Losers. They look on in vain as other sales guys
get ahead. Their close rate is terrible (2%) and gets worse if another Loser is
above them (half of what the person above is). A loser with a loser manager will
only succeed 1% of the time and a loser under them will succeed 0.5% of the time.

As a team, pair program a Salesperson class (language of your choice) that has:

* a `name` property, which returns the salesperson's name
* a `success_rate` method, which returns a value between 0 and 1 reflecting the close rate for the sales person, based on the rules above
* a `left` and `right` methods, which can be other Salesperson instances (we are assuming that salespeople can only manage 2 others at a time)
* a way to specify the type of salesperson.

When you're done, get another team to review your pull request for you.

## Part II: The Corporate Ladder

The Director of Sales comes to you ("yo nerd!") to ask for some software that
will do the following:

You are given a string that represents a hierarchical tree for all
the sales guys (it's a legacy format). The string is formatted this way:

* A 0{Name,Type} means add a node with the name "Name" and type "Type" and add a left child edge.
* A 1{Name,Type} means add a node with the name "Name" and type "Type" and pop up to the next node. For example, if you were on a leaf node, you would set the value of the left child and then point at the right node. If you had set the right node, you pop up to the parent and perform on its right node.

Here are some examples of the trees that get made:

`"0{Blake|Sociopath|1{Ricky,Clueless}1{Shelley,Clueless}"`

yields:

	        Blake
		   /    \
	 Ricky      Shelley


`"0{Blake|Sociopath}0{Ricky|Clueless}1{Dave|Loser}0{Shelley|Clueless}1{Williamson|Loser}"`

yields:

			Blake
		   /      \
	    Ricky      x
	    /     \
	   Dave    Shelley
	        /          \
	       Williamson   x

Your job is to interpret the string and create a hierarchy of Salespeople. This hierarchy can
be queried to see who should take on a Lead. Leads have a $ value associated with them. Of course,
the company has some rules about who gets leads:

The salesperson who introduces the lowest amount of $ risk to the company gets the Lead.
Anyone who already has a Lead on their plate can't take on a second.
To optimize their time, Sociopaths only take deals that are $1,000,000 or higher in value.

So given this hierarchy,

	      Blake
		 /    \
	 Ricky     Shelley

and two leads,

	1) Rio Rancho - $300,000
	2) Clear Meadows - $1,100,000

Then Ricky should take the Rio Rancho deal and Blake should take on the Clear Meadows deal. The
total $ risk taken on given their success rates should be

	$300,000 • (1 - 0.65) + $1,100,000 • (1 - 0.85) = $270,000

Which is the sum of the probability of failure assigned to Ricky and Blake multipled by the value of their assigned deals, respectively.

