const express = require('express');
const stripe = require('stripe')('YOUR_SECRET_KEY'); // Aizvieto ar savu secret key

const app = express();
app.use(express.json());

app.post('/create-checkout-session', async (req, res) => {
    const { minAmount, maxAmount } = req.body;

    const session = await stripe.checkout.sessions.create({
        payment_method_types: ['card'],
        line_items: [{
            price_data: {
                currency: 'eur',
                product_data: {
                    name: `Ziedojums no ${minAmount} līdz ${maxAmount} EUR`,
                },
                unit_amount: minAmount * 100, // Pārvērst EUR uz centiem
            },
            quantity: 1,
        }],
        mode: 'payment',
        success_url: `https://paula-web-stack.github.io/Sapnis/success.html`,
        cancel_url: `https://paula-web-stack.github.io/Sapnis/cancel.html`,
    });

    res.json({ sessionId: session.id });
});

app.listen(4242, () => console.log('Node.js server is running on port 4242'));
