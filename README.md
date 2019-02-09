import java.util.ArrayList; import java.util.Random; import java.util.Scanner;

public class Connect4 {

	static char [][] board;
	static int row;
	static int col;
	static int depth;
	static Random rand = new Random();
	static Scanner scan= new Scanner(System.in);

	public Connect4 (int rows, int cols) {
		this.row=rows;
		this.col=cols;
		this.board=new char[rows][cols];
		for(int i=0; i<rows; i++) {
			for(int j=0; j<cols; j++) {
				board[i][j]='.';
			}

		}
	}


	public static void printboard() {
		System.out.println("---------------------");
		for(int i=0; i<row;i++) {
			for(int j=0; j<col;j++) {
				System.out.print(board[i][j]+"  ");
			}
			System.out.println();
		}
		System.out.println("---------------------"); 
		if(isboard()) {
			System.out.println("0 |1 |2 ");
		}else {
			System.out.println("0 |1 |2 |3 |4 |5 |6 ");
		}
	}


	public static void Action(int column) {

		for(int bottomrow =row-1;bottomrow>0;bottomrow--){
			if(board[bottomrow][column]=='.') {
				board[bottomrow][column]='X';
				break;
			}else if(board[0][column]!='.') {
				System.out.println("This column is full, try again");
				System.out.println("Player 1 try again");
				int r=scan.nextInt();
				Action(r);
				break;
			}if((board[bottomrow][column]=='X' || board[bottomrow][column]=='O') && board[bottomrow-1][column]=='.') {
				board[bottomrow-1][column]='X';
				break;
			}

		}

	}


	public static void randomAction() {//random player

		//will return a number of 0, 1, 2
		//will represent Player 2, and use symbol "O"

		boolean cycle = true;

		while(cycle == true) {

			for(int bottomrow =row-1;bottomrow>0;bottomrow--){
				int randCol = rand.nextInt(col);
				if(board[0][randCol]!='.') {
					//do nothing
					//do another round
					break;
				}else if((board[bottomrow][randCol]=='X' || board[bottomrow][randCol]=='O') && board[bottomrow-1][randCol]=='.') {
					board[bottomrow-1][randCol]='O';
					cycle = false;
					break;
				}else if(board[bottomrow][randCol]=='.') {
					board[bottomrow][randCol]='O';
					cycle = false; 
					break;
				}
			}
		}


	}


	public static int Search(int b) {

		ArrayList<Integer> totalmoves= new ArrayList<Integer>();
		ArrayList<Integer> index = new ArrayList<Integer>();
		int best=Integer.MAX_VALUE;
		int move=-1;
		int num=-1;
		for(int i=0; i<board[0].length; i++) {
			if(rowAvailable(i)) {
				board=drop(i, 'O');
				if(b==1) {
					move=minimax(board, 'X',depth);
				}else if(b==2) {
					move=minimaxab(board, 'X', depth,Integer.MIN_VALUE, Integer.MAX_VALUE);
				}
				totalmoves.add(move);
				index.add(i);
				board=clear(i);
				if(move < best){
					best=move;
					num=i;
				}
			}
		}
		System.out.println();
		//for random if moves have many best value**
		int min = getMin(totalmoves);
		ArrayList<Integer> m = new ArrayList<Integer>();
		for(int i = 0 ; i < totalmoves.size() ; i++){
			if(totalmoves.get(i) == min){
				m.add(index.get(i));
			}
		}
		if(m.size() > 1){
			Random n = new Random();
			if(!rowAvailable(num)) {
				num = m.get(n.nextInt(m.size()));
			}
		}
		return num;


	}


	public static int getMin(ArrayList<Integer> moves){
		int min = Integer.MAX_VALUE;
		for(int i = 0; i < moves.size() ; i++){
			if(moves.get(i) < min){
				min = moves.get(i);
			}
		}
		return min;
	}


	public static boolean isboard() {
		if(row==3 & col==3) {
			return true;
		}else if(row==6 & col==7) {
			return false;
		}
		return false;
	}


