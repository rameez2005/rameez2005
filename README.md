const fetch = require('node-fetch');
const fs = require('fs');

// Function to fetch a random quote
async function fetchRandomQuote() {
    try {
        const response = await fetch('https://api.quotable.io/random');
        const data = await response.json();
        return data.content;
    } catch (error) {
        console.error('Error fetching random quote:', error);
        return 'An error occurred while fetching the quote.';
    }
}

// Function to update README.md file with the fetched quote
async function updateReadmeWithQuote() {
    const quote = await fetchRandomQuote();
    fs.readFile('README.md', 'utf8', (err, data) => {
        if (err) {
            console.error('Error reading README.md:', err);
            return;
        }
        const updatedData = data.replace(/<!--STARTS_HERE_QUOTE_README-->[\s\S]*<!--ENDS_HERE_QUOTE_README-->/, `<!--STARTS_HERE_QUOTE_README-->\n${quote}\n<!--ENDS_HERE_QUOTE_README-->`);
        fs.writeFile('README.md', updatedData, 'utf8', (err) => {
            if (err) {
                console.error('Error writing to README.md:', err);
                return;
            }
            console.log('README.md updated with a random quote.');
        });
    });
}

// Call the function to update README.md with a random quote
updateReadmeWithQuote();
