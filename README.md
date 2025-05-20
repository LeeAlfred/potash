The overarching goals of Potash are to:
Connect Artists and Charities: Create a platform that facilitates and promotes collaborations between artists and charitable organisations.
Facilitate Fundraising: Enable artists to use their creative work to support causes they care about, thereby raising funds for charities.
Promote Artists and Their Work: Provide a platform for artists to showcase their artwork and reach a wider audience.
Increase Awareness for Charities: Help charities gain visibility and connect with new potential donors through the artists and their networks.
Create a Community: Foster a sense of community and collaboration among artists and charities.
Provide a User-Friendly Platform: Develop an accessible and easy-to-use website that effectively connects artists and charities and showcases artwork.
The intended process is that artists will have an artist page to list a maximum of 20 items of art and allocate a charity that they support, and the percentage of the sale they want to allocate to them (10% - 50%).
When a person purchases the item, the funds will automatically split into three apportioned to 1) The Artist, 2) The Charity 3) Fee

/**
 * Calculates and distributes funds from a customer's payment to an artist,
 * a charity, and the platform (Potash).
 *
 * @param {number} amount - The total amount paid by the customer.
 * @param {number} artistCharityPercent - The percentage of the payment allocated to the charity (e.g., 0.10 for 10%).
 * @param {number} platformFeePercent - The percentage of the payment taken as a platform fee (e.g., 0.05 for 5%).
 * @returns {object} An object containing the amounts distributed to the charity,
 * the artist, and the platform.  Returns null on error.
 */
function distributePayment(amount, artistCharityPercent, platformFeePercent) {
    // Input validation:
    if (typeof amount !== 'number' || amount <= 0) {
        console.error('Invalid amount: Amount must be a positive number.');
        return null; // Or you might want to throw an error: throw new Error('Invalid amount...');
    }
    if (typeof artistCharityPercent !== 'number' || artistCharityPercent < 0 || artistCharityPercent > 0.50) {
        console.error('Invalid artistCharityPercent: Percentage must be between 0 and 0.50.');
        return null;
    }
    if (typeof platformFeePercent !== 'number' || platformFeePercent < 0 || platformFeePercent > 1) {
        console.error('Invalid platformFeePercent: Percentage must be between 0 and 1.');
        return null;
    }
    if (artistCharityPercent + platformFeePercent >= 1) {
        console.error('Invalid percentages: Charity and fee percentages must add up to less than 100%.');
        return null;
    }

    const charityAmount = amount * artistCharityPercent;
    const platformFee = amount * platformFeePercent;
    const artistAmount = amount - charityAmount - platformFee;

    // Ensure no negative amounts due to rounding errors
    if (charityAmount < 0 || artistAmount < 0 || platformFee < 0) {
        console.error('Error: Negative amount calculated.  Check input values and calculations.');
        return null;
    }

    return {
        charityAmount: charityAmount,
        artistAmount: artistAmount,
        platformFee: platformFee,
    };
}

// --- Example Usage ---

// Simulate a customer, artist, charity, and Potash (the platform)
const customer = { name: 'Customer A' };
const artist = { name: 'Artist B', charityPercent: 0.20 }; // Artist donates 20% to charity
const charity = { name: 'Charity C' };
const potash = { name: 'Potash', feePercent: 0.05 };       // Potash takes a 5% fee

// Simulate a purchase
const purchaseAmount = 100;  // Customer pays $100

// Distribute the payment
const distribution = distributePayment(purchaseAmount, artist.charityPercent, potash.feePercent);

if (distribution) {
    // Output the results
    console.log(`${customer.name} paid $${purchaseAmount}.`);
    console.log(`${charity.name} receives $${distribution.charityAmount.toFixed(2)}.`);
    console.log(`${artist.name} receives $${distribution.artistAmount.toFixed(2)}.`);
    console.log(`${potash.name} receives $${distribution.platformFee.toFixed(2)} (fee).`);
} else {
    console.error('Payment distribution failed.');
}

// --- Further Considerations ---

/*
1.  Error Handling:  The code includes basic input validation.  In a real-world application, you might want to:
    * Throw exceptions instead of returning null.
    * Provide more detailed error messages to the user.
    * Log errors to a server for monitoring.

2.  Data Types:  For financial calculations, consider using a library like `Decimal.js` to avoid potential floating-point precision issues.

3.  Currency:  The code assumes a single currency.  You might need to handle multiple currencies and exchange rates.

4.  Artist-Specific Charity:  The code assumes the artist has selected a charity.  You'd need to store this information in the artist's data.

5.  Platform Fee Structure:  The platform fee is a flat percentage.  You might have a more complex fee structure (e.g., tiered fees, fixed fees).

6.  Payment Processing:  This code only calculates the distribution.  You'll need to integrate with a payment gateway (e.g., Stripe, PayPal) to actually process the transaction.

7.  Database Interaction:  In a real application, you'd store this data in a database:
    * Customer, Artist, Charity information.
    * Transaction details (amount, dates, percentages, amounts).
    * Links between artists and charities.

8.  User Interface:  You'll need to build a UI where:
    * Artists can set their charity percentage.
    * Customers can make payments.
    * The platform displays transaction details.
    * Charities can track donations.
*/
