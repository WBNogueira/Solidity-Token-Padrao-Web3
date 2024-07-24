# # Criando o Seu Primeiro Token do Zero nos Padrões Web3

Este repositório fornece um guia passo a passo para a criação do seu próprio token utilizando os padrões Web3. Vamos configurar a Wallet Metamask, ajustar as informações do RPC e utilizar a IDE Remix para criar o arquivo `token.sol` em Solidity.

## Pré-requisitos

- **Metamask**: Uma extensão de navegador que permite interagir com a blockchain Ethereum.
- **Remix IDE**: Um ambiente de desenvolvimento integrado que permite escrever, compilar e implantar contratos inteligentes em Solidity.

## Configurando a Wallet Metamask

1. Instale a extensão Metamask em seu navegador.
2. Crie uma nova conta ou importe uma existente.
3. Configure a rede RPC:
   - Clique no ícone da Metamask.
   - Selecione "Custom RPC" e insira as informações da rede que deseja usar.

## Criando o Token em Solidity

Utilizaremos a IDE Remix para escrever e compilar nosso contrato inteligente em Solidity.

### Passos

1. Abra a [Remix IDE](https://remix.ethereum.org/).

2. Crie um novo arquivo chamado `token.sol`.

3. Copie e cole o código abaixo no arquivo `token.sol`:
   
   ```solidity
   // SPDX-License-Identifier: GPL-3.0
   pragma solidity ^0.8.7;
   
   // ERC Token Standard #20 Interface
   interface ERC20Interface {
       function totalSupply() external view returns (uint);
       function balanceOf(address tokenOwner) external view returns (uint balance);
       function allowance(address tokenOwner, address spender) external view returns (uint remaining);
       function transfer(address to, uint tokens) external returns (bool success);
       function approve(address spender, uint tokens) external returns (bool success);
       function transferFrom(address from, address to, uint tokens) external returns (bool success);
   
       event Transfer(address indexed from, address indexed to, uint tokens);
       event Approval(address indexed tokenOwner, address indexed spender, uint tokens);
   }
   
   // Contrato do token
   contract DIOToken is ERC20Interface {
       string public symbol = "DIO";
       string public name = "DIO Coin";
       uint8 public decimals = 2;
       uint256 public _totalSupply;
   
       mapping(address => uint) balances;
       mapping(address => mapping(address => uint)) allowed;
   
       constructor() {
           _totalSupply = 1000000;
           balances[msg.sender] = _totalSupply;
       }
   
       function totalSupply() public override view returns (uint256) {
           return _totalSupply;
       }
   
       function balanceOf(address tokenOwner) public override view returns (uint256) {
           return balances[tokenOwner];
       }
   
       function transfer(address to, uint tokens) public override returns (bool success) {
           require(tokens <= balances[msg.sender]);
           balances[msg.sender] = balances[msg.sender] - tokens;
           balances[to] = balances[to] + tokens;
           emit Transfer(msg.sender, to, tokens);
           return true;
       }
   
       function approve(address spender, uint256 tokens) public override returns (bool success) {
           allowed[msg.sender][spender] = tokens;
           emit Approval(msg.sender, spender, tokens);
           return true;
       }
   
       function allowance(address tokenOwner, address spender) public override view returns (uint remaining) {
           return allowed[tokenOwner][spender];
       }
   
       function transferFrom(address from, address to, uint256 tokens) public override returns (bool success) {
           require(tokens <= balances[from]);
           require(tokens <= allowed[from][msg.sender]);
           balances[from] = balances[from] - tokens;
           allowed[from][msg.sender] = allowed[from][msg.sender] - tokens;
           balances[to] = balances[to] + tokens;
           emit Transfer(from, to, tokens);
           return true;
       }
   }
   ```

4. Compile o contrato usando o compilador da Remix.

5. Implante o contrato na rede configurada na Metamask.

## Conclusão

Parabéns! Você criou com sucesso o seu próprio token utilizando os padrões Web3. Sinta-se à vontade para modificar e expandir o contrato conforme suas necessidades.

## Referências

- [Documentação da Metamask](https://metamask.io/)
- [Remix IDE](https://remix.ethereum.org/)
- [Solidity Documentation](https://docs.soliditylang.org/)
