- 👋 Hi, I’m @keerthi9963
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
keerthi9963/keerthi9963 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
[8:13 am, 28/07/2024] Sai Keerthi: const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');
const sqlite3 = require('sqlite3').verbose();

const app = express();
const port = 3001;

app.use(cors());
app.use(bodyParser.json());

// Initialize SQLite database
let db = new sqlite3.Database('./transactions.db', (err) => {
    if (err) {
        console.error(err.message);
    }
    console.log('Connected to the SQLite database.');
});

// Create Transactions table
db.run(`CREATE TABLE IF NOT EXISTS transactions (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    type TEXT,
    amount REAL,
    description TEXT,
    date TEXT,
    running_balance REAL
)`);

// API Endpoints
app.get('/api/transactions', (req, res) => {
    db.all(SELECT * FROM transactions, [], (err, rows) => {
        if (err) {
            res.status(400).json({ error: err.message });
            return;
        }
        res.json({ transactions: rows });
    });
});

app.post('/api/transactions', (req, res) => {
    const { type, amount, description } = req.body;
    const date = new Date().toISOString();
    // Add transaction logic here including calculating running balance
    // For simplicity, assume running_balance is calculated correctly.
    let running_balance = calculateRunningBalance(type, amount);
    db.run(INSERT INTO transactions (type, amount, description, date, running_balance) VALUES (?, ?, ?, ?, ?),
        [type, amount, description, date, running_balance],
        function(err) {
            if (err) {
                res.status(400).json({ error: err.message });
                return;
            }
            res.json({ id: this.lastID });
        }
    );
});

// Helper function to calculate running balance
function calculateRunningBalance(type, amount) {
    // Retrieve the last balance from the database
    // Calculate the new balance based on type and amount
    // This is just a placeholder logic
    return type === 'Credit' ? amount : -amount;
}

app.listen(port, () => {
    console.log(Server is running on port ${port});
});
 npx create-react-app office-transactions-frontend
cd office-transactions-frontend
 Bash
 // src/components/TransactionsList.js
import React, { useEffect, useState } from 'react';
import axios from 'axios';

const TransactionsList = () => {
    const [transactions, setTransactions] = useState([]);

    useEffect(() => {
        axios.get('http://localhost:3001/api/transactions')
            .then(response => setTransactions(response.data.transactions))
            .catch(error => console.error(error));
    }, []);

    return (
      [8:14 am, 28/07/2024] Sai Keerthi: <div>
            <h2>Office Transactions</h2>
            <table>
                <thead>
                    <tr>
                        <th>Date</th>
                        <th>Description</th>
                        <th>Credit</th>
                        <th>Debit</th>
                        <th>Balance</th>
                    </tr>
                </thead>
                <tbody>
                    {transactions.map(tx => (
                        <tr key={tx.id}>
                            <td>{tx.date}</td>
                            <td>{tx.description}</td>
                            <td>{tx.type === 'Credit' ? tx.amount : '-'}</td>
                            <td>{tx.type === 'Debit' ? tx.amount : '-'}</td>
                            <td>{tx.running_balance}</td>
                        </tr>
                    ))}
                </tbody>
            </table>
        </div>
    );
}

export default TransactionsList;
[8:15 am, 28/07/2024] Sai Keerthi: Jsx
[8:15 am, 28/07/2024] Sai Keerthi: Jsx
[8:15 am, 28/07/2024] Sai Keerthi: // src/components/AddTransaction.js
import React, { useState } from 'react';
import axios from 'axios';

const AddTransaction = () => {
    const [type, setType] = useState('Credit');
    const [amount, setAmount] = useState('');
    const [description, setDescription] = useState('');

    const handleSubmit = (event) => {
        event.preventDefault();
        axios.post('http://localhost:3001/api/transactions', {
            type,
            amount,
            description
        })
        .then(response => {
            console.log(response.data);
            // Optionally refresh the list or reset form
        })
        .catch(error => console.error(error));
    };

    return (
        <form onSubmit={handleSubmit}>
            <label>
                Transaction Type:
                <select value={type} onChange={e => setType(e.target.value)}>
                    <option value="Credit">Credit</option>
                    <option value="Debit">Debit</option>
                </select>
            </label>
            <label>
                Amount:
                <input
                    type="number"
                    value={amount}
                    onChange={e => setAmount(e.target.value)}
                    required
                />
            </label>
            <label>
                Description:
                <input
                    type="text"
                    value={description}
                    onChange={e => setDescription(e.target.value)}
                    required
                />
            </label>
            <button type="submit">Save</button>
        </form>
    );
};

export default AddTransaction;
 Component
 // src/App.js
import React from 'react';
import TransactionsList from './components/TransactionsList';
import AddTransaction from './components/AddTransaction';

const App = () => {
    return (
        <div>
            <TransactionsList />
            <AddTransaction />
        </div>
    );
}

export default App;
// src/App.js
import React from 'react';
import TransactionsList from './components/TransactionsList';
import AddTransaction from './components/AddTransaction';

const App = () => {
    return (
        <div>
            <TransactionsList />
            <AddTransaction />
        </div>
    );
}

export default App;
