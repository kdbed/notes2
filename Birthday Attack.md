#cryptography #infosec 

## Birthday Attack

The birthday attack is named after the birthday paradox. The name is based on fact that in a room with 23 people or more, the odds are greater than 50% that two will share the same birthday. Many find this counterintuitive, and the birthday paradox illustrates why many people’s instinct on probability (and risk) is wrong. You are not trying to match a specific birthday (such as yours); you are trying to match any birthday.

If you are in a room full of 23 people, you have a 1 in 365 chance of sharing a birthday with each of the 22 other people in the room, for a total of 22/365 chances. If you fail to match, you leave the room and Joe has a 21/365 chance of sharing a birthday with the remaining people. If Joe fails to match, he leaves the room and Morgan has a 20/365 chance, and so on. If you add 22/365 + 21/365 + 20/365 + 19/365 … + 1/365, you pass 50% probability.

The birthday attack is used to create hash collisions. Just as matching your birthday is difficult, finding a specific input with a hash that collides with another input is difficult. However, just like matching any birthday is easier, finding any input that creates a colliding hash with any other input is easier due to the birthday attack.


[[securityplus]]