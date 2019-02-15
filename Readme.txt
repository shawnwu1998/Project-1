Colloborators: Xueting Wang, Yuhan Chen, Shengyang Wu

How to run the program:
  To run the program,the user need to copy and paste the Connect4.java to Eclipse and run it.
  To start our game, the user will be asked several choices.
  First, our program allows the user to choose the board, one is 3*3*3, and the other is 6*7*4;
  And then, the user can choose the opponent: An AI uses random choices; An AI uses MINIMAX; An AI uses MINIMAX with alpha-beta pruning; An AI uses H-MINIMAX.
  Next is to choose whether to play first or second.
  If the user chooses H-MINIMAX, he/she will be asked to select a cut-off depth.

How we built the program:
  We first built the board system on which the connect4 games can be played between two players.
  We then constructed RANDOM method which allows the computer to randomly drop a chess piece on the board. 
  Then we constructed MINIMAX, MINIMAX with alpha beta pruning and H-MINIMAX as the methods the computer uses to drop a piece.  
  For H-MINIMAX we add method called search to get the best choice. 
  Meanwhile, we also constructed a scoring system, which allows the computer to make the optimal move based on the score of each position on the board. 
  For MINIMAX and MINIMAX with alpha beta pruning, it takes significant amount of time for AI to find best moves for 6*7 board. Other than that, it plays well and quickly.
