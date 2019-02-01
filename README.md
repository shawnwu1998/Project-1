# Project-1
Project-1 242
import java.util.ArrayList;
import java.util.Scanner;

public class Connect4 {

	static char [][] board;
	static int row;
	static int col;
	static int count=0;
 
 
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
			}else if((board[bottomrow][column]=='X' || board[bottomrow][column]=='O') && board[bottomrow-1][column]=='.') {
				board[bottomrow-1][column]='X';
				count++;
				break;
			}
	//	 count++;
		 if(count==this.row) {
			 System.out.println("The board is full");
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
		 }else if((board[bottomrow][column]=='X' || board[bottomrow][column]=='O') && board[bottomrow-1][column]=='.') {
			 board[bottomrow-1][column]='O';
			 count++;
			 break;
		 }
//		 count++;
		 if(count==this.row) {
			 System.out.println("The board is full");
			 break;
		 }
		 
	 }
  
 }
 
// public ArrayList<Integer> Actions(){
//  ArrayList<Integer> action= new ArrayList<Integer>();
//  for(int j=0; j<this.col;j++) {
//   if(this.board[0][j]=='.') {
//    action.add(j);
//   }
//  }
//  
//  return action;
//      
// }
 public static void main(String[] args) {
  Scanner scan= new Scanner(System.in);
  System.out.println("Please choose your game");
  System.out.println("1. Tiny 3X3X3 Connect-Three");
  System.out.println("2. Standard 6X7X4 Connect-Four");
  int x=scan.nextInt();
//  System.out.println("colomn");

  
  if(x==1) {
   Connect4 game= new Connect4(3,3);
   game.printboard();
   while(true) {
   System.out.println("player 1's col");
   int y=scan.nextInt();
   game.Action(y);
   System.out.println(count);
   if(count>8) {
	   System.out.println("Over");
	   break;
   }
   game.printboard();
   System.out.println("2's col");
   int z=scan.nextInt();
   game.ActionO(z);
   game.printboard();
   if(count>9) {
	   System.out.println("Over");
	   break;
   }
   
   }
   //game.printboard();
   
  // game.Action(y);
  }
  if(x==2) {
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
