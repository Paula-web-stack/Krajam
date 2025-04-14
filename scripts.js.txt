// Ielādē Stripe
const stripe = Stripe('YOUR_PUBLISHABLE_KEY'); // Aizvieto ar savu Stripe public key

// Sagatavo maksājumu funkcijas
const donate1to10 = document.getElementById('donate-1-10');
const donate10to15 = document.getElementById('donate-10-15');
const donate15to20 = document.getElementById('donate-15-20');

donate1to10.addEventListener('click', () => {
    handlePayment(1, 10);
});

donate10to15.addEventListener('click', () => {
    handlePayment(10, 15);
});

donate15to20.addEventListener('click', () => {
    handlePayment(15, 20);
});

// Maksājumu funkcija
function handlePayment(min, max) {
    fetch('/create-checkout-session', {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify({ minAmount: min, maxAmount: max }),
    })
    .then((response) => response.json())
    .then((sessionId) => {
        stripe.redirectToCheckout({ sessionId: sessionId });
    })
    .catch((error) => {
        console.error('Error:', error);
    });
}
