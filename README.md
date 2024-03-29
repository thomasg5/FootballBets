<pre>==============================================================================================
@title: Football Bets
@author: thomasg5
@notice: Contract designed to allow users to bet on football matches scores
@dev: Personal portfolio-oriented project, not intended for deployment and use by businesses
==============================================================================================

1) Contracts established
========================

1.1 - BetGenerator

	Purpose: define its owner and future manager of all betting contests being generated by himself or any assistants through the 'generator' function.

1.2 - FootballBets

	Purpose: receive new bets for a specific football match and in the end distribute the prize 'bowl' equally amongst winners or rewarding the contract's manager in the exception.

2) State/local variables explained
==================================

2.1 - BetGenerator

	2.1.1 - owner: owner of the contract and future manager of all betting options.
	
	2.1.2 - lastContract: address of the last contract created by the generator.

2.2 - FootballBets

	2.2.1 - manager: person responsible for the contract's management, whose duties are to follow-up users participation, halt new bets before the match starts and finalize the betting option after the
	match ended with the final score.

	2.2.2 - betValue: exact bet value in ether necessary to be sent by the user to participate.
	
	2.2.3 - betState: current state of the betting option, which can be Open, Halted or Closed. 
	
	2.2.4 - matchDispute: match being betted.
	
	2.2.5 - matchResult: final score of the match.
	
	2.2.6 - bets: array of bets created by the participants.
	
	2.2.7 - prizeAmount: total value of bets sent to be distributed.
	
	2.2.8 - checkWinner: mapping to store all betters addresses who won the bet. Used to filter unique addresses for further assignment in the betWinners array.
	
	2.2.9 - betWinners: array of unique addresses of all winners.

	2.2.10 - individualPrize: amount of wei to be given to each winning address.
	
	2.2.11 - paidWinner: mapping to store all winners already paid.

3) Functions explained
======================

3.1 - BetGenerator

	3.1.1 - generator: deploys new betting contract instances to be managed.
	Parameters: name of the league in which the match happens and the match itself (e.g. "La Liga", "Barcelona x Real Madrid" | both strings).
	Modifiers: none.
	
3.2 - FootballBets

	3.2.1 - newBets: includes a new bet from the participant in the pool.
	Parameters: score betted for the match (e.g. "1:1" | string).
	Modifiers: can only be executed during the 'Open' state of the bet. 
	
	3.2.2 - haltBets: halts the entrance of participants in the bet by changing the betState.
	Parameters: none.
	Modifiers: can only be executed by the owner/manager during the 'Open' state of the bet.
	
	3.2.3 - checkPrizeAmount: checks how much wei is available for distribuition/payment. 
	Parameters: none.
	Modifiers: none.
		
	3.2.4 - finalizeBetOption: ends the bet contest and calculates the due prizes (winners) or profit (manager) for the respective beneficiaries. 
	Parameters: the final match result (e.g. "2:1" | string).
	Modifiers: can only be executed by the owner/manager during the 'Halted' state of the bet.
	
	3.2.5 - withdrawPrize: allows the winning participants to withdraw their prize.
	Parameters: none.
	Modifiers: can only be executed during the 'Closed' state of the bet.
	
	3.2.6 - receiveContractProfit: allows the manager to collect its profit from the match when no winners were taken.
	Parameters: none.
	Modifiers: can only be executed by the owner/manager during the 'Closed' state of the bet.
	
4) Security measures
====================

4.1 - BetGenerator

	4.1.1 - generator: when creating a new instance of FootballBets, it is given as a parameter the owner variable instead of the sender in order to allow any assistant to create new bet options while
	avoiding malicious third-parties to create instances and manage them to steal the prize.
	
4.2 - FootballBets

	4.2.1 - newBets: two requirements were given - one to block the manager address from participating and a second to validate the bet value before accepting it - so no incompatible bets can be
	inserted.
	
	4.2.2 - finalizeBetOption: besides the modifier conditions, the prize amount destined to the participant winners or the manager is setted to zero before allowing the payments, avoiding
	further in both ways the known reentrancy attack in which the attacker causes a never ending withdraw pattern reexecuting a transfer before it changes the value due to zero. 

	4.2.3 - finalizeBetOption: in case a user enters the contest with duplicated bets, which end up being winners, it was implemented an unique identifier variable to store the winners addresses,
	despite how many right bets they got, to avoid duplicate payments (gas waste) and incorrect calculation of the payable prize amount to each user.
	
	4.2.4 - withdrawPrize: besides the modifiers, it is established before paying the winner that his paid status will become true, to avoid (as another measure) the reentrancy attack mentioned before.
	
5) Notes
========

	5.1: Constructors and events are simple and were implemented following usual behaviour, hence no description is necessary.
	
	5.2: It is true that this contract stores more information than needed and possibly could be written in a more concise manner, but since it's a portfolio project I opted to demonstrate more clearly 
	and diversily my capacity to implement as much resources as possible.

6) Technical specifications used
================================

	Truffle v5.4.1 (core: 5.4.1)
	Solidity v0.8.6 (solc-js)
	Node v14.17.3
	Web3.js v1.4.0
</pre>
