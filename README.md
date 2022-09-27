# ERC721-fullstack-Dapp-Tutorial

Let's put Taj Mahal on sale !! Yes i am not out of my mind and it is completely possible with the help of blockchain technology.Of course Blockchian will protect us from intervention of third parties. What we will be building is a digital asset (ERC721 token) on blockchain with the help of solidity(by writing smart contracts).We will then use IPFS to deploy our Dapp.

Setup Environment We need to get hold of truffle,get it via npm install -g truffle.You need to have Node.js v6+ LTS and npm (comes with Node) and Git too.This tutorial is best suited for unix environment. We would also be needing ganache or testrpc for local deployment.

Let's conspire to sell Taj Mahal Open Terminal Make a suitable folder with mkdir tajmahal and navigate into folder cd tajmahal Now initialize the project with truffle init . You will notice contracts migrations test truffle-config.js truffle.js directories and files being added to folder. contracts/: Contains the Solidity source files for our smart contracts. There is an important contract in here called Migrations.sol. migrations/: Truffle uses a migration system to handle smart contract deployments. A migration is an additional special smart contract that keeps track of changes. test/: Contains both JavaScript and Solidity tests for our smart contracts truffle.js: Truffle configuration file In truffle.js replace the stuff with following code. // Allows us to use ES6 in our migrations and tests. // require('babel-register') // require('babel-polyfill')

module.exports = { networks: { development: { host: '127.0.0.1', port: 7545, network_id: '*', // Match any network id } }, }; Now add the following contracts in the contracts directory and then hit truffle compile I have created Tajmahal.sol (because written in soilidty) that contains buisness logic.According to this contract ONLY ONE Tajmahal can be created.It will be non fungible an unique as it implement erc721 standards.Since we do not want some overflow in our arithmetic operations we have used safemath.sol library from openzepplin.ownable.sol is again from openzepplin for restricting use of some contract functions to owners. Tajmahal.sol erc721.sol ownable.sol safemath.sol Since we have smart contracts in place we need to migrate these contract.But before we proceed to this step we need to install and run ganache in background. ganache After installing ganache and running in backgroud navigate to folder migrations and create a file named 2_deploy_contracts.js and add the following code in it.link var Tajmahal = artifacts.require("Tajmahal");

module.exports = function(deployer) { deployer.deploy(Tajmahal); } After following the above steps hit truffle migrate (make sure ganache is running in background). In Ganache, note that the state of the blockchain has changed. The blockchain now shows that the current block, previously 0, is now 4. In addition, while the first account originally had 100 ether, it is now lower, due to the transaction costs of migration. ganache Hola your contract is deployed on local blockchain!!! But we still have long way to go. Ok those following genuinely till here have a homework to do.Try testing smart contracts with truffle framework. Deploy smart contracts Logicaly speaking there are tons of way to deploy smart contract on tons of network(ropsten,rinkbey,mainnet,private).We could use truffle also to deploy our network.In this i will be discussing deployment of smart contracts on ropsten network.

First we will be needing a wallet.Go and install chrome extension called Metamask. Once installed, you'll see the MetaMask fox icon next to your address bar. Click the icon and you'll see this screen appear: wallet Click Accept to accept the Privacy Notice. Then you'll see the Terms of Use. Read them, scrolling to the bottom, and then click Accept there too. wallet Now you'll see the initial MetaMask screen. Set your password. wallet Now we need to connect MetaMask to the blockchain.Connect to Ropsten network. wallet Create an Account wallet Copy Account Address wallet Request Free ether from here or here. Paste the copied account address in text box and click on send me 1 test ether. wallet you got a ether !!!! wallet now, we have to the paste contracts in remix IDE remix step11 click on Run. Select Injected Web 3 Ropsten under environment and the account in Metamask is shown here under Account with balance ether as well.Hit the deploy button. step1 Metamask will pop up with transaction details. step2 Now enter the gass price.Higher you pay faster it gets executed. step3 After transaction is completed you will be directed with etherscan link. step4 Important ! please note down contract address and abi as they would be required for frontend integration. Frontend for smart contract and interaction with it. Since we have made and deployed our smart contracts,we need to put a pretty face to it so that normal user can interact with it.

Make a folder called frontend in the directory. Make a file called index.html in frontend folder and add the following code from here. You can add following Tajmahal images from here to your frontend folder. frontend Now we have frontend in place,we need this frontend to interact with our smart contract.For this we require web3js

<script src="https://cdn.jsdelivr.net/gh/ethereum/web3.js/dist/web3.min.js"></script> code imports web3js in html file.
Now lets ponder over following snippet in index.html file. <script>

    if (typeof web3 !== 'undefined') {
        web3 = new Web3(web3.currentProvider);
    } else {
        // set the provider you want from Web3.providers
        web3 = new Web3(new Web3.providers.HttpProvider("http://localhost:8545"));
    }

</script>
It's saying that if web3 is not undefined, then we'll use that as our provider. If it's undefined (else), we can manually specify the provider ourselves. You may be wondering, how would web3 be defined? Well, if you're using the Chrome extension MetaMask (which we will use later in this course) or an Ethereum browser like Mist, the provider is automatically injected. Now go back to Remix IDE, click on the Compile tab and click Details. Scroll down until you see the Interface - ABI section and click the copy icon as shown below: ABI In index.html in the following code snippet paste your contract adsress and abi copied in above step. <script>

    // Previous if/else statement removed for brevity

    web3.eth.defaultAccount = web3.eth.accounts[0];

    //paste your abi function here
    var TajmahalContract = web3.eth.contract(YOUR ABI);
    
    //paste your contract address here.
    var Tajmahal = TajmahalContract.at('PASTE CONTRACT ADDRESS HERE');
    console.log(Tajmahal);

</script>
Deployment to IPFS Install IPFS as described at https://ipfs.io/docs/install/ Run IPFS daemons ipfs daemon then run ipfs add -r frontend/ You should see something like this added QmeYxwj4CwCeGVhwi3xLrmBZUUFQdftshSiGLrTdTnWEVV frontend The hash on the last line is the root of your site, you can visit is by opening https://gateway.ipfs.io/ipfs/. So the example site is at https://gateway.ipfs.io/ipfs/QmWrrQHELjXW41tkM2RUCGGyyHqKzauCZMSvcxa3271GP6/ Publish to IPNS ipfs name publish That will return your peerID and the hash you are publishing to it. You can confirm by running ipfs name resolve or by viewing https://gateway.ipfs.io/ipns/ (notice the directory is ipns not ipfs). Your site is deployed !!! Hola !! you completed the full stack ERC721 tutorial !!
