import java.util.ArrayList; import java.util.Random; import java.util.Scanner;

public class Connect4 {

	static char [][] board;
	static int row;
	static int col;
	static int count=0;
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
	}


	public static void Action(int column) {
		System.out.println("Player 1's board");
		for(int bottomrow =row-1;bottomrow>0;bottomrow--){
			if(board[bottomrow][column]=='.') {
				board[bottomrow][column]='X';
				count++;
				break;
			}else if(board[0][column]!='.') {
				System.out.println("This column is full, try again");
				System.out.println("Player 1 try again");
				int r=scan.nextInt();
				Action(r);
				break;
			}if((board[bottomrow][column]=='X' || board[bottomrow][column]=='O') && board[bottomrow-1][column]=='.') {
				board[bottomrow-1][column]='X';
				count++;
				break;
			}

		}

	}

	//random player
	public static void randomAction() {

		//will return a number of 0, 1, 2
		//will represent Player 2, and use symbol "O"

		boolean cycle = true;

		while(cycle == true) {

			int randCol = rand.nextInt(col);

			for(int bottomrow =row-1;bottomrow>0;bottomrow--){

				if(board[0][randCol]!='.') {
					//do nothing
					//do another round
					break;
				}else if((board[bottomrow][randCol]=='X' || board[bottomrow][randCol]=='O') && board[bottomrow-1][randCol]=='.') {
					board[bottomrow-1][randCol]='O';
					count++;
					cycle = false;
					break;
				}else if(board[bottomrow][randCol]=='.') {
					board[bottomrow][randCol]='O';
					count++;
					cycle = false; 
					break;
				}
			}
		}


	}

	public static int minimaxab (char board[][], int player) {
		//System.out.println("yes");
		if(checkFinish()==1) {
			return 1;
		}
		if(checkFinish()==-1) {
			return -1;
		}
		if(count==9) {
			return 0;
		}

		if(player==1) {
			//System.out.println("yes1");
			int bestMax=Integer.MIN_VALUE;
			//store all actions 
			for(int i=0; i<=2; i++) {
				//System.out.println("yes2");
				if(isRowAvailable(board, i)) {
					//System.out.println("yes3");
					board=drop(i, 'X');
					//printboard();
					//System.out.println("board0  "+"bestMax  "+bestMax+"  i "+i);
					bestMax = Math.max(minimaxab(board,-1), bestMax);
					board = removeCoin(board, i);
				}
			}
			return bestMax;
		}else{
			//System.out.println("yes11");
			int bestMIN=Integer.MAX_VALUE;
			for(int i=0; i<=2; i++) {
				//System.out.println("yes22");
				if(isRowAvailable(board, i)) {
					//System.out.println("yes33");
					board=drop(i,'O');
					//printboard();
					//System.out.println("board1  "+"bestMIN  "+bestMIN+"  i "+i);
					bestMIN = Math.min(minimaxab(board,1), bestMIN);
					board = removeCoin(board, i);
				}
			}
			return bestMIN;
		}
	}

	public static boolean isRowAvailable(char board[][],int k){
		if(k < 0 || k >= board[0].length)
			return false;

		for(int i = 0 ; i < board.length ; i ++){
			if(board[i][k] == '.')
				return true;

		}
		return false;
	}

	public static char[][] drop(int column, char player) {

		for(int i = board.length - 1 ; i >= 0 ; i--){
			//System.out.println(i);
			if(board[i][column] == '.'){
				board[i][column] = player;
				break;
			}
		}
		return board;

	}

	public static void action1(int column) {
		for(int i=row-1; i>0;i--) {
			if(board[i][column]=='.') {
				board[i][column]='O';
				break;
			}else if((board[i][column]=='X' || board[i][column]=='O') && board[i-1][column]=='.') {
				board[i-1][column]='O';
				break;
			}
		}
	}

	public static char[][] removeCoin(char board[][],int k){
		for(int i = 0 ; i < board.length ; i++){
			if(board[i][k] != '.'){
				board[i][k] = '.';
				break;
			}
		}
		return board;
	}

	public static int checkFinish() {
		int AIs = 0;
		int Humans=0;

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

					if(AIs==3) {
						return -1;
					}else if(Humans==3) {
						return 1;
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

					if(AIs==3) {
						return -1;
					}else if(Humans==3) {
						return 1;
					}
					AIs=0; Humans=0;
				}


				//	check diagonal right

				if(col==0 && row==2) {
					for(int x=0; x<3;x++) {
						if(board[row-x][col+x]=='O') {
							AIs++;
						}else if(board[row-x][col+x]=='X') {
							Humans++;
						}
					}
					if(AIs==3) {
						return -1;
					}else if(Humans==3) {
						return 1;
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
						return -1;
					}else if(Humans==3) {
						return 1;
					}
					AIs=0; Humans=0;
				}

			}

		}

		for(int j=0;j<3;j++){
			//Game has not ended yet
			if(board[0][j]=='.') {
				return 0;
			}
		}

		return 0;

	}


	public static int checkFinish2() {
		int AIs = 0;
		int Humans=0;
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
					if(AIs==4) {
						return 1;
					}else if(Humans==4) {
						return 2;
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
					if(AIs==4) {
						return 1;
					}else if(Humans==4) {
						return 2;
					}
					AIs=0; Humans=0;
				}


				//	check diagonal right
				if(j<=3 && i>=4) {
					for(int x=0; x<4;x++) {
						if(board[i-x][j+x]=='O') {
							AIs++;
						}else if(board[i-x][j+x]=='X') {
							Humans++;
						}
					}
					if(AIs==4) {
						return 1;
					}else if(Humans==4) {
						return 2;
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
					if(AIs==4) {
						return 1;
					}else if(Humans==4) {
						return 2;
					}
					AIs=0; Humans=0;
				}

			}

		}

		for(int j=0;j<7;j++){
			if(board[0][j]=='.') { 
				return -1; 
			}
		} 

		return 0;

	}


	public static void play() {

		while(true) {
			System.out.println("player 1's col");
			int y=scan.nextInt();
			Action(y);
			printboard();
			checkFinish();
			int r=checkFinish();
			if(r==1) {
				System.out.println("Player 2 wins");
				break;
			}else if(r==2) {
				System.out.println("Player 1 wins");
				break;
			}else if(count==9) {
				System.out.println("It's a Tie!");
				break;
			}
			System.out.println("2's col");

			randomAction();
			printboard();
			int r2=checkFinish();
			if(r2==1) {
				System.out.println("Player 2 wins");
				break;
			}else if(r2==2) {
				System.out.println("Player 1 wins");
				break;
			}else if(count==9) {
				System.out.println("It's a Tie!");
				break;
			}
		}
	}


	public static void play2() {

		while(true) {
			System.out.println("player 1's col");
			int y=scan.nextInt();
			Action(y);
			printboard();
			checkFinish();
			int r=checkFinish2();
			if(r==1) {
				System.out.println("Player 2 wins");
				break;
			}else if(r==2) {
				System.out.println("Player 1 wins");
				break;
			}else if(count==42) {
				System.out.println("It's a Tie!");
				break;
			}
			System.out.println("2's col");
			randomAction();
			printboard();
			int r2=checkFinish2();
			if(r2==1) {
				System.out.println("Player 2 wins");
				break;
			}else if(r2==2) {
				System.out.println("Player 1 wins");
				break;
			}else if(count==42) {
				System.out.println("It's a Tie!");
				break;
			}
		}
	}


	public static void play3() {

		while(true) {
			System.out.println("player 1's col");
			int y=scan.nextInt();
			Action(y);
			printboard();
			checkFinish();
			int r=checkFinish();
			if(r==1) {
				System.out.println("Player 2 wins");
				break;
			}else if(r==2) {
				System.out.println("Player 1 wins");
				break;
			}else if(count==9) {
				System.out.println("It's a Tie!");
				break;
			}
			System.out.println("2's col");
			//这边我随便写的，不能这么写
			action1(minimaxab(board, 1));
			printboard();
			int r2=checkFinish();
			if(r2==1) {
				System.out.println("Player 2 wins");
				break;
			}else if(r2==2) {
				System.out.println("Player 1 wins");
				break;
			}else if(count==9) {
				System.out.println("It's a Tie!");
				break;
			}
		}
	}

	public static void main(String[] args) {
		Scanner scan= new Scanner(System.in);
		System.out.println("Please choose your game");
		System.out.println("1. Tiny 3X3X3 Connect-Three");
		System.out.println("2. Standard 6X7X4 Connect-Four");
		System.out.println("Your choice?");
		int boardSize=scan.nextInt();

		System.out.println("Choose your opponent:");
		System.out.println("1. An agent that plays randomly\n" + 
				"2. An agent that uses MINIMAX\n" + 
				"3. An agent that uses MINIMAX with alpha-beta pruning\n" + 
				"4. An agent that uses H-MINIMAX with a fixed depth cutoff");
		System.out.println("Your choice?");
		int opponent =scan.nextInt();

		//play Random on board 3*3
		if(boardSize==1 && opponent==1) {
			Connect4 game= new Connect4(3,3);
			game.printboard();
			game.play();
		}

		//play  minimax on board 3*3
		if(boardSize==1 && opponent==2) {
			Connect4 game= new Connect4(3,3);
			game.printboard();
			game.play3();
		}

		//play Random on board 6*7
		if(boardSize==2 && opponent==1) {
			Connect4 game= new Connect4(6,7);
			game.printboard();
			game.play2();
		}

	}
}
