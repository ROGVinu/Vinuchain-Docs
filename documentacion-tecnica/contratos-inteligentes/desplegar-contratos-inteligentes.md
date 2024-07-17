# Desplegar Contratos Inteligentes

VinuChain aprovecha una parte significativa de la máquina virtual de Ethereum (EVM) en su backend. Los contratos inteligentes, codificados en Solidity, pueden operar sin problemas en la red VinuChain, al igual que lo hacen en Ethereum.&#x20;

Desplegar un contrato inteligente implica enviar una transacción en la cadena VinuChain que contenga su bytecode sin especificar ningún destinatario. Es esencial disponer de tokens VC para cubrir los gastos de gas del proceso de despliegue.&#x20;

Para adquirir tokens VC de testnet, puede utilizar el faucet de testnet.&#x20;

Una vez que el contrato se despliega con éxito, se hace accesible a todos los usuarios dentro de la red VinuChain. A los contratos inteligentes se les asigna una dirección VinuChain, similar a otras cuentas de la plataforma.



## **Requisitos**

* Bytecode (código compilado) de su contrato inteligente
* VC para los costes de gas
* Script/plugin de Despliegue
* Acceso a un nodo VinuChain, ya sea ejecutando su propio nodo u obteniendo acceso API a un nodo.

Para adquirir tokens VC de testnet, puede utilizar el faucet de testnet.

## Ejemplo de despliegue de un contrato inteligente

_Próximamente_

### Despliegue mediante Remix

Para desplegar un contrato inteligente utilizando **Remix** en la **Testnet VinuChain**, siga estos sencillos pasos:

1. Conecta tu billetera Metamask a la Testnet de VinuChain.
2. En la opción Entorno, seleccione 'Proveedor Inyectado - Metamask' ( 'Injected Provider - Metamask.')
3. Establezca el **id de red** en **26**, que corresponde al id de red de VinuChain Testnet.
4. Una vez que inicie el despliegue del contrato inteligente, se desplegará con éxito en la VinuChain Testnet.



## Recursos adicionales

* [Compilación](https://ethereum.org/en/developers/docs/smart-contracts/compiling/)
* [Desplegar un contrato inteligente en Ethereum](https://ethereum.org/en/developers/tutorials/deploying-your-first-smart-contract/)

## Herramientas

* [Hardhat](https://hardhat.org/): Un entorno de desarrollo integral que facilita la edición, compilación, depuración y despliegue de contratos inteligentes utilizando la máquina virtual de Ethereum (EVM).
* [Truffle](https://www.trufflesuite.com/): Un entorno de desarrollo todo-en-uno, un marco de pruebas y una canalización de activos adaptados para proyectos de blockchain que utilizan la Máquina Virtual Ethereum (EVM).
* [Remix](https://remix.ethereum.org/): Un entorno de desarrollo integrado (IDE) que le permite escribir, compilar, depurar y desplegar código Solidity directamente en su navegador web.
* [Solidity](https://solidity.readthedocs.io/): Un sofisticado lenguaje de alto nivel, orientado a objetos, diseñado específicamente para implementar contratos inteligentes en varias plataformas blockchain.
* [OpenZeppelin](https://github.com/OpenZeppelin/openzeppelin-contracts): Mitigue los riesgos en su desarrollo de contratos inteligentes aprovechando la biblioteca de contratos inteligentes probada en batalla proporcionada por OpenZeppelin Contracts, compatible con Ethereum y otras redes de blockchain.
* [thirdweb](https://thirdweb.com/): Un completo marco de desarrollo Web3 que te equipa con todas las herramientas necesarias para conectar tus aplicaciones y juegos con redes descentralizadas.
