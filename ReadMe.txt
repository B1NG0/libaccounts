LibAccounts is a handy tool to let you tell which accounts you're on,
using account-local SavedVariables to store magic cookies.

Call Library.LibAccounts.table_init() (at a time when player and shard
are available), then you can poke at the exposed variables:

  Library.LibAccounts.here = {
    chars = { char = acctid, ... },
    accounts = { acctid = { char = faction, ... }, ... }
  }

NOTE:  Please don't write to this.  Also, please don't look at other
fields; there may be some, but the ones that aren't documented here
aren't considered part of the stable API.

The other useful function, of interest mostly to altoholics:

	Library.LibAccounts.available_chars()

This produces a table:
	{ char = char, faction = faction, acctid = acctid }, ...
	char = { faction, acctid }, ...

of characters who are "available".  That's every character on this account,
plus any characters on other accounts who are of the same faction as at
least one character on this account, and so on recursively.  There's numeric
keys for a complete list, or you can just index by a character.

So if you have account #1:
	Defiant1
	Defiant2
	Defiant3
and account #2:
	Defiant4
	Guardian1
and account #3:
	Guardian1
	Guardian2
	Guardian3

All the characters are considered "available" to each other because you
could transfer items from Guardian3 to Guardian1 (via mail) to Defiant4
(via same-account mail, new in 1.6.1) to Defiant1.

You can use /accounts (it ignores any options you give it for now, but
again, please don't rely on that) for a dump of this shard's info.

LibAccounts doesn't currently expose cross-shard stuff.

Intentionally exposed data:
	Library.LibAccounts.here = {
	  chars = { char = acctid, ... },
	  accounts = { acctid = { char = faction, ... }, ... }
	}
	Library.LibAccounts.acctid = number

Intentionally exposed API:
	Library.LibAccounts.available_chars(acct_only)
		Yields { char, faction, acctid } tuples for this shard
		which are on the current account or reachable from characters
		on this account.  If acct_only is truthy, checks only
		characters on this account.  Also char = { faction, acctid }
		tuples.
	Library.LibAccounts.acct_of(char)
		Returns the acctid, if any, that has the named
		character (or player if char is nil)
	Library.LibAccounts.available_p(charname)
		Indicates whether "charname" is believed reachable.