	public static int minimax (char board[][], char player, int depth) {
		if(isboard()) {
			int r=checkFinish(1);
			if(r==10000) {
				return r-depth;
			}if(r==-10000) {
				return r+depth;
			}if(!spaceAvailable()) {
				return 0;
			}if(depth>8) {
				return r;
			}
		}
		if(!isboard()) {
			int r=checkFinish(2);
			if(r==10000) {
				return r-depth;
			}if(r==-10000) {
				return r+depth;
			}if(!spaceAvailable()) {
				return 0;
			}if(depth>8) {
				return r;
			}
		}

		if(player=='X') {
			int bestMax=Integer.MIN_VALUE;  
			for(int i=0; i<board[0].length; i++) {   
				if(rowAvailable(i)) {
					board=drop(i, 'X'); 
					//  printboard();
					bestMax = Math.max(minimax(board,'O',depth+1), bestMax); 
					// System.out.println("s"+bestMax);
					board = clear(i);
					// printboard();
				}
			}
			return bestMax;
		}else{
			int bestMIN=Integer.MAX_VALUE;
			for(int i=0; i<board[0].length; i++) {
				if(rowAvailable(i)) {
					board=drop(i,'O'); 
					//  printboard();
					bestMIN = Math.min(minimax(board,'X',depth+1), bestMIN);
					board = clear(i);
					// System.out.println("m"+bestMIN);
					//  printboard();
				}
			}
			return bestMIN;
		}
	}


	public static int minimaxab (char board[][], char player, int depth, int alpha, int beta) {

		if(isboard()) {
			int r=checkFinish(1);
			if(r==10000) {
				return r-depth;
			}if(r==-10000) {
				return r+depth;
			}if(!spaceAvailable()) {
				return 0;
			}if(depth>8) {
				return r;
			}
		}
		if(!isboard()) {
			int r=checkFinish(2);
			if(r==10000) {
				return r-depth;
			}if(r==-10000) {
				return r+depth;
			}if(!spaceAvailable()) {
				return 0;
			}if(depth>8) {
				return r;
			}
		}
		if(player=='X') {
			int bestMax=Integer.MIN_VALUE;  
			for(int i=0; i<board[0].length; i++) {   
				if(rowAvailable(i)) {
					board=drop(i, 'X'); 
					bestMax = Math.max(minimaxab(board,'O',depth+1, alpha, beta), bestMax); 
					board = clear(i);
					alpha = Math.max(alpha,bestMax);
					if(beta <= alpha)
						return bestMax;
				}
			}
			return bestMax;
		}else{
			int bestMIN=Integer.MAX_VALUE;
			for(int i=0; i<board[0].length; i++) {
				if(rowAvailable(i)) {
					board=drop(i,'O'); 
					bestMIN = Math.min(minimaxab(board,'X',depth+1,alpha, beta), bestMIN);
					board = clear(i);
					beta = Math.min(beta, bestMIN);
					if(beta <= alpha)
						return bestMIN;
				}
			}
			return bestMIN;
		}
	}


	public static boolean rowAvailable(int x) {
		for(int i=0; i<board.length; i++) {
			if(board[i][x]=='.') {
				return true;
			}
		}
		if(x<0 || x>= board[0].length) {
			return false;
		}
		return false;
	}


	public static char[][] drop(int column, char player) {
		for(int i = board.length - 1 ; i >= 0 ; i--){
			if(board[i][column] == '.'){
				board[i][column] = player;
				break;
			}
		}
		return board;
	}


	public static char[][] clear(int k){
		for(int i = 0 ; i < board.length ; i++){
			if(board[i][k] != '.'){
				board[i][k] = '.';
				break;
			}
		}
		return board;
	}


