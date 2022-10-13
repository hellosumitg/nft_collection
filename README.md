# nft_collection

Let's launch our own NFT collection - `Crypto Devs`.

![](https://i.imgur.com/fVxV66f.png)

## Requirements

- There should only exist 20 Crypto Dev NFT's and each one of them should be unique.
- User's should be able to mint only 1 NFT with one transaction.
- Whitelisted users, should have a 5 min presale period before the actual sale where they are guaranteed 1 NFT per transaction.
- There should be a website for our NFT Collection.

Lets start building 🚀

## Theory

- What is a Non-Fungible Token?
  Fungible means to be the same or interchangeable eg Eth is fungible. With this in mind, NFTs are unique; each one is different. Every single token has unique characteristics and values. They are all distinguishable from one another and are not interchangeable eg Unique Art
  ![NFT's Creation Process](https://i.imgur.com/wt4qWKT.jpg)


- What is ERC-721?
  ERC-721 is an open standard that describes how to build Non-Fungible tokens on EVM (Ethereum Virtual Machine) compatible blockchains; it is a standard interface for Non-Fungible tokens; it has a set of rules which make it easy to work with NFTs. Before moving ahead have a look at all the functions supported by [ERC721](https://docs.openzeppelin.com/contracts/3.x/api/token/erc721)

## Build

### Smart Contract

- We would also be using [`Ownable.sol`](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable.sol) from Openzeppelin which helps us manage the `Ownership` of a contract

  - By default, the owner of an Ownable contract is the account, that deployed it, which is usually exactly what we want.
  - Ownable also lets you:
    - transferOwnership from the owner account to a new one, and
    - renounceOwnership for the owner to relinquish his administrative privilege, a common pattern after an initial stage with centralized administration is over.

- We would also be using an extension of ERC721 known as [ERC721 Enumerable](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC721/extensions/ERC721Enumerable.sol)
  - ERC721 Enumerable helps us to keep track of all the tokenIds in the contract and also the tokensIds held by an address for a given contract.
  - Please have a look at the [functions](https://docs.openzeppelin.com/contracts/4.x/api/token/erc721#ERC721Enumerable) it implements before moving ahead

To build the smart contract we would be using [Hardhat](https://hardhat.org/). Hardhat is an Ethereum development environment and framework designed for full stack development in Solidity. In simple words we can write your smart contract, deploy them, run tests, and debug your code.

- To setup a Hardhat project, Open up a terminal and execute these commands

  ```bash
  mkdir nft_collection
  cd nft_collection
  mkdir hardhat
  cd hardhat
  npm init --yes 
  or 
  yarn init --yes
  npm install --save-dev hardhat 
  or
  yarn add hardhat --dev
  ```
- In the same directory where you installed Hardhat run:

  ```bash
  npx hardhat
  ```

  - Select `Create a basic sample project`
  - Press enter for the already specified `Hardhat Project root`
  - Press enter for the question on if you want to add a `.gitignore`
  - Press enter for `Do you want to install this sample project's dependencies with npm (@nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers)?`

Now we have a hardhat project ready to go!

If you are not on mac, please do this extra step and install these libraries as well :)

```bash
npm install --save-dev @nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers
or 
yarn add @nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers --dev
```

  and press `enter` for all the questions.

- In the same terminal now install `@openzeppelin/contracts` as we would be importing [Openzeppelin's ERC721Enumerable Contract](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC721/extensions/ERC721Enumerable.sol) in our `CryptoDevs` contract.

  ```bash
  npm install @openzeppelin/contracts
  or
  yarn add @openzeppelin/contracts
  ```

- We will need to call the `Whitelist Contract` that we had deployed at our previous [github repo](https://github.com/hellosumitg/whitelist-dapp/blob/main/hardhat/contracts/Whitelist.sol) to check for addresses that were whitelisted in that project and give them presale access. As we only need to call `mapping(address => bool) public whitelistedAddresses;` We can create an interface for `Whitelist contract` with a function only for this mapping, in this way we would save `gas` as we would not need to inherit and deploy the entire `Whitelist Contract` but only a part of it.

- Create a new file inside the `contracts` directory and call it `IWhitelist.sol` as shown in the repo...

- Now lets create a new file inside the `contracts` directory and call it `CryptoDevs.sol` as shown in the repo...

- Now we would install `dotenv` package to be able to import the env file and use it in our config. Open up a terminal pointing at `hardhat` directory and execute this command

  ```bash
  npm install dotenv
  or
  yarn add dotenv
  ```

- Now create a `.env` file in the `hardhat` folder and add the following lines, use the instructions in the comments to get your Alchemy API Key URL and RINKEBY Private Key. Make sure that the account from which you get your rinkeby private key is funded with Rinkeby Ether as shown in the repo...

- Lets deploy the contract to `rinkeby` network. Create a new file named `deploy.js` under the `scripts` folder as shown in the repo...

- As you can read, `deploy.js` requires some constants. Lets create a folder named `constants` under the `hardhat` folder
- Now add an `index.js` file inside the `constants` folder and add the lines of code to the file as shown in the above repo with the address of the whitelist contract that we had deployed at our previous project[github repo](https://github.com/hellosumitg/whitelist-dapp/blob/main/next-app/constants/index.js). For Metadata_URL, just copy the sample one that has been provided in this repo. We would replace this later.

- Now open the hardhat.config.js file, we would add the `rinkeby` network here so that we can deploy our contract to rinkeby as shown in this repo...
  
- Compile the contract, open up a terminal pointing at`hardhat` directory and execute this command

  ```bash
    npx hardhat compile
  ```
  
- To deploy, open up a terminal pointing at `hardhat` directory and execute this command
  ```bash
    npx hardhat run scripts/deploy.js --network rinkeby
  ```
- Save the Crypto Devs Contract Address that was printed on your terminal in your notepad, you would need it futher down in the tutorial.

### Website

- To develop the website we would be using [React](https://reactjs.org/) and [Next Js](https://nextjs.org/). React is a javascript framework which is used to make websites and Next Js is built on top of React.
- First, You would need to create a new `next` app. Your folder structure should look something like

  ```
     - nft_collection
         - hardhat
         - next-app
  ```

- To create this `next-app`, in the terminal point to nft_collection folder and type

  ```bash
    npx create-next-app@latest
  ```

  and press `enter` for all the questions

- Now to run the app, execute these commands in the terminal

  ```
  cd next-app
  npm run dev
  or
  yarn dev
  ```

- Now go to `http://localhost:3000`, your app should be running 🤘

- Now lets install Web3Modal library(https://github.com/Web3Modal/web3modal). Web3Modal is an easy-to-use library to help developers add support for multiple providers in their apps with a simple customizable configuration. By default Web3Modal Library supports injected providers like (Metamask, Dapper, Gnosis Safe, Frame, Web3 Browsers, etc), You can also easily configure the library to support Portis, Fortmatic, Squarelink, Torus, Authereum, D'CENT Wallet and Arkane.
  Open up a terminal pointing at`next-app` directory and execute this command

  ```bash
    npm install web3modal
    or 
    yarn add web3modal
  ```

- In the same terminal also install `ethers.js`

  ```bash
  npm install ethers
  or
  yarn add ethers
  ```

- In your public folder download and paste the images from this repo...

- Now go to styles folder and replace all the contents of `Home.module.css` with code shown in this repo...

- Open you `index.js` file under the `pages` folder and paste the codes shown in this repo...

- Now create a new folder under the `next-app` folder and name it `constants`.
- In the constants folder create a file, `index.js` as shown in the repo...

  - Replace `"addres of your NFT contract"` with the address of the CryptoDevs contract that you deployed and saved to your notepad.
  - Replace `---your abi---` with the abi of your CryptoDevs Contract. To get the abi for your contract, go to your `hardhat/artifacts/contracts/CryptoDevs.sol` folder and from your `CryptoDevs.json` file get the array marked under the `"abi"` key.

- Now in your terminal which is pointing to `next-app` folder, execute

  ```bash
  npm run dev
  or 
  yarn dev
  ```

Your Crypto Devs NFT dapp should now work without errors 🚀

---

### Push to github

Make sure before proceeding you have [pushed all your code to github](https://medium.com/hackernoon/a-gentle-introduction-to-git-and-github-the-eli5-way-43f0aa64f2e4) :)

---

## Deploying your dApp

We will now deploy your dApp, so that everyone can see your website and you can share it with all of your LearnWeb3 DAO friends.

- Go to https://vercel.com/ and sign in with your GitHub
- Then click on `New Project` button and then select your nft_collection repo
- When configuring your new project, Vercel will allow you to customize your `Root Directory`
- Click `Edit` next to `Root Directory` and set it to `next-app`
- Select the Framework as `Next.js`
- Click `Deploy`
  ![](https://i.imgur.com/oaEt3Tj.png)

- Now you can see your deployed website by going to your dashboard, selecting your project, and copying the `domain` from there! Save the `domain` on notepad, you would need it later.

## View your Collection on Opensea

Now lets make your collection is available on Opensea

To make the collection available on Opensea, we would need to create a metadata endpoint. This endpoint would return the metadata for an NFT given its `tokenId`.

- Open your `next-app` folder and under`pages/api` folder, create a new file named `[tokenId].js`(Make sure the name has the brackets as well). Adding the brackets helps create dynamic routes in [next js](https://nextjs.org/docs/routing/dynamic-routes)
- Add the following lines to `[tokenId].js` file. Read about adding API routes in `next js` [here](https://nextjs.org/docs/api-routes/introduction) and copy the code as shown in this repo...

- Now you have an api route which `Opensea` can call to retrieve the metadata for the NFT

- Lets deploy a new `Crypto Devs` contract with this new api route as your `METADATA_URL`

- Open your `hardhat/constants` folder and inside your `index.js` and add `const METADATA_URL` = `your domain which you saved to notepad` and add `/api/` to its end as shown in the repo...

- Save the file and open up a new terminal pointing to `hardhat` folder and deploy a new contract
  ```bash
    npx hardhat run scripts/deploy.js --network rinkeby
  ```
- Save the new NFT contract address to a notepad.

- Open up "next-app/constants" folder and inside the `index.js` file replace the old NFT contract address with the new one

- Push all the code to github and wait for vercel to deploy the new code.

- After vercel has deployed your code, open up your website and mint an NFT

- After your transaction gets successful,In your browser open up this link by replacing `your-nft-contract-address` with the address of your NFT contract in this link (i.e https://testnets.opensea.io/assets/your-nft-contract-address/1)

- Your NFT is now available on Opensea 🚀 🥳

You can check my final website here :- [nft_collection](https://nft-collection-hellosumitg.vercel.app/)

Special thanks 🙏🏻 to team@LearnWeb3DAO 😇
