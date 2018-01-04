---
title: "Programmeringsuppgift 6"
date: 2016-03-31
tags: ["c++", "programmeringsuppgifter", "160331"]
draft: false
---

Submit your solution as Assignment #6
The game Yahtzee is based on throwing five dice and trying to get specific combinations of these dice. In this assignment, you are going to implement a part of the scoring mechanism.

There are a lot of possible combinations that gives points in Yahtzee, but in this exercise we are going to support checking for a number of ones, number of twos and one pair (two dice with the same result) in a given set of dice.

Create an abstract base class ``Score_Variant`` having two direct subclasses ``Counted_Dice`` and ``Pair``. Also create two subclasses to ``Counted_Dice`` called ``Ones`` and ``Twos``.

All classes must have these two member functions:

- string name() - Gives the string ”Ones”, ”Twos” and ”Pair” for the classes Ones, Twos and Pair respectively.
- int score() - Takes a ``vector<int>`` and returns the highest possible score for this scoring variant. For Ones and Twos, this is the sum of all ones and twos, for Pair the result is the sum of the highest possible pair in the vector. See table 1 for examples.

Since the calculation in ``score`` for Ones and Twos will be very similar, this calculation should be done by ``Counted_Dice`` by adding a constructor taking an int (the die searched for) and storing this as a member. ``Counted_Dice`` will also have a member function ``get_number`` to get the value of this number (this may be useful in the final printout step).

No other member functions than those listed are allowed.

It must not be possible to copy or move any objects of the above class types.

Dice........Ones....Twos....Pair 

1,2,3,2,1...2.........4.........4 

3,3,5,5,5...0.........0.......10

*Table 1: Expected scores from the three variants for some given sets of dice.*

Also create a main function that test your implementation by creating a vector containing one of each of the concrete classes (Ones, Twos and Pair). Also create one vector of ints representing your dice. For each of the score variants, check if the dice gives any points and if so, print first the name of the score variant, then the score according to that variant and, if it is Ones or Twos, print the number of ones or twos found. An example is given in listing 1.

    Listing 1: Exmple output for the dice 3, 2, 2, 3, 5
    Twos: 4 (2) 
    Pair: 6
Note: Even if this assignment only require three concrete score variants to be implemented, your code must show some general solutions so that it can be easily expanded with more score variants. Note that all possible Conted_Dice should print the number of scoring dice in the set (not specific to Ones and Twos).







## Facit:

```
#include <iostream>
#include <vector>
#include <algorithm>
#include <functional>
#include <memory>
using namespace std;
class Score_Variant
{
public:
    virtual int score(vector<int> const &) const = 0;
    virtual string name() const = 0;
    virtual ~Score_Variant() = default;
protected:
    Score_Variant() = default;
private:
    Score_Variant(Score_Variant const &) = delete;
    Score_Variant & operator=(Score_Variant const &) = delete;
};
class Counted_Dice: public Score_Variant
{
public:
    int score(vector<int> const & dice) const override
    {
        int found {};
        for ( auto die : dice )
        {
            if ( die == value )
                ++found;
        }
        return value*found;
        // or value * count(begin(dice), end(dice), value);
    }
    int get_number() const { return value; }
protected:
    Counted_Dice(int lookfor): value{lookfor}
    {}
private:
    int value;
};

class Ones : public Counted_Dice
{
public:
    Ones(): Counted_Dice{1} {}
    string name() const override { return "Ones"; }
};

class Twos : public Counted_Dice
{
public:
    Twos(): Counted_Dice{2} {}
    string name() const override { return "Twos"; }
};

class Pair : public Score_Variant
{
public:
    int score(vector<int> const & dice) const override
    {
        auto d = dice;
        sort(begin(d), end(d), greater<int>{});
        auto it = adjacent_find(begin(d), end(d));
        if ( it != end(d) )
        {
            return (*it)*2;
        }
        return 0;
    }
    string name() const override
    {
        return "Pair";
    }
};

int main()
{
    // should really use some smart pointer, but fine without...
    const vector<Score_Variant*> score_variants {new Ones, new Twos, new Pair};
    vector<int> dice {1,4,1,5,4};
    for ( auto && variant : score_variants )
    {
        auto points = variant->score(dice);
        if ( points != 0 )
        {
            cout << variant->name() << ": " << points;
            if ( auto ptr = dynamic_cast<Counted_Dice*>(variant) )
            {
                cout << " (" << points / ptr->get_number() << ")";
            }
            cout << endl;
        }
    }
    // deallocate pointers
    for ( auto & variant : score_variants )
    {
        delete variant;
    }
}

```