	public static int checkFinish(int b) {
		int AIs = 0;
		int Humans=0;
		int points=0;
		if(b==1) {
			for(int row=2; row>=0;row--) {
				for(int col=0; col<=2;col++) {
					if(board[row][col]=='.') {
						continue;
					}

					//check right
					if(col==0) {
						for(int x=0; x<3;x++) {
							if(board[row][x]=='O') {
								AIs++;
							}else if(board[row][x]=='X') {
								Humans++;
							}
						}
						switch(AIs) {
						case 1:points-=50;break;
						case 2:points-=100;break;
						case 3:return -10000;
						}
						switch(Humans) {
						case 1:points+=50;break;
						case 2:points+=100;break;
						case 3:return 10000;
						}
						AIs=0; Humans=0;
					}


					//check up
					if(row==0) {
						for(int x=0;x<3;x++) {
							if(board[x][col]=='O') {
								AIs++;
							}else if(board[x][col]=='X') {
								Humans++;
							}
						}

						switch(AIs) {
						case 1:points-=50;break;
						case 2:points-=100;break;
						case 3:return -10000;
						}
						switch(Humans) {
						case 1:points+=50;break;
						case 2:points+=100;break;
						case 3:return 10000;
						}

						AIs=0; Humans=0;
					}


					// check diagonal right

					if(col==0 && row==2) {
						for(int x=0; x<3;x++) {
							if(board[row-x][col+x]=='O') {
								AIs++;
							}else if(board[row-x][col+x]=='X') {
								Humans++;
							}
						}
					
						switch(AIs) {
						case 1:points-=50;break;
						case 2:points-=100;break;
						case 3:return -10000;
						}
						switch(Humans) {
						case 1:points+=50;break;
						case 2:points+=100;break;
						case 3:return 10000;
						}

						AIs=0; Humans=0; 
					}

					//check diagonal left
					if(col==2 && row==2) {
						for(int x=0; x<3;x++) {
							if(board[row-x][col-x]=='O') {
								AIs++;
							}else if(board[row-x][col-x]=='X') {
								Humans++;
							}
						}
						if(AIs==3) {
							return -10000;
						}else if(Humans==3) {
							return 10000;
						}
						
						switch(AIs) {
						case 1:points-=50;break;
						case 2:points-=100;break;
						case 3:return -10000;
						}
						switch(Humans) {
						case 1:points+=50;break;
						case 2:points+=100;break;
						case 3:return 10000;
						}
						AIs=0; Humans=0;
					}
				}
			}

			for(int j=0;j<3;j++){
				if(board[0][j]=='.') {
					return 0;
				}
			}
		}


		if(b==2) {
			for(int i=5; i>=0;i--) {
				for(int j=0; j<=6; j++) {
					if(board[i][j]=='.') {
						continue;
					}
					//check right 
					if(j<=3) {
						for(int x=0; x<4;x++) {
							if(board[i][x+j]=='O') {
								AIs++;
							}else if(board[i][x+j]=='X') {
								Humans++;
							}
						} 
						switch(AIs) {
						case 2:points-=50;break;
						case 3:points-=100;break;
						case 4:return -10000;
						}
						switch(Humans) {
						case 2:points+=50;break;
						case 3:points+=100;break;
						case 4:return 10000;
						}
						AIs=0; Humans=0;
					}

					//check up 
					if(i<=2) {
						for(int x=0;x<4;x++) {
							if(board[i+x][j]=='O') {
								AIs++;
							}else if(board[x+i][j]=='X') {
								Humans++;
							}
						}
						switch(AIs) {
						case 2:points-=50;break;
						case 3:points-=100;break;
						case 4:return -10000;
						}
						switch(Humans) {
						case 2:points+=50;break;
						case 3:points+=100;break;
						case 4:return 10000;
						}
						AIs=0; Humans=0;
					}


					// check diagonal right
					if(j<=3 && i>=4) {
						for(int x=0; x<4;x++) {
							if(board[i-x][j+x]=='O') {
								AIs++;
							}else if(board[i-x][j+x]=='X') {
								Humans++;
							}
						}
						switch(AIs) {
						case 2:points-=50;break;
						case 3:points-=100;break;
						case 4:return -10000;
						}
						switch(Humans) {
						case 2:points+=50;break;
						case 3:points+=100;break;
						case 4:return 10000;
						}
						AIs=0; Humans=0; 
					}

					//check diagonal left
					if(j>=3 && i>=4) {
						for(int x=0; x<4;x++) {
							if(board[i-x][j-x]=='O') {
								AIs++;
							}else if(board[i-x][j-x]=='X') {
								Humans++;
							}
						}
						switch(AIs) {
						case 2:points-=50;break;
						case 3:points-=100;break;
						case 4:return -10000;
						}
						switch(Humans) {
						case 2:points+=50;break;
						case 3:points+=100;break;
						case 4:return 10000;
						}
						
						AIs=0; Humans=0;
					}

				}

			}

			for(int j=0;j<7;j++){
				if(board[0][j]=='.') { 
					return 0; 
				}
			} 
		}
		return points;

	}


	public static boolean spaceAvailable() {
		for(int i=0; i<board.length; i++) {
			for(int j=0; j<board[i].length; j++) {
				if(board[i][j]=='.') {
					return true;
				}
			}
		}
		return false;
	}
	
	
	public static boolean utility() {//3*3*3

		int r=checkFinish(1);
		if(r==-10000) {
			System.out.println("AI wins");
			return true;
		}else if(r==10000) {
			System.out.println("Human wins");
			return true;
		}else if(!spaceAvailable()) {
			System.out.println("It's a Tie!");
			return true; 
		}
		return false;
	}


	public static boolean utility2() {//6*7*4

		int r=checkFinish(2);
		if(r==-10000) {
			System.out.println("AI wins");
			return true;
		}else if(r==10000) {
			System.out.println("Human wins");
			return true;
		}else if(!spaceAvailable()) {
			System.out.println("It's a Tie!");
			return true; 
		}
		return false;
	}


