# blackJack
a text based blackjack game
#include "stdafx.h"
#include<iostream>
#include<string>
#include<stdlib.h>
#include<ctime>


using namespace std;

string greeting();
int buyChips(string);
int riskChips(string);
int yourScore(string);
int dealerScore(int, string);

int main()
{
	srand(time(NULL));

	int bank, bet, quit, twentyOne, yourCards, scoreToBeat;
	string response, playAgain, playerName, yourName, nameOfPlayer;
	playerName = greeting();
	bank = buyChips(playerName);
	cout << "\n" << playerName << " has successfully bought $" << bank <<
		" worth of gaming chips.\n" << endl;
	bet = riskChips(playerName);

	do {
		yourCards = yourScore(playerName);
		scoreToBeat = dealerScore(yourCards, playerName);
		if (scoreToBeat == 21)		//if the dealer has 21 no matter the player will lose 
		{
			bank -= bet;
			cout << "Dealer has BlackJack!" << endl;
		}
		else if (yourCards == 21) 			//you have a score of 21, dealer does not
		{
			bank += bet;
			cout << "\n" << playerName << " wins." << endl;
			cout << "\nCongratulations on a black jack" << endl;
		}
		else if ((yourCards > 21) || (yourCards == scoreToBeat))  		//player busts or dealer has equal score, neither has a 21
		{
			bank -= bet;
			cout << "\n" << playerName << " looses." << endl;
		}
		else if ((yourCards > scoreToBeat) && (yourCards<21) || (scoreToBeat>21))  	//player has a better hand & player has score under 21, Or Dealer Busts.
		{
			bank += bet;
			cout << "\n" << playerName << " wins." << endl;
		}
		else if ((yourCards < scoreToBeat) && (yourCards < 21))		//already checked that Dealers didnt bust, checks if your score is less than the dealers
		{
			bank -= bet;
			cout << "\n" << playerName << " looses." << endl;
		}

		else 						//yourCards>ScoreToBeat
		{
			bank += bet;
			cout << "\n" << playerName << " wins." << endl;
		}


		cout << "\n" << playerName << " has " << bank << " chips remaining" << endl;
		if (bank == 0)
		{
			cout << playerName << " has run out of chips. Please leave the table and re-buy in." << endl;
			exit;
		}

		cout << "Press 1 to continue betting or 0 to quit: ";
		cin >> quit;
		if (quit == 0)
		{
			cout << "Thanks for playing group 2s BlackJack Game. \nWe hope you had fun this semester" << endl;
			exit;
		}
		else
		{
			while (bank > 0)
			{
				bet = riskChips(playerName);
				yourScore(playerName);
				dealerScore(yourCards, playerName);


				if (scoreToBeat == 21)
				{
					bank -= bet;
					cout << "\n" << playerName << " looses." << endl;
					cout << "\nDealer has blackjack" << endl;
				}

				else if (yourCards == 21)
				{
					bank += bet;
					cout << "\n" << playerName << " wins." << endl;
					cout << "\nCongratulations on black jack" << endl;
				}
				else if (scoreToBeat > 21)
				{
					bank += bet;
					cout << playerName << " Wins" << endl;
				}
				else if ((yourCards < scoreToBeat) && (scoreToBeat<21))
				{
					bank -= bet;
					cout << "\n" << playerName << " looses." << endl;
				}
				else 							//if ( youCards > scoreToBeat) 
				{
					bank += bet;
					cout << "\n" << playerName << " wins." << endl;
				}

				cout << playerName << " has run out of chips" << endl;;
				cout << "Play again? " << endl;
				cin >> playAgain;
			}
		}
	} while ((playAgain == "Yes") || (playAgain == "yes"));
	system("pause");
	return 0;
}

int dealerScore(int playerScore, string nameOfPlayer) //dealerScore function needs to know yourScore
{
	int dealerScore, card1, card2, hitMe;
	card1 = 1 + rand() % 12;
	card2 = 1 + rand() % 12;
	cout << "\nThe first card the dealer draws has a value of: " << card1 << endl;
	cout << "The second card the dealer draws has a value of: " << card2 << endl;
	if (playerScore>21)
	{
		cout << "Because the " << nameOfPlayer << " has busted the dealer automatically wins" << endl;
		cout << "No need for the dealer to bother with hitting/risking a bust." << endl;
		dealerScore = (card1 + card2);
		return dealerScore;

	}
	dealerScore = (card1 + card2);
	cout << "The dealers total is: " << dealerScore << endl;
	cout << "The dealer must hit at least a 17" << endl;
	cout << "Therefore the dealer will continue hitting until this has happened." << endl;
	while (dealerScore<17)
	{
		hitMe = 1 + rand() % 12;
		dealerScore += hitMe;
	}

	cout << "\nThe dealer has a score of : " << dealerScore << endl;
	return dealerScore;
}

string greeting()
{
	string playerName;
	cout << "\t\t\t Welcome To Our BlackJack Game\n"
		"\t\t\t Programmed By Group 2\n"
		"---------------------------------------------------------------------------------\n"
		"\t\t\t Here are the basics of Black Jack \n\n"
		"Each card has a value. The cards 2 to 10 hold the value of their number.\n"
		"The face cards: King, Queen, Jack each hold a value of 10.\n"
		"The four Aces hold a value of either 1 or 11.\n"
		"The goal of this game is to reach a card value total of 21.\n"
		"After you are dealt your first two cards, you must decide wether to Hit or Stay.\n"
		"Staying will keep you safe, and hitting will add another card.\n"
		"Because you are competing against the dealer you must beat their hand.\n"
		"\t\t\t\t BUT BEWARE!\n"
		"\t\tIf your total goes over 21, YOU LOSE!\n";

	cout << "Please enter the players name who will be gambling: ";
	cin >> playerName;

	return playerName;
}

int buyChips(string nameOfPlayer)
{
	string yourName = nameOfPlayer;
	int chips;
	cout << "Please enter how many $1 chips " << yourName << " would like to buy: ";
	cin >> chips;
	return chips;
}

int riskChips(string nameOfPlayer)
{
	int risk;
	cout << "How many chips would " << nameOfPlayer << " like to bet on this hand? " << endl;
	cin >> risk;
	return risk;
}

int yourScore(string nameOfPlayer)
{
	int card1, card2, hit, newTotal, total, temp;
	string hitOrStay;
	card1 = 1 + rand() % 12;
	card2 = 1 + rand() % 12;
	cout << "The first card " << nameOfPlayer << " draws has a value of " << card1 << endl;
	cout << "The second card " << nameOfPlayer << " draws has a value of " << card2 << endl;
	total = (card1 + card2);
	cout << nameOfPlayer << " total is: " << total << endl;
	if (total == 21)
	{
		cout << "Wow thats amazing! BlackJack!" << endl;
		return total;
	}
	do
	{
		cout << "Would " << nameOfPlayer << " like to hit or stay? ";
		cin >> hitOrStay;
		if ((hitOrStay == "Stay") || (hitOrStay == "stay"))
			return total;
		else
		{
			hit = rand() % 12;
			newTotal = total;
			temp = newTotal + hit;
			newTotal = temp;
			total = newTotal;

			cout << "Players new total is: " << newTotal << endl;
			if (newTotal == 21)
			{
				cout << "Congratulations" << nameOfPlayer << " on hitting 21" << endl;
				return newTotal;
			}
		}
	} while ((hitOrStay == "Hit") || (hitOrStay == "hit") && (total<21));
}
