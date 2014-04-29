/////////////////////////////////////////////////////////////
//                   The Blackjack Game                    //
//                 Panneerselvam Valluvan                  //
//                  v.selvam.v@gmail.com                   // 
/////////////////////////////////////////////////////////////

#include <iostream>
#include <iomanip>
#include <cstdlib>
#include <string>
#include <ctime>
#include <limits>


using namespace std;

const unsigned int MAX_HAND_CARDS = 10;
const unsigned int MAX_DECK_CARDS = 52;



/////////////////////////////////////////////////////////////////////////////
//      Card Class which represents each card in the game.                //
////////////////////////////////////////////////////////////////////////////
class Card {

public:
    enum rank { ACE = 1, TWO, THREE, FOUR, FIVE, SIX, SEVEN, EIGHT, NINE, TEN,
                JACK, QUEEN, KING };
    // ASCII code of each card
    enum suit { HEARTS = 3, DIAMONDS = 4, CLUBS = 5, SPADES = 6 };

    friend ostream& operator<< (ostream& os, const Card aCard);
    Card(rank r = ACE, suit s = SPADES, bool cardfaceup = true);
    ~Card();
    unsigned int GetCardValue() const;
    void FlipCard();
    bool GetFace();

private:
    rank cardRank;
    suit cardSuit;
    bool cardIsFaceUp;
};

// Card class constructor
Card::Card(rank r, suit s, bool cardFaceUp):  cardRank(r), cardSuit(s), cardIsFaceUp(cardFaceUp)
{}

Card::~Card()
{}

// Returns the real value of card if greater than 10
unsigned int Card::GetCardValue() const
{
    int cardValue = 0;

    if (cardIsFaceUp)
    {
        cardValue = cardRank;

        if (cardValue > 10)
            cardValue = 10;
    }

    return cardValue;
}

// For hiding the the first card of dealer
void Card::FlipCard()
{
    cardIsFaceUp = !cardIsFaceUp;
}

// Check if the card is facing up
bool Card::GetFace()
{
    return cardIsFaceUp;
}


/////////////////////////////////////////////////////////////////////////////
// This is the Deck Class which represents a deck of cards.                //
// Each deck in the game can have up to 52 cards maximum.                  //
/////////////////////////////////////////////////////////////////////////////

class Deck {

public:
    Deck(unsigned int dPos = 0);
    ~Deck();
    void ClearDeck();
    void PopulateDeck();
    void ShuffleDeck();
    bool DeckIsEmpty();
    void AddCardToDeck(Card pCard);
    unsigned int CurrentPosition();
    Card GiveCardToPlayer();

private:
    unsigned int deckCurrentPos;
    Card deckCards[MAX_DECK_CARDS];
};

// Deck class constructor
Deck::Deck(unsigned int dPos) : deckCurrentPos(dPos)
{
    ClearDeck();
    PopulateDeck();
    ShuffleDeck();
}

// Empty destructor
Deck::~Deck()
{}

// Current position in the array
unsigned int Deck::CurrentPosition()
{
    return deckCurrentPos;
}

// Reset position of array
void Deck::ClearDeck()
{
    deckCurrentPos = 0;
}

// Create an array of Card objects
void Deck::PopulateDeck()
{
    for (int s = Card::HEARTS; s <= Card::SPADES; s++)
    {
        for (int r = Card::ACE; r <= Card::KING; r++)
        {
            AddCardToDeck(Card(static_cast<Card::rank>(r),
                               static_cast<Card::suit>(s)));
        }
    }
}

// Shuffle the array of cards
void Deck::ShuffleDeck()
{
    srand(static_cast<unsigned int>(time(0)));
    for (int i = 0; i < (MAX_DECK_CARDS - 1); i++)
    {
        int r = i + (rand() % (MAX_DECK_CARDS - i));
        Card tempCard = deckCards[i];
        deckCards[i] = deckCards[r];
        deckCards[r] = tempCard;
    }
}

// Check if we have an empty deck
bool Deck::DeckIsEmpty()
{
    return (deckCurrentPos <= 0);
}

// Helper function to PopulateDeck()
void Deck::AddCardToDeck(Card pCard)
{
    deckCards[deckCurrentPos++] = pCard;
}

// Start dealing cards from the last index
Card Deck::GiveCardToPlayer()
{
    if (!DeckIsEmpty())
        return deckCards[--deckCurrentPos];
    else
    {
        // If cards ran out, we simply
        // re-shuffle the deck
        deckCurrentPos = MAX_DECK_CARDS;
        ShuffleDeck();
        return deckCards[--deckCurrentPos];

    }

}


