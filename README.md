![image](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/11e43e73-ffc4-4181-a9af-e7ffe3a63685)# Ali_Khatami_Lottery2(learning from the video of Patrick Collins)

### Adding Events

Go to the link below to know what is an Event

https://docs.google.com/document/d/1mtPf-QikpzU1TWfyBokJZLSkR9RihGlblNPJnVRLGAA/edit?usp=sharing

![l25](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/12960201-b8e8-47a4-87af-054a8810e6e3)

Here we have added an event <br>

![l26](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/4e07016a-89a8-4789-a3b6-19e3ff6d6916)

Here we have added to emit that event <br>

We emit an event when we update a dynamic array or mapping <br>

We will name the events with function name reversed <br>


### Implementing Chainlink VRF

![l64](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/2d745052-e8e6-41de-90a5-8c03d307a73e)

The above function is gonna be called by chainlink keepers network <br>
So that it can autiomatically run without having us actually interact with it <br>

![l65](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/ff671207-9990-4121-b51f-6ba397332e0e)

We gonna make it external because external are cheaper than public <br>

because solidity knows that our own contract can call this <br>


In order us to pick a random winner we have to do actually two things <br>

We first have to request the random number <br>

Then once we get it do something with it <br>


Chainlink VRF is a two transaction process <br>
This is actually intentional ,having random nunmber in two transaction <br>
is actually better than having it in one <br>

if it just one transaction people could just brute force tries simulating calling this transaction <br>

Simulate calling these transaction to see what they can manipulate to make sure they are the winner <br>


We wanna make sure that this is absolutely fair and nobody can manipulate our smart contract <br>

The pickRandomChampion function gonna request it <br>

and in the second function thatwe will create random number gonna returned <br>



![l66](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/749ce971-0fb3-4601-a861-2201f4464551)

We have changed it from pick to request <br>


![l67](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/21f0598a-13ea-4fb1-b603-e2ccfce33254)

We have created a new function <br>

In order to make our contract VRF we have to import chainlink code <br>

![c1](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/15e271ce-09a6-4fa6-bfc6-7f832c1274b8)

we will go to ducumentation and copy the above line shown below <br>

``` import "@chainlink/contracts/src/v0.8/VRFConsumerBaseV2.sol"; ``` <br>

Since we are importing chainlink/contracts we gonna add that in using yarn <br>


![c3](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/a3b35071-b43f-4621-88c1-3d16fd0bd16e)

For that we need to use the command shown below <br>

```yarn add --dev @chainlink/contracts``` <br>

We ned to make our lottery VRF consumer Base able <br>


![c4](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/f843888d-ac49-4804-b09a-4804f5662a58)

We will click the above options sequentially <br>

![c6](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/ff083d06-64da-4e8f-898d-305b93287655)

we can see the contract comes with this function fulfillRandomWords and it is internal and virtual <br>


Virtual means it is expecting to be overriden becauser the VRF coordinator which we will use can call this function <br>




```solidity

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@chainlink/contracts/src/v0.8/VRFConsumerBaseV2.sol";

error akrkLottery_NotEnoughEthEntered();

contract akrkLottery {
    //below we gonna pick minimum price and it gonna be storage variable
    //visibiliy will be private but it will be configurable
    //We will cover our both storage and non storage variables under state variables section
    /* State variables */

    uint256 private immutable i_welcomeFee;
    //We probably also nedd to track of all the users who entered the lottery
    //participants is a storage variable because we gonna modify this a lot
    address payable[] private s_participants;

    /* Events */

    event LotteryEnter(address indexed participants);

    // to configure it we st constructor below

    constructor(uint256 welcomeFee) {
        i_welcomeFee = welcomeFee;
    }

    //to enter the lottery we created a function below

    function enterLottery() public payable {
        if (msg.value < i_welcomeFee) {
            revert akrkLottery_NotEnoughEthEntered();
        }

        s_participants.push(payable(msg.sender));

        emit LotteryEnter(msg.sender);
    }

    //to pick a random champion we created the function below
    //The below function is gonna be called by chainlink keepers network

    function requestRandomchampion() external {}

    function fulfillRandomWords() internal override {}

    //we want other users to see entrance fee so we created the function below
    /*View/Pure Function*/
    function getEntranceFee() public view returns (uint256) {
        return i_welcomeFee;
    }

    //to know who are in the participants array the function is created below
    function getParticipant(uint256 index) public view returns (address) {
        return s_participants[index];
    }
}


```


![c8](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/b6383018-3260-47f1-a160-cc2f252ca513)

here akrkLottery contract is inheriting all the functionalities of VRFConsumerBaseV2.sol contract <br>


VRF Coordinator is the address of the contract that does random number verification <br>



![c9](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/5d4a5c40-c82d-4d91-bc9f-aa09d4d3c2d6)

In the constructor of akrkLottery.sol we will add VRFConsumerBaseV2() constructor and pass to it <br>
address of VRFCoorinator contract <br>


![c10](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/5698073f-f8b3-456e-b976-7c4b6ad01724)


In our main constructor we gonna pass that parameter as well <br>


![c11](https://github.com/C191068/Ali_Khatami_Lottery2/assets/89090776/f9f1e23b-5a7f-4a3e-aec6-a1da2f539c47)

Now at the above we can see that our code is compuiled successfully <br>

it is given below:

```solidity

//SPDX-License-Identifier: MIT
pragma solidity ^0.8.19;

import "@chainlink/contracts/src/v0.8/VRFConsumerBaseV2.sol";

error akrkLottery_NotEnoughEthEntered();

contract akrkLottery is VRFConsumerBaseV2 {
    //below we gonna pick minimum price and it gonna be storage variable
    //visibiliy will be private but it will be configurable
    //We will cover our both storage and non storage variables under state variables section
    /* State variables */

    uint256 private immutable i_welcomeFee;
    //We probably also nedd to track of all the users who entered the lottery
    //participants is a storage variable because we gonna modify this a lot
    address payable[] private s_participants;

    /* Events */

    event LotteryEnter(address indexed participants);

    // to configure it we st constructor below

    constructor(address vrfCoordinatorV2, uint256 welcomeFee) VRFConsumerBaseV2(vrfCoordinatorV2) {
        i_welcomeFee = welcomeFee;
    }

    //to enter the lottery we created a function below

    function enterLottery() public payable {
        if (msg.value < i_welcomeFee) {
            revert akrkLottery_NotEnoughEthEntered();
        }

        s_participants.push(payable(msg.sender));

        emit LotteryEnter(msg.sender);
    }

    //to pick a random champion we created the function below
    //The below function is gonna be called by chainlink keepers network

    function requestRandomchampion() external {}

    function fulfillRandomWords(
        uint256 requestId,
        uint256[] memory randomWords
    ) internal override {}

    //we want other users to see entrance fee so we created the function below
    /*View/Pure Function*/
    function getEntranceFee() public view returns (uint256) {
        return i_welcomeFee;
    }

    //to know who are in the participants array the function is created below
    function getParticipant(uint256 index) public view returns (address) {
        return s_participants[index];
    }
}

```






















