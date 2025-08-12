# examen123

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
