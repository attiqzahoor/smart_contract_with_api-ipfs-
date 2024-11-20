Lion&Tiger NFT Collection
This project is an ERC721-based non-fungible token (NFT) collection that allows users to mint "Lion" and "Tiger" NFTs. The NFTs are linked to dynamic metadata stored on IPFS.

Features:
Mint Lion or Tiger NFTs
Dynamic metadata for each NFT (stored on IPFS)
Supports ERC721 standard with URI storage
Admin can mint new NFTs for users
Easily customizable metadata via IPFS
Requirements
Before you start, ensure you have the following installed:

Node.js (LTS version recommended)
Hardhat (for smart contract development)
A wallet like MetaMask for interacting with Ethereum or testnet
Project Setup
1. Install Dependencies
First, clone the repository and install the necessary dependencies:

bash
Copy code
git clone <repo_url>
cd LionTigerNFT
npm install
2. Configure Network
If you're deploying to a testnet or mainnet, configure the network in the hardhat.config.js file. You'll need your Infura/Alchemy endpoint and a wallet private key to deploy.

js
Copy code
module.exports = {
  solidity: "0.8.0",
  networks: {
    rinkeby: {
      url: `https://rinkeby.infura.io/v3/YOUR_INFURA_PROJECT_ID`,
      accounts: [`0x${YOUR_WALLET_PRIVATE_KEY}`]
    }
  }
};
Make sure to replace YOUR_INFURA_PROJECT_ID and YOUR_WALLET_PRIVATE_KEY with your actual Infura project ID and wallet private key.

3. Deploy the Contract
To deploy the smart contract to a local network or a testnet, create a deployment script in the scripts/ directory. Here's an example (deploy.js):

js
Copy code
async function main() {
  const [deployer] = await ethers.getSigners();

  console.log("Deploying contracts with the account:", deployer.address);

  const LionTiger = await ethers.getContractFactory("LionTiger");
  const lionTiger = await LionTiger.deploy();
  console.log("LionTiger deployed to:", lionTiger.address);
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
Then run the deployment script:

bash
Copy code
npx hardhat run scripts/deploy.js --network rinkeby
This will deploy the contract to the Rinkeby testnet. You can replace rinkeby with any other network you've configured.

4. Minting NFTs
Once the contract is deployed, you can interact with it to mint NFTs. By default, the safeMint function mints a "Lion" NFT.

You can interact with the contract using JavaScript (via ethers.js):

js
Copy code
const contractAddress = "<deployed_contract_address>";
const abi = [ /* ABI of your contract */ ];
const provider = new ethers.providers.Web3Provider(window.ethereum);
const signer = provider.getSigner();
const contract = new ethers.Contract(contractAddress, abi, signer);

// Mint a Lion NFT
await contract.safeMint("0xRecipientAddress");
Replace "0xRecipientAddress" with the Ethereum address where you want to send the minted NFT.

5. Adding Your Own IPFS Metadata
You can upload your own metadata and images to IPFS using services like Pinata or Infura. Here's how:

Upload files to IPFS:

Upload images and metadata files to your chosen IPFS service.
After uploading, you’ll receive a CID (Content Identifier) for each file.
Update Metadata URIs:

Update the IPFS URIs in the contract code to link to your newly uploaded files.
For example, to add a new Lion NFT URI:

solidity
Copy code
string[] lionUrisIpfs = [
    "https://ipfs.io/ipfs/YOUR_NEW_CID?filename=lion_king.json",
    "https://ipfs.io/ipfs/YOUR_NEW_CID?filename=roaring_lion.json"
];
Modify the contract:

You can modify the metadata and images to represent your own NFT art and attributes.
Example of a metadata structure for a Lion:

json
Copy code
{
    "name": "Lion King",
    "description": "A majestic lion.",
    "image": "https://ipfs.io/ipfs/YOUR_IMAGE_CID",
    "attributes": [
        {
            "trait_type": "Strength",
            "value": 90
        }
    ]
}
Deploy the updated contract:

After updating the IPFS URIs, redeploy the contract to your desired network.
6. Interacting with the Contract
The contract supports the following functions:

safeMint(address to): Mints a new NFT for the specified address. By default, it mints a Lion NFT. You can add logic to switch between Lion and Tiger based on conditions.
Example:

js
Copy code
// Mint a Tiger NFT
await contract.safeMint("0xRecipientAddress"); // You can modify the contract to mint either Lion or Tiger based on your needs.
7. Testing Locally
To test the contract locally, use Hardhat’s built-in local network:

bash
Copy code
npx hardhat node
Then deploy your contract to this local network:

bash
Copy code
npx hardhat run scripts/deploy.js --network localhost
8. Verifying the Contract
After deploying the contract, you can verify it on Etherscan or any other blockchain explorer.

bash
Copy code
npx hardhat verify --network rinkeby <contract_address> <constructor_parameters>
Replace <contract_address> with the deployed contract address and <constructor_parameters> if needed.

License
This project is licensed under the MIT License - see the LICENSE file for details.

