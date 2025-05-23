# Multiplayer Game Networking Architecting Networked Games by Joshua Glazier and Sanjay Madhav
Always write down the following: note, tip, warning. Optionally, I can write down the sidebar. You can find the companion website [here](https://github.com/MultiplayerBook/MultiplayerBook). 
## Overview of Networked Games
Multiplayer game architecture overview of Age of Empires, and Starsiege: Tribes. Also discussing the challenges of engineering a networked multiplayer game. 

Brief History of Computer Games
- Local Multiplayer Games: Were player on a single computer with split screen. Similar to single player games. Examples are Tennis for Two(1958) and Spacewar!(1962).

- Early Networked Multiplayer Games: SImilar to a Local Area network. Using a serial port(allowing a bit of data to be transmitted at a time). A daisy chain scheme meant more than 1 computer was connected at once

- Multi User Dungeon(MUD): multiple users connect to a virtual world at once. Some MUD  games were ran on a bulletin board system using modems

- Local Area Network(LAN) Games: several computers connected to each other over a relatively small area. LANs really took off with the proliferation of the ethernet. DOOM 1993 was the progenitor of modern networked games supporting up to 4 players in a single game session

- Online Games: players connect over some large network with geographically distant computers. In the late 1990s online games like id Software's Quake(1996) and Epic Game's Unreal (1998) took off. The major consideration when working with online games is **latency**, the amount of time it takes for data to travel over the network. Services like Xbox Live and PlayStation Network help with multiplayer scalability.

- Massively Multiplayer Online(MMO) Games: online multiplayer games are limited per session(4 to 32), however, in MMOs hundreds or thousands of players can join in a single game session. For example, Ultima Online(1997), EverQuest(1999), World of Warcraft(2004).

Architecting an MMO is a complex technical challenge, most of the techniques necessary to create an MMO are well beyond the scope of this book. The foundation for creating a smaller scale networked game  is more important to understand before creating an MMO.
## The Internet

## Berkeley Sockets

## Object Serialization

## Object Replication

## Network Topologies and Sample Games

## Latency, Jitter, and Reliability

## Improved Latency Handling

## Scalability

## Security

## Real World Engines

## Gamer Services

## CLoud Hosting Dedicated Servers
