1. Install sqlite3
2. Download sample database to follow along with tutorial
---

## Remarks
- By **default**, a sqlite session started with `sqlite3` cli tool will use an **in memory database**, which will be ~~gone upon session end~~.
- To open db from file instead,
	- use the `.open /path/to/database.db` command at `sqlite>` prompt
	- provide `.db` file as arg to cli tool when connecting 👇![[Pasted image 20230724152753.png]]
	- If the specified file does not exist, _new database will be created_
- 