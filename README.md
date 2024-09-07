Código del Contrato Inteligente
Aquí tienes un contrato inteligente básico en Solidity para un sistema de lotería. Este contrato permite a los usuarios comprar un billete de lotería y realizar un sorteo.

solidity
Copiar código
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Loteria {
    address public gerente;
    address[] public jugadores;
    uint256 public precioBillete = 0.01 ether; // Precio de un billete

    constructor() {
        gerente = msg.sender;
    }

    function entrar() public payable {
        require(msg.value == precioBillete, "Precio del billete incorrecto");

        jugadores.push(msg.sender);
    }

    function aleatorio() private view returns (uint) {
        return uint(keccak256(abi.encodePacked(block.difficulty, block.timestamp, jugadores)));
    }

    function elegirGanador() public soloGerente {
        require(jugadores.length > 0, "No hay jugadores en la lotería");

        uint indice = aleatorio() % jugadores.length;
        address ganador = jugadores[indice];

        payable(ganador).transfer(address(this).balance);

        // Reiniciar el array de jugadores para la próxima lotería
        jugadores = new address ;
    }

    modifier soloGerente() {
        require(msg.sender == gerente, "Solo el gerente puede llamar a esto");
        _;
    }

    function obtenerJugadores() public view returns (address[] memory) {
        return jugadores;
    }
}
Pasos para Desplegar el Contrato
Configura el Entorno:

Instala Node.js y npm.
Instala Truffle y Ganache para pruebas locales o utiliza Remix o Hardhat.
Desplegar el Contrato:

Crea un proyecto Truffle o usa Remix IDE para el despliegue.
Configura la red para usar Rinkeby u otra red de prueba.
Despliega el contrato usando scripts de migración de Truffle o Remix IDE.
Obtén los Detalles del Despliegue:

Después del despliegue, obtendrás un hash de transacción y la dirección del contrato.
Para redes de prueba, también tendrás un hash de transacción.
Ejemplo de Despliegue y Pruebas
Usando Remix