/////////////////////////////////////////////////////////////////////////////
// This is the Hand Class which represents a hand of cards.                //
// Each hand in the game can have up to 10 cards maximum.                  //
/////////////////////////////////////////////////////////////////////////////

class Hand {

public:
    Hand(unsigned int hPos = 0, unsigned int pPos = 0);
    ~Hand();
    void ClearHand();
    void FlipFirstCard();
    bool HandIsEmpty();
    bool HandIsFull();
    Card RetrieveCard();
    void AddCardToHand(Card handCard);
    unsigned int GetHandTotal() const;
    unsigned int GetHandLastPosition() const;

private:
    unsigned int handCurrentPos;
    unsigned int playerCardPos;
    Card handCards[MAX_HAND_CARDS];
};

// Hand class constructor
Hand::Hand(unsigned int hPos, unsigned int pPos) : handCurrentPos(hPos), playerCardPos(pPos)
{ }

// Empty Hand class destructor
Hand::~Hand()
{}

// Reset hand
void Hand::ClearHand()
{
    playerCardPos = handCurrentPos = 0;
}

// Flip dealer's first card
void Hand::FlipFirstCard()
{
    handCards[0].FlipCard();
}

// Check if we have an empty hand
bool Hand::HandIsEmpty()
{
    return (handCurrentPos < 0);
}

// Check if we have a full hand
bool Hand::HandIsFull()
{
    return (handCurrentPos >= MAX_HAND_CARDS);
}

// Retrieve a card from hand
Card Hand::RetrieveCard()
{
    return handCards[playerCardPos++];
}

// Add a card from deck to hand
void Hand::AddCardToHand(Card handCard)
{
    if (!HandIsFull())
    {
        handCards[handCurrentPos++] = handCard;
    }
}

// Get hand total value
unsigned int Hand::GetHandTotal() const
{
    if (handCards[0].GetCardValue() == 0)
        return 0;

    int total = 0;

    // Start from top most card
    for (int i = GetHandLastPosition(); i >= 0; i--)
    {
        total += handCards[i].GetCardValue();
    }

    // Check if we have an Ace
    bool cardHasAce = false;
    for (int i = GetHandLastPosition(); i >= 0; i--)
    {
        if (handCards[i].GetCardValue() == Card::ACE)
            cardHasAce = true;
    }

    // If we have an ace and our total is <= 11
    // then we make the ace equal to 11
    if (cardHasAce && total <= 11)
        total += 10;

    return total;
}

// Get the hand's last position
unsigned int Hand::GetHandLastPosition() const
{
    return handCurrentPos - 1;
}


/////////////////////////////////////////////////////////////////////////////
// This is the Player Class which represents a player of the game.         //
/////////////////////////////////////////////////////////////////////////////

class Player {

public:
    friend ostream& operator<< (ostream& os, const Player aPlayer);
    Player(const string playerName);
    Player();
    ~Player();
    void Clear();
    string GetPlayerName() const;
    void SetPlayerName(string name);
    bool IsEmpty();
    bool IsDoubleDown() const;
    bool IsHitting() const;
    void FlipDealerFirstCard();
    void ReceiveCard(Card playerCard);
    bool IsBusted() const;
    unsigned int GetCardTotal() const;
    unsigned int GetPosition() const;

private:
    string playerName;
    Card playerCard;
    Hand playerHand;
};

// Player class constructor
Player::Player(const string pName) : playerName(pName)
{}

// Set default name for player
Player::Player()
{
    playerName = "Dealer";
}

// Empty Player class destructor
Player::~Player()
{}

// Get player's name
// Can be used in multi-player game
string Player::GetPlayerName() const
{
    return playerName;
}

// Set player's name
void Player::SetPlayerName(string name)
{
    playerName = name;
}

// Check if player's hand has no card
bool Player::IsEmpty()
{
    return playerHand.HandIsEmpty();
}

// Reset player's hand
void Player::Clear()
{
    playerHand.ClearHand();
}

//Check if the palyer is  asking for Double Down.
//But the option has to be available for the player 
//when he/she is having first two cards only 

bool Player::IsDoubleDown() const
{
 	if (playerName != "Dealer" && playerHand.GetHandLastPosition() == 1 )
    {
	   	cout << endl << playerName << ", Do you want to Double Down : [Y]es   [N]o : ";
	   	char response;
	   	cin >> response;
   	   	return (response == 'y' || response == 'Y');
    }
	   
}

// Check if the player or dealer is still
// asking for additional cards
// In the dealer's case, if the card value
// is <= 16 then we continue giving additional
// cards

