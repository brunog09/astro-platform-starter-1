<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Voting System</title>
</head>
<body>
    <h1>Vote for Your Favorite Option</h1>
    <form id="voteForm">
        <label>
            <input type="radio" name="option" value="Option A"> Option A
        </label><br>
        <label>
            <input type="radio" name="option" value="Option B"> Option B
        </label><br>
        <button type="submit">Submit Vote</button>
    </form>
script>
        document.getElementById('voteForm').addEventListener('submit', async function(event) {
            event.preventDefault();
            const formData = new FormData(this);
            const response = await fetch('/.netlify/functions/submit-vote', {
                method: 'POST',
                body: JSON.stringify({ option: formData.get('option') }),
                headers: { 'Content-Type': 'application/json' }
            });
            const result = await response.json();
            alert(result.message);
        });
    </script>
</body>
</html>
Step 2: Create the Netlify Function
Create a directory named functions in your project root and add a file named submit-vote.js:
exports.handler = async (event, context) => {
    if (event.httpMethod !== 'POST') {
        return {
            statusCode: 405,
            body: JSON.stringify({ message: 'Method Not Allowed' })
        };
    }

    const { option } = JSON.parse(event.body);
