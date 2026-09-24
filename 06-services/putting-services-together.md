# Putting Services Together
```diagram
                         Browser
                            │
                            │ HTTPS
                            ▼
                         NGINX
                       /       \
                      /         \
             Static file       PHP request
                 │                 │
                 ▼                 ▼
            index.html          PHP-FPM
                                    │
                                    ▼
                              WordPress/PHP
                                    │
                                    │ SQL
                                    ▼
                                 MariaDB
                                    │
                                    ▼
                              Persistent data
```

This is, in my opinion, the best preparation before you build the actual application.

The big advantage is that when something breaks later, you won't just think:

> "Docker networking is broken."

You can ask precise questions:

- Can NGINX reach PHP-FPM?
	    ↓
- Can PHP-FPM execute PHP?
        ↓
- Can WordPress reach MariaDB?
        ↓
- Is MariaDB initialized?
        ↓
- Is the data persisted?
		↓
- That's how you turn Inception from a mysterious Docker project into a series of understandable systems.