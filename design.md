# Design of Deployments & Disasters

A SRE RPG.

## Design principles

Game design tries to achieve:

* Game should be scalable - making these exercises is expensive, making them repeatable amortizes the cost.
* Players should learn about cooperation - incident management involves many people (devs, ops, support, dedicated SRE, ...) that need to work together. As more people can coordinate their work, chances of quickly resolving the incident rise.
* Players should detect areas for personal improvement - this exercise cannot teach usage of tools like Grafana, Graylog, etc. However, it can show players which tools are available to them and how useful they can be.

In the rest of the document, whenever a game design decision is influenced by one of these principles, I'll add a `Scale`, `Coop` or `Improve` tag.

### Metrics

How to know if goals are achieved?

#### Scale

* A quick poll after the game to see, among other things, if players would "recommend the game to a friend".
* Playing a few games in front of audience can give us a hint into how well next games would be perceived. Note that this has a downside of _spoiling_ the game scenario for spectators.
* Schedule a re-run of a game for the next day to see if players indeed spread the word and interest rises. 
* Ask players for ideas for the next game scenarios, try to hash them out after the initial game. See how easy / hard it is to create new scenarios.
* Ask players if any of them would be interested in hosting a game.

#### Coop

* DM can observe player interactions during the game to see if their teamwork improves.
* Players' line managers could report on their improvement in teamwork as part of their performance review process.

#### Improve

* Endgame poll can include a question on newly discovered tools.
* DM can observe players and note instances of them discovering a tool they didn't already know about.
* Player mentors can work with them to incorporate improving in areas they detected as significant through the game. Mentors could later report on this.

## Game rules

Rules proposed here diverge from any real world tabletop RPG. The name association is only a marketing ploy (`Scale`). Game session consists of 3 main parts:

1. Each player is assigned a role (like a class) and is briefed on a single aspect of the project that will be hit with an incident. For example, roles can be frontend dev, API dev, backend dev, SRE, team lead, etc. Each role will have unique insight into the system, but nobody will know everything (`Coop`, `Improve`).
1. The incident starts, and along with it the clock that counts how long it takes the team to resolve it. This segment lasts until successful resolution, and is split into turns. Each turn adds 5 min to the clock. Players can actually discuss their actions for longer than that, turn length is fixed to make the game simpler and more clear to players (`Scale`, `Coop`).
1. After the incident is resolved, players plan for future improvements to the system that will prevent this kind of thing repeating. They discuss the session with DM and fill out the poll, so that we can keep on improving the game (`Scale`, `Improve`).

## Player roles

Each player is assigned a role. Depending on their role, each player is given information on a component in the architecture of the system. Components can be something like: web app, public API, load balancer, backend service, database, queue, cache, etc. In addition to component based roles there are 2 more roles:

* Dedicated SRE. They receive info on metrics, alerts and alike.
* Project lead role. They receive info on other systems that depend on their important clients and their use cases, etc.

The goal of this setup is not to reflect real life conditions. Ideally, in real life knowledge is shared among all team members. However, there are always gaps in knowledge of any real team member. Codifying those gaps in game rules insures that all players will experience them in the course of the game and will need to learn how to deal with them (`Improve`). Additionally, it will increase a co-dependency of players (`Coop`).

Materials that players receive should include enough info to enable them to understand the incident, but also to resolve it in various ways and to muddy the waters a little bit. Each incident should have a list of possible complications that can develop, and these materials should make those understandable as well. For each setback there needs to be at least one player that can understand and resolve it. That will keep the game feeling fair, and in turn entice more players to try it (`Scale`).

## Incident clock

Incident clock measures how many turns it took the team to resolve the incident. It represents one clear success metric for the players. In order to give players a sense of scale, the clock should denote various timestamps. For example, if they resolve the incident under 20 min that's cool, but if they go over 1 hour that constitutes a breach of contract. This mechanic gamifies the experience (`Scale`) and forces players to relate with client impact (`Coop`).

Additional clock related mechanic is _Client impact report_. If the team manages to produce precise enough report of the impact the incident is having on the clients within a predefined time (for example 10 min) other timestamps on the clock are pushed back, giving them more time to resolve the incident overall. This illustrates the importance of reporting the incident to clients (`Coop`) and reuses the one clear mechanical currency (time on the clock) to reward players.

