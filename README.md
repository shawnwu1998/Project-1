import java.util.ArrayList; import java.util.Random; import java.util.Scanner;

public class Connect4 {

	static char [][] board;
	static int row;
	static int col;
	static int count=0;
	static Random rand = new Random();


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

	public void printboard() {
		System.out.println("---------------------");
		for(int i=0; i<this.row;i++) {
			for(int j=0; j<this.col;j++) {
				System.out.print(board[i][j]+"  ");
			}
			System.out.println();
		}
		System.out.println("---------------------"); 
	}

	Scanner sc= new Scanner(System.in);

	public void Action(int column) {
		System.out.println("Player 1's board");
		for(int bottomrow =row-1;bottomrow>0;bottomrow--){
			if(board[bottomrow][column]=='.') {
				board[bottomrow][column]='X';
				count++;
				break;
			}else if(board[0][column]!='.') {
				System.out.println("This column is full, try again");
				System.out.println("Player 1 try again");
				int r=sc.nextInt();
				Action(r);
				break;
			}if((board[bottomrow][column]=='X' || board[bottomrow][column]=='O') && board[bottomrow-1][column]=='.') {
				board[bottomrow-1][column]='X';
				count++;
				break;
			}

		}

	}

	public void Action2(int column) {
		System.out.println("Player 2's board");
		for(int bottomrow =row-1;bottomrow>0;bottomrow--){
			if(board[bottomrow][column]=='.') {
				board[bottomrow][column]='O';
				count++;
				break;
			}else if(board[0][column]!='.') {
				System.out.println("This column is full, try again");
				System.out.println("Player 2 try again");
				int r=sc.nextInt();
				Action(r);
				break;
			}if((board[bottomrow][column]=='X' || board[bottomrow][column]=='O') && board[bottomrow-1][column]=='.') {
				board[bottomrow-1][column]='O';
				count++;
				break;
			}

		}

	}


	//random player
	public void randomAction() {

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

	public int checkFinish() {
		int AIs = 0;
		int Humans=0;
		for(int i=2; i>=0;i--) {
			for(int j=0; j<=2; j++) {
				if(board[i][j]=='.') {
					continue;
				}
				//check right
				if(j<=2) {
					for(int x=0; x<3;x++) {
						if(board[i][x]=='O') {
							AIs++;
						}else if(board[i][x]=='X') {
							Humans++;
						}else {
							break;
						}
					}
					if(AIs==3) {
						return 1;
					}else if(Humans==3) {
						return 2;
					}
					AIs=0; Humans=0;
				}


				//check up
				if(i>=2) {
					for(int x=0;x<3;x++) {
						if(board[x][j]=='O') {
							AIs++;
						}else if(board[x][j]=='X') {
							Humans++;
						}else {
							break;
						}
					}
					if(AIs==3) {
						return 1;
					}else if(Humans==3) {
						return 2;
					}
					AIs=0; Humans=0;
				}


				//	check diagonal right
				if(j<=2 && i>=2) {
					for(int x=0; x<1;x++) {
						if(board[i-x][j+x]=='O') {
							AIs++;
						}else if(board[i-x][j+x]=='X') {
							Humans++;
						}
					}
					if(AIs==3) {
						return 1;
					}else if(Humans==3) {
						return 2;
					}
					AIs=0; Humans=0;	
				}

				//check diagonal left
				if(j>=2 && i>=2) {
					for(int x=0; x<1;x++) {
						if(board[i-x][j-x]=='O') {
							AIs++;
						}else if(board[i-x][j-x]=='X') {
							Humans++;
						}
					}
					if(AIs==3) {
						return 1;
					}else if(Humans==3) {
						return 2;
					}
					AIs=0; Humans=0;
				}

			}
			for(int j=0;j<3;j++){
				if(board[0][j]=='.') {
					return -1;
				}
			}
		}

		return 0;


	}
	public int checkFinish2() {
		int AIs = 0;
		int Humans=0;
		for(int i=5; i>=0;i--) {
			for(int j=0; j<=6; j++) {
				if(board[i][j]=='.') {
					continue;
				}
				//check right 对了
				if(j<=3) {
					for(int x=0; x<4;x++) {
						if(board[i][x+j]=='O') {
							AIs++;
						}else if(board[i][x+j]=='X') {
							Humans++;
						}else {
							break;
						}
					}
//					System.out.println(i+","+j+"    O "+AIs+"     X "+Humans);
					if(AIs==4) {
						return 1;
					}else if(Humans==4) {
						return 2;
					}
					AIs=0; Humans=0;
				}


				//check up 对了
				if(i<=2) {
					for(int x=0;x<4;x++) {
						if(board[i+x][j]=='O') {
							AIs++;
						}else if(board[x+i][j]=='X') {
							Humans++;
						}else {
							break;
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
			//这一段我不确定要不要用
//			for(int j=0;j<7;j++){
//				if(board[0][j]=='.') {
//					return -1;
//				}
//			}
		}

		return 0;


	}



	public static void main(String[] args) {
		Scanner scan= new Scanner(System.in);
		System.out.println("Please choose your game");
		System.out.println("1. Tiny 3X3X3 Connect-Three");
		System.out.println("2. Standard 6X7X4 Connect-Four");
		System.out.println("Your choice?");
		int boardSize=scan.nextInt();

		System.out.println("Choose your opponent:");
		System.out.println("0.测试"+
				"1. An agent that plays randomly\n" + 
				"2. An agent that uses MINIMAX\n" + 
				"3. An agent that uses MINIMAX with alpha-beta pruning\n" + 
				"4. An agent that uses H-MINIMAX with a fixed depth cutoff");
		System.out.println("Your choice?");
		int opponent =scan.nextInt();

		//play Random on Tiny board
		if(boardSize==1 && opponent==1) {
			Connect4 game= new Connect4(3,3);
			game.printboard();
			while(true) {
				System.out.println("player 1's col");
				int y=scan.nextInt();
				game.Action(y);
				game.printboard();
				int r=game.checkFinish();
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
				//because Player 1 makes the 9th step and ends the game

				System.out.println("2's col");
				game.randomAction();
				game.printboard();
				int r1=game.checkFinish();
				if(r1==1) {
					System.out.println("Player 2 wins");
					break;
				}else if(r1==2) {
					System.out.println("Player 1 wins");
					break;
				}

			}

		}


//		if(boardSize==2) {
//			Connect4 game= new Connect4(6,7);
//			game.printboard();
//			while(true) {
//				System.out.println("player 1's col");
//				int y=scan.nextInt();
//				game.Action(y);
//				game.printboard();
//				if(count==42) {
//					System.out.println("Over");
//					break;
//				}
//
//				System.out.println("2's col");
//				game.randomAction();
//				game.printboard();
//				int r=game.checkFinish2();
//				if(r==1) {
//					System.out.println("Player 2 wins");
//					break;
//				}else if(r==2) {
//					System.out.println("Player 1 wins");
//					break;
//				}
//			}
//		}

		//play with person on board
		if(boardSize==2 && opponent==0) {
			Connect4 game= new Connect4(6,7);
			game.printboard();
			while(true) {
				System.out.println("player 1's col");
				int y=scan.nextInt();
				game.Action(y);
				game.printboard();
				int r=game.checkFinish2();
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
				int z=scan.nextInt();
				game.Action2(z);
				game.printboard();
				int r1=game.checkFinish2();
				if(r1==1) {
					System.out.println("Player 2 wins");
					break;
				}else if(r1==2) {
					System.out.println("Player 1 wins");
					break;
				}else if(count==42) {
					System.out.println("It's a Tie!");
					break;
				}
			}
		}
	}

}
