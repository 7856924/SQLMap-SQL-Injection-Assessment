# Secure Code Remediation Standards

To prevent SQL injection across your infrastructure, developers must remediate insecure dynamic queries using secure, decoupled input models.

---

## ❌ Insecure Code Implementation (Vulnerable Pattern)
The code pattern below takes raw input directly from the user and concatenates it straight into the SQL command context. This allows attackers to break out of string boundaries and execute unauthorized database statements.

```javascript
const express = require('express');
const { Client } = require('pg');
const app = express();

app.post('/Login.asp', async (req, res) => {
    const username = req.body.tfUName;
    const password = req.body.tfUPass;

    // VULNERABLE: Direct string interpolation allows backend command injection
    const query = `SELECT * FROM users WHERE uname = '${username}' AND upass = '${password}'`;
    
    try {
        const result = await client.query(query);
        res.status(200).send('Authentication Evaluated');
    } catch (err) {
        res.status(500).send('Internal Server Error');
    }
});
```

---

## ✅ Secure Code Implementation (Remediated Pattern)
This safe implementation replaces dynamic string interpolation with positional arguments (`$1`, `$2`). The query string structure is pre-compiled by the database engine, and user parameters are sent separately as clean string data parameters.

```javascript
app.post('/Login.asp', async (req, res) => {
    const username = req.body.tfUName;
    const password = req.body.tfUPass;

    // SECURE: Parameters are cleanly decoupled from the query instruction logic
    const secureQuery = {
        text: 'SELECT id, uname, email FROM users WHERE uname = \$1 AND upass = \$2',
        values: [username, password], 
    };

    try {
        const result = await client.query(secureQuery);
        if (result.rows.length > 0) {
            res.redirect('/dashboard');
        } else {
            res.status(401).send('Invalid credentials provided.');
        }
    } catch (err) {
        console.error(err.stack);
        res.status(500).send('A system error occurred during authentication.');
    }
});
```