## The incident

Incident is the main inciting event of the game. It can start either with players receiving an automated alert or a customer complaint. For each incident DM should prepare:

* A series of failure conditions that lead to the incident. Removing those is the ultimate goal for the players.
* A series of observable side effects. This includes things like logs, metrics, alerts, thread / heap dumps, DB states, etc. Players will combine these observations with their knowledge of the system (see  Player roles section) in order to detect the failure conditions.
* A series of complications that can arise from the incident. These can include things like specific high priority clients' use cases being affected, problem spreading to other systems through shared resources, running out of some resources like disc space overrun by logs or Graylog queueing logs, etc. These will happen as a gamefied consequence of players failing some action. (There can be other complications not related to the incident itself.) This will entice players to be more cautious, making the game more appealing (`Scope`).

## Game turn

The 2nd game stage (resolving the incident) is composed of a series of turns. Each turn progresses the incident clock by 5 minutes. This part of the game is the most codified with rules. In each turn players decide on the actions they'll take, throw a set of 5 dices, and depending on their roll DM reveals the effects of those actions. More formally, each turn has 2 steps:

1. Players agree among themselves on the actions they will perform. Each action will be performed by one of the players. No player can perform multiple actions per turn, so they can perform as many actions as there are players in the game. It's ok to take less actions, including none. (Incident clock will still progress.) Players have a shared pool of 5D6 dice that they can allocate to actions they picked. They can allocate any number of dice to any action, as long as they don't go over the limit of 5.
1. Dice are rolled for each action in succession, and DM decides on the success or failure of the action. If action fails, DM can either withhold some information that players were looking for or introduce a complication to the game. DM relates results of each action to the players. Note that in case a complication does occur, DM doesn't need to spell out its full scope, only the immediately visible side effects.

This system has a few notable properties:

* Players use a common pool of dice that they allocate to their actions. This will entice them to pick carefully and limit the number of actions they choose to perform, making the game more engaging (`Scale`).
* All players discuss actions together. In doing so they share information awarded to them based on their roles (`Coop`).
* Each player can suggest actions that others might not be aware existed, thus enabling discovery of new troubleshooting tools (`Improve`). 
* Since all actions are defined first and subsequently resolved the system simulates the real world situation in which incident responders assign special task to do / things to check, perform those and again coordinate to analyse results (`Coop`).

## Actions

There are 2 types of actions:

* Query actions - those have no side effects on the state of the system. Players aim to gain some insight into the system. If they fail DM, should withhold the information players seek.
* Command actions - those have side effects on the state of the system. Players attempt to fix things with these actions. If they fail, DM should introduce a complication.

For each action players can choose a tool they will use to perform the action. Each player has proficiency in certain tools. If a player performing an action has proficiency in the tool the action is using, they receive a `+2` to their dice roll. Available tools should reflect the real world setup, thus raising awareness among players (`Improve`).

Examples of tools for query actions:

* Graylog, Kibana for logs
* Grafana, Prometheus, NewRelic for metrics
* Java mission control for insight into JVM applications

Examples of tools for commands:

* Git, Jenkins, Artifactory for CI related stuff
* Docker, Kubernetes for deploy related operation
* Programming languages for implementation changes
* Redis, RabbitMQ, SQL for data management

Each list can include proprietary technologies used in real world scenarios. Listing tools will enable players to discover troubleshooting options they haven't previously considered (`Improve`).

Depending on your players interest, tool proficiency can be predefined in their profiles or individually defined by players themselves. This option could improve engagement (`Scale`).

DM should assign each action a difficulty. Players, while considering which actions to pick and how to allocate their shared dice, should check the difficulty. For assigning difficulty it should suffice to categorize actions into easy, medium and hard category which should, on average, be solvable with 1, 2 or 3 dice by a player with tool proficiency. More precisely:

|Action|Difficlty|
|------|---------|
|Easy  |5        |
|Medium|9        |
|Hard  |12       |

Dice roll combined with tool proficiency bonus (if exists) should be above the difficulty for action to succeed. DM can decide on the severity of complications for failure based on how bad the roll is.

This system is designed to be as easy as possible for players to understand and DMs to implement (`Scale`), while keeping the game interesting (`Scale`). On the other hand it raises awareness of the real world tools (`Improve`) and encourages cooperation among players (`Coop`).
