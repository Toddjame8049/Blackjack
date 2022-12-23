# Blackjack
blackjack app
using System;
using System.Collections.Generic;

namespace Blackjack
{
    class Card
    {
        public string Suit { get; set; }
        public string Value { get; set; }

        public Card(string suit, string value)
        {
            Suit = suit;
            Value = value;
        }

        public int GetValue()
        {
            if (Int32.TryParse(Value, out int val))
            {
                return val;
            }
            else if (Value == "A")
            {
                return 11;
            }
            else
            {
                return 10;
            }
        }

        public override string ToString()
        {
            return $"{Value} of {Suit}";
        }
    }

    class Deck
    {
        public List<Card> Cards { get; set; }

        public Deck()
        {
            Cards = new List<Card>();
            string[] suits = { "Hearts", "Spades", "Clubs", "Diamonds" };
            string[] values = { "2", "3", "4", "5", "6", "7", "8", "9", "10", "J", "Q", "K", "A" };
            foreach (string suit in suits)
            {
                foreach (string value in values)
                {
                    Cards.Add(new Card(suit, value));
                }
            }
        }

        public void Shuffle()
        {
            Random rng = new Random();
            for (int i = 0; i < Cards.Count; i++)
            {
                int j = rng.Next(i, Cards.Count);
                Card temp = Cards[i];
                Cards[i] = Cards[j];
                Cards[j] = temp;
            }
        }

        public Card Draw()
        {
            Card drawnCard = Cards[0];
            Cards.RemoveAt(0);
            return drawnCard;
        }
    }

    class Hand
    {
        public List<Card> Cards { get; set; }

        public Hand()
        {
            Cards = new List<Card>();
        }

        public int GetTotal()
        {
            int total = 0;
            int aces = 0;
            foreach (Card card in Cards)
            {
                if (card.Value == "A")
                {
                    aces++;
                }
                total += card.GetValue();
            }
            while (total > 21 && aces > 0)
            {
                total -= 10;
                aces--;
            }
            return total;
        }

        public void Add(Card card)
        {
            Cards.Add(card);
        }

        public void Clear()
        {
            Cards.Clear();
        }
    }

    class Player
    {
        public string Name { get; set; }
        public Hand Hand { get; set; }

        public Player(string name)
        {
            Name = name;
            Hand = new Hand();
        }

        public bool Hit(Deck deck)
       
