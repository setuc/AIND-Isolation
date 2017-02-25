#Paper: Deep Blue by IBM Watson team

The desire to build machines capable of beating the best humans at chess, remarkable discoveries and technological 
advances have paved the way to Deep Blue’s narrow victory over Garry Kasparov in 1997. This paper describes the factors 
that lead to the success.
 
 ##Hardware
Deep blue is a 30-node special purpose machine equipped with 480 special purpose chess chips, which are capable of searching 
anywhere between 100 to 200  million chess positions a second. This when compared to Mike Valvo and grandmaster Gary Kasparov
searched deeper than Deep Thought, the original chess machine.  Each chip has three functions:
1. Move generator, that first tries to capture pieces (probably becaue Gary Kasparov is an attacking player) and then 
identify non capture moves. 
2. a fast evaluation to quickly calculate the board score and a slow evaluation that tries to calculate values for special 
chess moves like X-rays etc. 
3. Search control that makes use of null-window alpha beta search. 
These chips also support an external Field Programmable Gate Array (FPGA) chip to help with transposition table (storing
a particular chess board position) and more complicated serach controls and evaulation fucntions. 


##Software
They developed a new selective search called *dual credit with delayed extensions*, probably at an attempt to mimick human
thinking. A credit is given for forced moves which is tracked for both players for a given move. A credit is built up as 
the search tree is traversed until a sufficient credit is made. The generation of credit itself is built on a set of 
 complicated rules, for example greate the number of moves a less credit is given and vice versa. Also the close the move
 to the root of the tree, the higher credit it got. 
  They also implemented an idea of pruning to their alpha-beta search called *No Progress*. They assume that is best to play
  a good move earlier in the game than later. 
  
##Opening and Endings
The opening book was created by Grandmaster Joel Benjamin which contained about 4000 positions of which a Deep Blue was
prepared with a specific repertoires for which it was trained. Besides the opening book, an extended book with details 
about 700,000 grandmaster matches was made available to help with specifi opening strategies. 
Often times in chess endgames, there are a very few specific pieces available on the board, there is very specific way to 
play those pieces so as not to loose or draw the matches. Deep Blue was also supplemented with two different databases that
was used for training as well as during the play. 


##Conclusions
The main AI lesson  learned from this event is that in some domains human creativity, intuition, and reasoning ability 
can be compensated for or even be surpassed by brute-force search requiring only simple evaluation functions. Unfortunately, 
this insight does not help much when it comes to solving much harder decision problems than chess for which full-width 
search is infeasible or simple heuristics do not work. The IBM team had to make decisions for which there was no prior. 