<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>My DApp</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <header>
        <div class="container">
            <h1>My DApp</h1>
            <nav>
                <ul>
                    <li><a href="#">Home</a></li>
                    <!-- Add more navigation links as needed -->
                </ul>
            </nav>
        </div>
    </header>

    <main>
        <div class="container">
            <h2>Welcome to My DApp</h2>
            <div class="input-group">
                <label for="inputAmount">Enter Amount:</label>
                <input type="text" id="inputAmount" placeholder="0 ETH">
                <button onclick="deposit()">Deposit</button>
            </div>
            <div id="output"></div>
        </div>
    </main>

    <footer>
        <div class="container">
            <p>&copy; 2024 My DApp. All rights reserved.</p>
        </div>
    </footer>

    <script src="web3.min.js"></script>
    <script src="app.js"></script>
</body>
</html>
/* Reset CSS */
* {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

/* Global Styles */
body {
    font-family: Arial, sans-serif;
    line-height: 1.6;
    background-color: #f5f5f5;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
}

header {
    background-color: #fff;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    padding: 20px 0;
}

header .container {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

header h1 {
    font-size: 24px;
    color: #333;
}

nav ul {
    list-style-type: none;
}

nav ul li {
    display: inline;
    margin-right: 20px;
}

nav ul li a {
    text-decoration: none;
    color: #666;
    transition: color 0.3s;
}

nav ul li a:hover {
    color: #333;
}

main {
    padding: 50px 0
// Connect to Ethereum network
const web3 = new Web3(Web3.givenProvider || "http://localhost:8545");

// ABI (Application Binary Interface) of your smart contract
const abi = [
    // Include ABI of your contract functions
];

// Address of your deployed smart contract
const address = '0x123456789...'; // Replace with your contract address

// Create contract instance
const contract = new web3.eth.Contract(abi, address);

// Function to deposit funds into the smart contract
function deposit() {
    const amount = document.getElementById("inputAmount").value;
    
    // Convert amount to Wei (1 Ether = 10^18 Wei)
    const amountWei = web3.utils.toWei(amount, 'ether');
    
    // Send transaction to smart contract
    contract.methods.deposit().send({ from: web3.eth.defaultAccount, value: amountWei })
        .on('transactionHash', function(hash){
            console.log('Transaction Hash:', hash);
        })
        .on('confirmation', function(confirmationNumber, receipt){
            console.log('Confirmation Number:', confirmationNumber);
            console.log('Receipt:', receipt);
            document.getElementById("output").innerText = 'Transaction confirmed!';
        })
        .on('error', function(error){
            console.error('Error:', error);
            document.getElementById("output").innerText = 'Error: ' + error.message;
        });
}
