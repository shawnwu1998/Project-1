import java.util.ArrayList; 
import java.util.Scanner;
import java.util.Random;

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

	public void Action(int column) {
		System.out.println("Player 1's turn");
		for(int bottomrow =row-1;bottomrow>0;bottomrow--){
			if(board[bottomrow][column]=='.') {
				board[bottomrow][column]='X';
				count++;
				break;
			}else if(board[0][column]!='.') {
				System.out.println("This column is full, try again");
				break;
			}if((board[bottomrow][column]=='X' || board[bottomrow][column]=='O') && board[bottomrow-1][column]=='.') {
				board[bottomrow-1][column]='X';
				count++;
				break;
			}

		}

	}
	public void ActionO(int column) {
		System.out.println("Player 2's turn");
		for(int bottomrow =row-1;bottomrow>0;bottomrow--){
			if(board[bottomrow][column]=='.') {
				board[bottomrow][column]='O';
				count++;
				break;
			}else if(board[0][column]!='.') {
				System.out.println("This column is full, try again");
				break;
			}else if((board[bottomrow][column]=='X' || board[bottomrow][column]=='O') && board[bottomrow-1][column]=='.') {
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

	public void checkFinish(int win) {
		
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

		//play Random on Tiny board
		if(boardSize==1 && opponent==1) {
			Connect4 game= new Connect4(3,3);
			game.printboard();
			while(true) {
				System.out.println("player 1's col");
				int y=scan.nextInt();
				game.Action(y);
				game.printboard();

				//because Player 1 makes the 9th step and ends the game
				if(count==9) {
					System.out.println("Over");
					break;
				}

				System.out.println("2's col");
				game.randomAction();
				game.printboard();
			}
		}

		if(boardSize==2) {
			Connect4 game= new Connect4(6,7);
			game.printboard();
			while(true) {
				System.out.println("player 1's col");
				int y=scan.nextInt();
				game.Action(y);
				game.printboard();
				System.out.println("2's col");
				int z=scan.nextInt();
				game.ActionO(z);
				game.printboard();

			}
		}



	}
}