bool Player::IsHitting() const
{
   
    if (playerName != "Dealer")
    {
        cout << endl << playerName << ", [H]it  [S]tand  : ";
        char response;
        cin >> response;
        return (response == 'h' || response == 'H');
    }
    else
    {
        return (playerHand.GetHandTotal() <= 16);
    }
}

// Flip dealer's first card
void Player::FlipDealerFirstCard()
{
    if (!(playerHand.HandIsEmpty()))
        playerHand.FlipFirstCard();
}

// Add a new card to player's hand
void Player::ReceiveCard(Card playerCard)
{
    playerHand.AddCardToHand(playerCard);
}

// Check if player or dealer is busted
bool Player::IsBusted() const
{
    return (playerHand.GetHandTotal() > 21);
}

// Get hand total
unsigned int Player::GetCardTotal() const
{
    return playerHand.GetHandTotal();
}

// Get the player's hand's last position
unsigned int Player::GetPosition() const
{
    return playerHand.GetHandLastPosition();
}


/////////////////////////////////////////////////////////////////////////////
// This is the Game Class which represents the game itself.                //
/////////////////////////////////////////////////////////////////////////////

class Game {

public:
    Game(string s);
    ~Game();
    void Play();
    void ShowTable();
    void AnnounceWinner();
    void IfDeckIsEmpty();
    void ClearGame();
    void Bet();
    void WinPot();
    void LostPot();
    bool EnoughMoney();

private:
    string name;
    Deck *gameDeck;
    Player dealer;
    Player player1;
    int PotMoney;
    int BetMoney;

};

// Game class constructor
// string can be set up as an array of string
// for use with multiple players
Game::Game(string s) : name(s) , PotMoney(100), BetMoney(0)
{    
    gameDeck = new Deck(0);
    player1.SetPlayerName(name);
}

// Game destructor
Game::~Game()
{
    delete gameDeck;
}

void Game::Bet()
{
	cout<< player1.GetPlayerName() << ", How much you want to bet?"<< "    ";
 	cin>> BetMoney;
 	while (BetMoney < 1 || BetMoney > PotMoney )
	{
 		cout<< player1.GetPlayerName() << ", Invalid bet. Bet Again!"<<endl;
		cin>> BetMoney;
 	}
	
	cout<< player1.GetPlayerName() << ", Your bet for this hand is "<< BetMoney<<"\tPress Enter to continue the Hand...";
 	char key = std::cin.get();	
	std::cin.ignore();
}
void Game::WinPot()
{

 PotMoney+=BetMoney;
 cout<< player1.GetPlayerName() << ", Your Current Pot is worth $"<<PotMoney<<endl;

}

void Game::LostPot()
{
    PotMoney-= BetMoney;
    cout<< player1.GetPlayerName() << ", Your Current Pot is worth $"<<PotMoney<<endl;
}

bool Game::EnoughMoney()
{
	if(PotMoney == 0)
	{
		cout<< player1.GetPlayerName() <<", Sorry! You don't have enough Money in your Pot!\nExiting..."<<endl;
		return 0;
	}
	else
		return 1;
}
// The blackjack game logic
void Game::Play()
{
    // Always check if we have an empty deck
    if (gameDeck->DeckIsEmpty())
    {
        IfDeckIsEmpty();
    }
    else
    {
		ShowTable();	
		Bet();
	
        // Deal 2 cards initially
        for (int i = 0; i < 2; i++)
        {
            player1.ReceiveCard(gameDeck->GiveCardToPlayer());
            dealer.ReceiveCard(gameDeck->GiveCardToPlayer());
        }

        // Hide dealer's first card
        dealer.FlipDealerFirstCard();

        // Display everyone's cards
        ShowTable();
	
    }

    // Asking for Double Down
    if (!player1.IsBusted() && player1.IsDoubleDown() )
    {
        if (gameDeck->DeckIsEmpty())
        {
            IfDeckIsEmpty();
        }
        else
        {
    		if (BetMoney > (PotMoney/2))
			{	    	    
		  		cout << player1.GetPlayerName() <<", You don't have enough Pot Money to Double Down " <<endl;	
		  		Bet();
			}

			BetMoney=2*BetMoney;
			cout<< player1.GetPlayerName() <<", Your Bet Money is Doubled!! Now, Your bet for this hand is "<< BetMoney << "\tPress Enter to continue the Hand...";
			char key = std::cin.get();	
			std::cin.ignore();	
			player1.ReceiveCard(gameDeck->GiveCardToPlayer());
           	ShowTable();
	    
        }
    }

	// Asking for Hitting or Standing
    else{
    // Get more cards from deck
     	while (!player1.IsBusted() && player1.IsHitting())
     	{
        	if (gameDeck->DeckIsEmpty())
        	{
            	IfDeckIsEmpty();
        	}
        	else
        	{
            	player1.ReceiveCard(gameDeck->GiveCardToPlayer());
            	ShowTable();
        	}
     	}
   }
        // Show dealer's first card
        dealer.FlipDealerFirstCard();

    // Dealer's turn to hit
    while (!dealer.IsBusted() && dealer.IsHitting())
    {
        if (gameDeck->DeckIsEmpty())
        {
            IfDeckIsEmpty();
        }

        dealer.ReceiveCard(gameDeck->GiveCardToPlayer());
    }

    // Show the cards and announce the winner
    if (!gameDeck->DeckIsEmpty())
    {
        ShowTable();
        AnnounceWinner();
        ClearGame();
		BetMoney = 0;
	
    }
}

