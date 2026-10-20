# Draw-It-or-Lose-It-design-document-1.2
[Software Design Document](CS 230 Project Software Design Morin 1.2.docx)

## Who was this software design document for?

The client for this design document was "The Gaming Room" with a game called Draw it or Lose it. Starting out, they only had the game on Android phones but they wanted to scale the game to be web based and reach many different platforms and wanted to cover all bases to make such a thing as smooth and optimised as possible.  Their requirements were that a game can have one or more teams, each team can have multiple players, game and team names would have to be unique so if a name was taken already it had to be locked out from selection, and only one instance of the game could exist in memory at one time.

## What I did well in this documentation:

If I had to pick, I would say tying every design decision back to a specific requirement was my strongest area.  The singleton used for this exists to satisfy the one-instance rule. Entity base class ensures every object has an ID and a name. Iterators were also used because of the unique name rule as well. I also am proud of the design constraints section where I found race conditions that would show up once the game was moved over to a multi threaded server. Two threads could pass the null check in getInstance(), or two threads both finding a name free in addGame(). Finally, in the Recommendations I connected OS level concepts like paging and swap directly to gameplay, because a page fault during a game with timed rounds would show up to players as visible stutters.

## How the Design document helped with developing the code:

like writing pseudo code, writing the domain model first meant the structure was already planned out before I even started coding. I started coding knowing that Game, Team and Player would all inherit from Entity, and the fields would be privatised with no setters. I also knew that addTeam() and addPlayer() would be the only way in ensuring a uniqueness test could not be bypassed. Reading the UML's 0...* relationships made the ownership between games, teams, and players clear. Thinking through how the design would behave under real traffic also helped to expose problems that would never have shown up from single threaded testing.

## If I could revise one part, what would it be?

I would maybe go back and sort out the System Architecture View which was left empty as it wasn't a requirement. A logical topology diagram would work great here showing the browser clients connecting over HTTPS, Linux application servers running in Docker, a shared database for game state. Instead of "Synchronisations will be needed" I could have also added concrete fixes such as synchronised singleton initialisation or making the name check and insert into a single atomic operation.

## How did I interpret the user's needs and why it matters:

Each client requirement is translated into a specific part of the design. Unique names become the iterator check, one instance in memory became the singleton, and timed round became latency, memory, and real time networking concerns. I also considered the players themselves. If round timers were not in sync with WebSockets, some players would get more time than others. This creates an unbalanced gameplay experience which could break the entire game. This is why the user's needs matter so much, sure the design can technically be correct but it can still fail if it doesn't work the way people using it need it to.

## How I approached designing software, and changes I'd make in the future:

This all started with requirements, I mapped them each to a design element, and then used object oriented principles like inheritance, encapsulation, polymorphism etc. along with design patterns like singleton and Iterator. For platform decisions, I compared every option on cost, time, and required expertise instead of being biased and going after what was familiar. In the future, I would keep that approach, but I would maybe consider scaling and concurrency from the very start instead of after the prototype works. I'd draw the UML and architecture diagrams early, ask the client clarifying questions before committing, and I would plan tests for concurrent access, not just sequential use.
