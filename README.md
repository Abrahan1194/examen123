ya# examen123

CREATE TABLE users ( 
id SERIAL PRIMARY KEY, 
names VARCHAR(255), 
documents VARCHAR(255), 
adress VARCHAR (255) ,
phone VARCHAR (255) ,
email VARCHAR(255) UNIQUE
)


CREATE TABLE invoices ( 
id SERIAL PRIMARY KEY ,
platform_used VARCHAR(255), 
invoice_numbers VARCHAR(255),
billing_periods VARCHAR(255), 
amount_billeds varchar(255), 
amount_paids VARCHAR(255) 
)


CREATE TABLE transactions ( 
id SERIAL PRIMARY KEY, 
dates VARCHAR(255), 
times_of_transactions VARCHAR(255), 
transactions_statut VARCHAR(255), 
transactions_type VARCHAR(255)
)


async function loadusersCSV() {
    try {
        await client.connect();

        const users = [];
        fs.createReadStream('./back-end/data/user.csv')
        .pipe(csv())
        .on('data', (data) => {
            users.push(data);
        })
        .on('end', async () => {
            for (const user of users) {
                const query = `
                    INSERT INTO users (names,documents,adress,email)
                    VALUES ($1, $2, $3, $4)
                `;
                const values = [
                    user.name,
                    user.documents,
                    user.adress,
                    user.email,
                ];
                await client.query(query, values);
            }
            console.log('success users');
            await client.end();
        });

    } catch (err) {
        console.error('Error loading users:', err);
        await client.end();
    }
}



const fs = require('fs');
const { Client } = require('pg');
const csv = require('csv-parser');

async function loadInvoicesCSV() {
  const client = new Client({
    user: 'tu_usuario',
    host: 'tu_host',
    database: 'tu_base_de_datos',
    password: 'tu_contraseña',
    port: 5432,
  });

  try {
    await client.connect();

    const invoices = [];
    fs.createReadStream('./back-end/data/invoices.csv')
      .pipe(csv())
      .on('data', (data) => {
        invoices.push(data);
      })
      .on('end', async () => {
        for (const invoice of invoices) {
          const query = `
            INSERT INTO invoices (platform_used, invoice_numbers, billing_periods, amount_billeds, amount_paids)
            VALUES ($1, $2, $3, $4, $5)
          `;
          const values = [
            invoice.platform_used,
            invoice.invoice_numbers,
            invoice.billing_periods,
            invoice.amount_billeds,
            invoice.amount_paids,
          ];
          await client.query(query, values);
        }
        console.log('Invoices subidas exitosamente.');
        await client.end();
      });
  } catch (err) {
    console.error('Error subiendo invoices:', err);
    await client.end();
  }
}

// Llama a la función para ejecutarla
loadInvoicesCSV();

const fs = require('fs');
const { Client } = require('pg');
const csv = require('csv-parser');

async function loadTransactionsCSV() {
  const client = new Client({
    user: 'tu_usuario',
    host: 'tu_host',
    database: 'tu_base_de_datos',
    password: 'tu_contraseña',
    port: 5432,
  });

  try {
    await client.connect();

    const transactions = [];
    fs.createReadStream('./back-end/data/transactions.csv')
      .pipe(csv())
      .on('data', (data) => {
        transactions.push(data);
      })
      .on('end', async () => {
        for (const transaction of transactions) {
          const query = `
            INSERT INTO transactions (dates, times_of_transactions, transactions_statut, transactions_type)
            VALUES ($1, $2, $3, $4)
          `;
          const values = [
            transaction.dates,
            transaction.times_of_transactions,
            transaction.transactions_statut,
            transaction.transactions_type,
          ];
          await client.query(query, values);
        }
        console.log('Transacciones subidas exitosamente.');
        await client.end();
      });
  } catch (err) {
    console.error('Error subiendo transacciones:', err);
    await client.end();
  }
}

// Llama a la función para ejecutarla
loadTransactionsCSV();
