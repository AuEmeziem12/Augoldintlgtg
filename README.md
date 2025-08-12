# Augoldintlgtg
Web app hosted on sql data base 
const express = require('express');
const sql = require('mssql');
const app = express();
const port = process.env.PORT || 3000;

const config = {
  user: process.env.DB_USER,
  password: process.env.DB_PASS,
  server: process.env.DB_SERVER,
  database: process.env.DB_NAME,
  options: { encrypt: true, trustServerCertificate: false }
};

app.get('/', async (req, res) => {
  try {
    await sql.connect(config);
    const result = await sql.query`SELECT name FROM sys.databases`;
    res.send(result.recordset);
  } catch (err) {
    res.status(500).send(err.message);
  }
});

app.listen(port, () => console.log(`Server running on port ${port}`));