import React, { useState } from "react";
import "./App.css";

function App() {
  const [transactions, setTransactions] = useState([]);
  const [description, setDescription] = useState("");
  const [amount, setAmount] = useState("");
  const [type, setType] = useState("Income");

  const addTransaction = (e) => {
    e.preventDefault();

    if (!description || !amount || amount <= 0) {
      alert("Enter valid details");
      return;
    }

    const newTransaction = {
      id: Date.now(),
      description,
      amount: parseFloat(amount),
      type,
    };

    setTransactions([...transactions, newTransaction]);
    setDescription("");
    setAmount("");
    setType("Income");
  };

  const deleteTransaction = (id) => {
    setTransactions(transactions.filter((t) => t.id !== id));
  };

  const income = transactions
    .filter((t) => t.type === "Income")
    .reduce((acc, t) => acc + t.amount, 0);

  const expenses = transactions
    .filter((t) => t.type === "Expense")
    .reduce((acc, t) => acc + t.amount, 0);

  const balance = income - expenses;

  return (
    <div className="app">
      <div className="card">
        <header className="header">
          <h1>💸 Expense Tracker</h1>
          <p>Track your money smartly</p>
        </header>

        <section className="summary">
          <div className="balance">
            <h2>₹ {balance.toFixed(2)}</h2>
            <span>Current Balance</span>
          </div>

          <div className="stats">
            <div className="income-box">
              <h3>Income</h3>
              <p>₹ {income.toFixed(2)}</p>
            </div>
            <div className="expense-box">
              <h3>Expenses</h3>
              <p>₹ {expenses.toFixed(2)}</p>
            </div>
          </div>
        </section>

        <section className="form-section">
          <h3>Add Transaction</h3>
          <form onSubmit={addTransaction}>
            <input
              type="text"
              placeholder="Description"
              value={description}
              onChange={(e) => setDescription(e.target.value)}
            />

            <input
              type="number"
              placeholder="Amount"
              min="0"
              value={amount}
              onChange={(e) => setAmount(e.target.value)}
            />

            <select value={type} onChange={(e) => setType(e.target.value)}>
              <option value="Income">Income</option>
              <option value="Expense">Expense</option>
            </select>

            <button type="submit">Add Transaction</button>
          </form>
        </section>

        <section className="history">
          <h3>Transaction History</h3>
          <ul>
            {transactions.map((t) => (
              <li
                key={t.id}
                className={t.type === "Income" ? "income-item" : "expense-item"}
              >
                <div>
                  <strong>{t.description}</strong>
                  <span>₹ {t.amount.toFixed(2)}</span>
                </div>
                <button onClick={() => deleteTransaction(t.id)}>✕</button>
              </li>
            ))}
          </ul>
        </section>
      </div>
    </div>
  );
}

export default App;
