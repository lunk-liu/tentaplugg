---
title: "Programmeringsuppgift 9"
date: 2016-06-02
tags: ["c++", "programmeringsuppgifter", "160602"]
draft: true
---
Writeyourcodeinafilenamedassignment9.ccandsubmityoursolutionasAssignment#9. [5p]
A company creating saunas wants your help to create a polymorphic class hierarchy to handle their three basic types of sauna heaters; Steam room heaters, wood-burning heaters and the standard electrical heaters.
Create one abstract base class Sauna with three direct subclasses; Steam_Heater, Wood_Heater and Electrical_Heater. Sauna has the following properties:
• temperature, should be accessible and modifiable with public member functions.
• default temperature, the thermostat temperature setting. If unset, it shall be room temperature (22 degrees). Is set at construction and never changed.
• whether the heater is on or not. A newly created heater is always off. When a heater is turned on, the temperature is set to the default temperature. When the heater is turned off the temperature is reset to room temperature.
Sauna has a public function get_clone that calls a non-public pure virtual function clone that returns a new object of the dynamic type of the current object which get_clone then returns. This is the only way of publicly copying objects in the hierarchy. Also create a virtual function str that returns a string that names the current object.
• Steam_Heater has a default temperature of 38 degrees and has a str function returning the string ”Steam sauna”.
• Wood_Heater hasn’t got a default temperature and has a str function returning ”Wood-burning sauna”. Since wood-burning heaters don’t have a thermostat, an ex- tra member function add_log is added that increases the current temperature with 5 degrees. Physically there would be a maximum temperature but you can ignore this.
• Electrical_Heater has a default setting of 85 degrees and the str function return ”Electrical heater”.
Overload the formatted output operator for Sauna to print the following information:
• the type of the sauna • if the sauna is on:
– The string [ON] followed by the temperature • else: the string [OFF]
An example of above rules with a wood-burning sauna heater at 76 degrees:
   Wood-burning sauna [ON] 76 degrees
Create a program with a vector referring to one object of each concrete type. Pass a copy of each separate object to another function that does the following:
1. print the sauna to cout
2. turn it on
3. if it is a wood-burning heater, add 10 logs 4. print it again
Handle resources in a good way, your program should not have any memory leaks.