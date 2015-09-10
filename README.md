# Text-based-Minesweeper
//Recreation of the classic minesweeper game with text-based visuals rather than a developed GUI
import java.util.Scanner;

public class Minesweeper
{
	static Scanner keyboard = new Scanner(System.in);

	public static void main(String[] args)
	{
		int size = welcomeAndAskForSizeOfField();
		int[][] minesfield = new int[size][size];
		char[][] outputField = new char[size][size];

		init(minesfield, outputField);
		//displayBombs(minesfield);
		System.out.println();
		displayOutput(outputField);
		System.out.println();

		boolean gameOver = false;
		while (gameOver != true)
		{
			gameOver = playGame(minesfield, outputField);
		}
		displayBombs(minesfield);
	}
	public static int welcomeAndAskForSizeOfField()
	{
		System.out.print("Welcome to Minesweeper!\n"
						+ "\nChoose size of grid (Press 1 for 5x5, Press 2 for 10x10): ");

		if (keyboard.nextInt() == 1)
			return 5;
		else
			return 10;
	}
	public static void init(int[][] mines, char[][] output)
	{
		for (int r = 0; r < output.length; r++)
		{
			for (int c = 0; c < output[0].length; c++)
				output[r][c] = ' ';
		}
		for (int r = 0; r < mines.length; r++)
		{
			for (int c = 0; c < mines[0].length; c++)
				mines[r][c] = (int) (Math.random() * 2);
		}
	}
	public static void displayBombs(int[][] mines)
	{
		for (int r = 0; r < mines.length; r++)
		{
			for (int c = 0; c < mines[0].length; c++)
			{
				if(mines[r][c] == 1)
					System.out.print("[X]");
				else if(mines[r][c] == -1)
					System.out.print("[" + (countMines(mines, r, c) + "").charAt(0) + "]");
				else
					System.out.print("[ ]");
			}
			System.out.println();
		}
	}
	public static void displayOutput(char[][] output)
	{
		for (int r = 0; r < output.length; r++)
		{
			for (int c = 0; c < output[0].length; c++)
				System.out.print("[" + (char) output[r][c] + "]");
			System.out.println();
		}
	}
	public static int countMines(int[][] mines, int r, int c)
	{
		int rMin = r - 1;
		int rMax = r + 1;
		int cMin = c - 1;
		int cMax = c + 1;

		if (r == 0)
			rMin = 0;
		if (r == mines.length - 1)
			rMax = mines.length - 1;
		if (c == 0)
			cMin = 0;
		if (c == mines.length - 1)
			cMax = mines.length - 1;

		int count = 0;
		for (int i = rMin; i <= rMax; i++)
			for (int j = cMin; j <= cMax; j++)
				if (mines[i][j] == 1 && (i != r || j != c))
					count++;
		return count;
	}
	public static boolean playGame(int[][] mines, char[][] outputBoard)
	{
		System.out.print("Enter row and column of the cell you want to open[row col]: ");
		int r = keyboard.nextInt() - 1;
		int c = keyboard.nextInt() - 1;
		if(mines[r][c] == 1)
		{
			outputBoard[r][c] = 'X';
			displayOutput(outputBoard);
			System.out.println("Ooppps! You stepped on a bomb. Sorry, game over!");
			return true;
		}
		outputBoard[r][c] = (countMines(mines, r, c) + "").charAt(0);
		mines[r][c] = -1;
		displayOutput(outputBoard);
		return checkStatus(mines);
	}
	private static boolean checkStatus(int[][] mines)
	{
		for(int r = 0; r < mines.length; r++)
		{
			for(int c = 0; c < mines[0].length; c++)
			{
				if(mines[r][c] == 0)
					return false;
			}
		}
		System.out.println("You win!!!");
		return true;
	}
}