// Show the players and cards
void Game::ShowTable()
{
    cout << string(50, '\n');
    cout << "\tWelcome to BlackJack Game!" <<"\tPotMoney: "<< PotMoney <<"\tBetMoney: "<< BetMoney<< endl << endl;
    cout << left << setw(10) << dealer.GetPlayerName() << dealer << endl;
    cout << left << setw(10) << player1.GetPlayerName() << player1 << endl;
}

// Who won the game?
void Game::AnnounceWinner()
{
    cout << endl;
    if (player1.GetCardTotal() == 21)
        {cout << player1.GetPlayerName() << " hit a BlackJack, " << player1.GetPlayerName() << " Wins!" << endl; WinPot(); }
    else if (dealer.GetCardTotal() == 21)
        {cout << dealer.GetPlayerName() << " hit a BlackJack, " << player1.GetPlayerName() << " lose." << endl; LostPot(); }
    
    else if (player1.GetCardTotal() > 21)
        {cout << player1.GetPlayerName() << " busts, Dealer Wins." << endl; LostPot(); }
    else if (dealer.GetCardTotal() > 21)
        {cout << dealer.GetPlayerName() << " busts, " << player1.GetPlayerName() << " Wins!" << endl; WinPot(); }
   
    else if (player1.GetCardTotal() > dealer.GetCardTotal())
        {cout << player1.GetPlayerName() << " Wins!" << endl; WinPot(); }
    else if (dealer.GetCardTotal() > player1.GetCardTotal())
        {cout << dealer.GetPlayerName() << " Wins!" << endl; LostPot(); }
    else
        cout << "It's a push!" << endl;
}

//  if we have an empty deck
// Just create a new deck object
void Game::IfDeckIsEmpty()
{
    delete gameDeck;
    gameDeck = new Deck(0);
}

// Reset the game
void Game::ClearGame()
{
    dealer.Clear();
    player1.Clear();
}

/////////////////////////////////////////////////////////////////////////////
//               Client Program                                            //
/////////////////////////////////////////////////////////////////////////////

// Set the function prototype for the overloaded << operator
ostream& operator<< (ostream& os, const Card aCard);
ostream& operator<< (ostream& os, const Player aPlayer);

int main()
{
    string name;
    char ans = 'y';
    cout << "Enter your name: ";
    cin >> name;

    Game *aGame = new Game(name);
    
    while (ans == 'y' || ans == 'Y')
    {
        aGame->Play();
	if (aGame->EnoughMoney() == 0)
	{
		ans = 'n';
	}
	else
	{
        cout << "\nDo you want to play Another Hand? (y/n): ";
        cin >> ans;
	}
    }

    delete aGame;

    return 0;
}

// Overloaded << operator for use with the Card object
ostream& operator<< (ostream& os, const Card aCard)
{
    const string RANKS[] = {"0", "A", "2", "3", "4", "5", "6", "7", "8", "9", "10",
                            "J", "Q", "K"};

    if (aCard.cardIsFaceUp)
    {
        os << RANKS[aCard.cardRank] <<  (char) aCard.cardSuit << "\t";
    }
    else
    {
        // Hide card
        os << (char)219  << "\t";
    }

    return os;
}

// Overloaded << operator for use with the Player object
ostream& operator<< (ostream& os, Player aPlayer)
{
    if (!aPlayer.IsEmpty())
    {
        for (int i = aPlayer.playerHand.GetHandLastPosition(); i >= 0; i--)
            os << aPlayer.playerHand.RetrieveCard();

        if (aPlayer.GetCardTotal() != 0)
            cout << "[" << aPlayer.GetCardTotal() << "]";
    }
    else
    {
        os << "Error: Empty Hand";
    }

    return os;
}
