### How to Integrate Web3Auth in Fixtix

### Step 1: Create Metadata

Before deploying the smart contract, the first step is to create the metadata for the NFT. Here's a sample structure:

```json
{
  "name": "TOKEN2049 Singapore Pass",
  "description": "This pass grants access to all sessions and networking events at TOKEN2049 Singapore, a major event for the blockchain and fintech industry.",
  "image": "https://example.com/path/to/your/image.png",
  "external_url": "https://token2049.com/events/singapore",
  "attributes": [
    {
      "trait_type": "Event Name",
      "value": "TOKEN2049 Singapore"
    },
    {
      "trait_type": "Date",
      "value": "2024-09-17 to 2024-09-22"
    },
    {
      "trait_type": "Location",
      "value": "Marina Bay Sands, Singapore"
    },
    {
      "trait_type": "Ticket Type",
      "value": "General"
    },
    {
      "trait_type": "Access Level",
      "value": "Sessions and Networking"
    }
  ],
  "properties": {
    "creators": [
      {
        "address": "0xYourWalletAddressHere",
        "share": 100
      }
    ],
    "files": [
      {
        "uri": "https://example.com/path/to/your/image.png",
        "type": "image/png"
      }
    ],
    "event_details": {
      "start_time": "2024-09-17T10:00:00Z",
      "end_time": "2024-09-22T22:00:00Z",
      "venue": "Marina Bay Sands, Singapore"
    }
  }
}
```

### Step 2: Upload Metadata to IPFS

Once you’ve created the metadata, upload it to an IPFS service like **Pinata** or **Filecoin**. This will give you an IPFS link, which you'll need in the next step for deploying the smart contract.

### Step 3: Modify and Deploy the Contract

1. **Use the Contract**: You can take this contract from [this Gist](https://gist.github.com/Gautambnsl/3108eac99e7d27f2033a3343c7f131df).
   
2. **Change `baseURI` Function**: Update the `baseURI` function in the contract to point to the new IPFS link from Step 2.

3. **Deploy on Polygon Testnet**: Deploy the modified contract on the Polygon Amoy testnet (Chain ID: 80002). Ensure you have the correct setup and network configuration for Polygon.

4. **Transfer Ownership**: After deploying, transfer the contract ownership to the **Web3Auth NFT Checkout** service address so they can mint the NFT after receiving payment.

   - Web3Auth Address: `0x99B8f8Ef183694f04477e30Ddce1D521f4B733bE`

### Step 4: Submit Contract Details to Web3Auth

After successfully deploying the contract and transferring ownership, gather the following details:

- **Contract address** (e.g., `0x7085CBB977f24B4675CBc408a552e241D7C0b779`)
- **Contract type** (e.g., ERC721)
- **Chain ID** (e.g., Polygon Amoy Testnet - 80002)
- **NFT name** (e.g., Demo Singapore Token2049)
- **NFT image URL** (e.g., `https://olive-urgent-coyote-736.mypinata.cloud/ipfs/QmUcdxf1c5k3nX1LDKoarf5iKoc7XArgCS6KvqVFRU8rAx`)
- **Minting limit** (e.g., 1 mint per wallet)
- **Quantity** (e.g., 200 NFTs)
- **Price in USD** (e.g., $1 per NFT)

Then, email this information to Web3Auth at `product@tor.us`.

### Step 5: Web3Auth Response

Web3Auth should provide you with the following information after submission:

1. **Contract ID**
2. **Publishable key**
3. **Secret key**

### Step 6: Embed Web3Auth NFT Checkout

To enable the NFT purchase, you will need to embed the NFT checkout into your frontend using an iframe. Here’s a sample iframe setup:

```html
<iframe 
  src="https://develop-nft-checkout.web3auth.io/?contract_id=your_contract_id&receiver_address=user_wallet_address&api_key=your_publishable_key" 
  title="FIXTIX NFT Checkout">
</iframe>
```

Replace the following values:

- `your_contract_id` – The contract ID from Web3Auth.
- `user_wallet_address` – The wallet address from the user, which you will obtain from your frontend.
- `your_publishable_key` – The publishable key provided by Web3Auth.

### Step 7: Closing the NFT Checkout

To close the NFT checkout iframe when the transaction is complete, listen for the `message` event in your frontend:

```js
window.addEventListener('message', function (event) {
  if (event.data === 'close-nft-checkout') {
    // Logic to close the iframe
  }
});
```

This concludes the process for integrating Web3Auth with Fixtix.
