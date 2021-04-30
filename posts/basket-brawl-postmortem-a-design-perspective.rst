.. title: Basket Brawl postmortem - A design perspective
.. slug: basket-brawl-postmortem-a-design-perspective
.. date: 2016-04-09 04:14:57 UTC+08:00
.. tags: 
.. category: Postmortem
.. link: 
.. description: 
.. type: text


Making a game fun is no easy feat and sometimes we stumble upon what seems like a normal idea and it somehow becomes super fun. We are a great example of that. This will be how BasketBrawl becomes that game.

BasketBrawl started off as a simple Unity prototype where there are 2 players and which players are allowed to snatch and shoot a basketball into the hoops. That was the basic idea of the initial game. We play test the prototype and we found out that it was relatively fun because there is a certain level of competitiveness in the game because players constantly want to take control of the ball and try to score. In the initial prototype, players snatch by touching another player and when they shoot, the characters will freeze in air and they can aim. I did that because I realized that it would be fun if players can aim properly and that will make the game easier.

As we develop the game we did a few changes to the mechanics of the initial prototype, we made players drop slowly instead of hanging in air when they shoot and we implement a snatching key so that player will have to "snatch" instead of the one in the Unity prototype where player lose the ball automatically when they walk past another player. This created a sense of playing real life basketball when players can choose to snatch as and when they want, additionally we disabled the snatching for the player with the ball because it doesn't make sense for player with the ball to snatch.

Another thing that we add was a simple shooting indicator where player can see roughly the trail of where their shot is going to end up at and that greatly improves the scoring of both players. We also made platforms climbable and this allows for a faster and tighter game play. We added the platforms because in our Unity prototype, the levels was only occupying the bottom half of the screen and players have trouble catching up to the other player, therefore we added that so that player can move to the top of the screen faster.

In the original Unity prototype, the hoops were preset to a few locations and will be randomly switched to the next hoop when player score in the hoop thus discouraging players from camping at the same area and scoring multiple shots. In addition to that, we made the ball spawned at random location instead of a fixed location whenever the game starts so that the seasoned player cannot remember where the ball will spawn. I think that this change is a very important thing for the game because this makes the game a different experience every time you play because the element of randomness makes the game a little unpredictable and every game feels like a unique match.

Our game have 8 different maps, each having a different theme and property, we have 2 icy, volcano, windy and underwater level, each having different properties like icy level being more slippery and windy level having constant wind blowing the ball and character. This clever level properties combined with the clever use of random in the game can allow things like lucky shots to happen for example in the windy level, the hoops are placed in a planned position so that sometimes scoring one shot will lead to subsequent lucky shots due to the wind.

In the game, the element of random, competitiveness makes the game addictive and combined with the gentle learning curve, makes the player hard not to like the game. In this aspect I think that the game is doing really well and this is where it shine.
We also introduced slight rubber banding because we realized that some players are not that good when playing against strong players so we introduced a catching up system where the lower points player can score higher when his score is lower during the last minute of the game.

However every game is not without it's flaws and there are a few design flaws that I will share when developing the game. The game's strong suit is that it offers players some degree of freedom when it comes to choosing the characters to play, but this in turn makes the balancing of the game very hard and till now, I can't say for sure that the characters are balanced. Along with the characters that players choose, the levels are equally important in helping the characters shine however when we design the levels, we did not adopt proper metric system to measure the area between platforms and this made the game fairly awkward in some places.

Another matter that I feel that we could handle it better was the rubber banding, when we introduced the rubber banding, the system wasn't as good because though it allows for the lower points player to catch up, the higher points player can hog the ball during the last minute of the game and this just broke our system.

These are my only dissatisfaction for the game because I really hope to make a fun and balanced game but despite all these flaws, the game is very enjoyable.

P.S. I have included a link_ to the original Unity Prototype.

.. _link: https://googledrive.com/host/0BxvxphVst5bxUHV0Rm41Q05HdjA