	public static void human() {
		System.out.println("Human's turn");
		int y=scan.nextInt();
		Action(y);
		printboard();
	}


	public static void AI(int k) {
		System.out.println("AI's turn");
		switch (k) {
		case 1:
			randomAction();
			printboard();
		//	System.out.println("AI's choice: "+randCol);
			break;
		case 2:
			int col=Search(1);
			board=drop(col,'O');
			printboard();
			System.out.println("AI's choice: "+ col);
			break;
		case 3:
			int coln=Search(2);
			board=drop(coln,'O');
			printboard();
			System.out.println("AI's choice: "+ coln);
			break;
		} 
	}


	public static void play(int b,int choice) {
		
		if(b==1) {
			switch(choice) {
			case 1:
				while(true) {
					human();
					if(utility()) break;
					AI(1);
					if(utility()) break;
				}
			break;
			case 2:
				while(true) {
					AI(1);
					if(utility()) break;
					human();
					if(utility()) break;
				}
			break;
			}
		}
		
		if(b==2) {
			switch(choice) {
			case 1:
				while(true) {
					human();
					if(utility2()) break;
					AI(1);
					if(utility2()) break;
				}
			break;
			case 2:
				while(true) {
					AI(1);
					if(utility2()) break;
					human();
					if(utility2()) break;
				}
			break;
			}	
		}
		
	}


	public static void playminimax(int b, int choice) {
		if(b==1) {
			switch(choice) {
			case 1:
				while(true) {
					human();
					if(utility()) break;
					AI(2);
					if(utility()) break;
				}
			break;
			case 2:
				while(true) {
					AI(2);
					if(utility()) break;
					human();
					if(utility()) break;
				}
			break;
			}	
		}
		
		if(b==2) {
			switch(choice) {
			case 1:
				while(true) {
					human();
					if(utility2()) break;
					AI(2);
					if(utility2()) break;
				}
			break;
			case 2:
				while(true) {
					AI(2);
					if(utility2()) break;
					human();
					if(utility2()) break;
				}
			break;
			}	
		}
	}


	public static void playminimaxab(int b,int choice) {
		if(b==1) {
			switch(choice) {
			case 1:
				while(true) {
					human();
					if(utility()) break;
					AI(3);
					if(utility()) break;
				}
			break;
			case 2:
				while(true) {
					AI(3);
					if(utility()) break;
					human();
					if(utility()) break;
				}
			break;
			}	
		}
		
		if(b==2) {
			switch(choice) {
			case 1:
				while(true) {
					human();
					if(utility2()) break;
					AI(3);
					if(utility2()) break;
				}
			break;
			case 2:
				while(true) {
					AI(3);
					if(utility2()) break;
					human();
					if(utility2()) break;
				}
			break;
			}	
		}
	}


	public static void start() {
		Scanner scan= new Scanner(System.in);
		System.out.println("Please choose your game");
		System.out.println("1. Tiny 3X3X3 Connect-Three");
		System.out.println("2. Standard 6X7X4 Connect-Four");
		System.out.println("Your choice?");
		int boardSize=scan.nextInt();
		System.out.println("Choose your opponent:");
		System.out.println(
				"1. An agent that plays randomly\n" + 
						"2. An agent that uses MINIMAX\n" + 
						"3. An agent that uses MINIMAX with alpha-beta pruning\n" + 
				"4. An agent that uses H-MINIMAX with a fixed depth cutoff");
		System.out.println("Your choice?");
		int opponent =scan.nextInt();
		System.out.println("Do you want to play first or second? 1--first, 2--second");
		int turn =scan.nextInt();
		System.out.println("Please choose your depth limit: 0--8 ");
		depth=scan.nextInt();
		if(boardSize==1) {
			Connect4 game= new Connect4(3,3);
			switch(opponent){
			case 1:
				game.printboard();
				game.play(1,turn);
				break;
			case 2:
				game.printboard();
				game.playminimax(1,turn);
				break;
			case 3:
				game.printboard();
				game.playminimaxab(1,turn);
				break;
			case 4:
				game.printboard();
				// game.playhminimax(1);
				break;
			}

		}
		if(boardSize==2) {
			Connect4 game= new Connect4(6,7);
			switch(opponent){
			case 1:
				game.printboard();
				game.play(2,turn);
				break;
			case 2:
				game.printboard();
				game.playminimax(2,turn);
				break;
			case 3:
				game.printboard();
				game.playminimaxab(2,turn);
				break;
			case 4:
				game.printboard();
				// game.playhminimax(2);
				break;
			}
		}
	}


	public static void main(String[] args) {
		start();
	}
}
