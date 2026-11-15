Web3 Multiplication Challenge DAppA decentralized Ethereum web application (DApp) built with Solidity, Truffle, Ganache, MetaMask, and Web3.js. Players connect their Ethereum wallet, generate random multiplication problems on-chain, submit their answers, and earn verified points stored permanently on the blockchain.Project StructureAssignment#3_Blockchain_Ebraheem_Karim/
├── contracts/
│   ├── Migrations.sol
│   └── MultiplicationGame.sol      # Main Solidity smart contract
├── migrations/
│   ├── 1_initial_migration.js
│   └── 2_deploy_contracts.js       # Deployment script for MultiplicationGame
├── build/
│   └── contracts/
│       └── MultiplicationGame.json # Compiled ABI and deployed network metadata
├── src/
│   └── index.html                  # Frontend UI and Web3/TruffleContract logic
├── truffle-config.js               # Truffle network configuration (Ganache)
└── README.md                       # Project documentation
PrerequisitesEnsure the following tools are installed before running the project:Node.js & npm (v16+ recommended)Truffle Suite (npm install -g truffle)Ganache (Desktop GUI or CLI running on http://127.0.0.1:7545)MetaMask browser extensionXAMPP (Apache server) or lite-server to serve the frontend over HTTPSetup & Installation Steps1. Start Ganache Local BlockchainOpen Ganache and start a Quickstart Ethereum workspace.Verify the RPC Server is listening at http://127.0.0.1:7545.2. Compile and Migrate Smart ContractsOpen a terminal inside the project directory (Assignment#3_Blockchain_Ebraheem_Karim) and run:# Compile the Solidity smart contracts
truffle compile

# Deploy the contracts to the local Ganache blockchain
truffle migrate --reset
This generates the compiled contract artifact at build/contracts/MultiplicationGame.json.3. Configure MetaMask for GanacheOpen MetaMask and click the network selector at the top left > Add network > Add a network manually.Enter the following details:Network Name: GanacheDefault RPC URL: http://127.0.0.1:7545Chain ID: 1337Currency Symbol: ETHCopy a private key from one of the accounts in Ganache and import it into MetaMask (Account Menu > Add account or hardware wallet > Import account).4. Host the Frontend (XAMPP)Start Apache from the XAMPP Control Panel.Create the folder C:\xampp\htdocs\multiplication-game\.Copy the following two files into C:\xampp\htdocs\multiplication-game\:index.htmlbuild/contracts/MultiplicationGame.jsonOpen your browser and navigate to:http://localhost/multiplication-game/
Commands UsedCommandPurposetruffle initInitializes a new Truffle project directory structure.truffle compileCompiles MultiplicationGame.sol into EVM bytecode and ABI JSON files.truffle migrateRuns migration scripts in migrations/ to deploy contracts to Ganache.truffle migrate --resetRe-runs all migrations from the beginning after contract changes.truffle consoleOpens an interactive JavaScript console connected to Ganache for direct contract calls.truffle testRuns automated Solidity/JavaScript tests inside the test/ directory.How to Test the ProjectA. End-to-End Frontend Testing (Browser + MetaMask)Navigate to http://localhost/multiplication-game/ with MetaMask unlocked and switched to the Ganache network.Connect Wallet: Click Connect MetaMask and approve the connection prompt.Expected Result: Button changes to Connected, your shortened wallet address (0x...) appears, and the Start Game button becomes enabled.Start a Round: Click Start Game and confirm the transaction in MetaMask.Expected Result: Status updates to "New problem generated!", a multiplication question (e.g., What is 7 × 8?) is displayed, and the answer input field appears.Submit Correct Answer: Enter the correct product, click Submit Answer, and confirm the transaction in MetaMask.Expected Result: Status updates to "Answer submitted!" and your on-chain Score increases by 10 points.Submit Incorrect Answer: Start a new round, enter a wrong answer, and submit.Expected Result: The round resets without adding points to your score.B. Truffle Console Verification (Optional)You can also verify the deployed contract directly from the terminal:truffle console
// Get deployed contract instance
let instance = await MultiplicationGame.deployed()
let accounts = await web3.eth.getAccounts()

// Start a game and inspect generated numbers
await instance.startGame({ from: accounts[0] })
let problem = await instance.getCurrentProblem({ from: accounts[0] })
console.log(problem[0].toString(), problem[1].toString())

// Submit answer and check updated score
let answer = problem[0].toNumber() * problem[1].toNumber()
await instance.submitAnswer(answer, { from: accounts[0] })
let score = await instance.getScore(accounts[0])
console.log("Score:", score.toString())
