# Ali_Khatami_Lottery2(learning from the video of Patrick Collins)

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

we can see the contract comes with this function fulfillRandomWords <br>












