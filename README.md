<div align="center">

## Curly Bracket Counter


</div>

### Description

Counts all valid curly brackets in a c++ source file. Tells where a file's brackets may be out of balance OR tells the user if the brackets are in balance.
 
### More Info
 
file input

input file must be in same folder as .exe

(after compiled of course)

output to screen

none known


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[Jon Martin](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/jon-martin.md)
**Level**          |Beginner
**User Rating**    |5.0 (10 globes from 2 users)
**Compatibility**  |C\+\+ \(general\)
**Category**       |[Debugging and Error Handling](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/debugging-and-error-handling__3-6.md)
**World**          |[C / C\+\+](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/c-c.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/jon-martin-curly-bracket-counter__3-401/archive/master.zip)





### Source Code

```
/*************************************************************************
J.R. Martin
Curly Bracket counter.
This program counts all valid curly brackets in c++
disregarding comment lines and paragraphs as well as constants (char and strings
including curly brackets).
*************************************************************************/
#include <iostream>
#include <cstdlib>
#include <string>
#include <fstream>
using namespace std;
void OpenInFile ( ifstream&, ofstream& );
void OpenOutFile ( ofstream& );
void CheckForBrackets ( ifstream&, ofstream& );
void GetComments ( char&, ifstream& );
void GetStringConstant ( char&, ifstream& );
void GetCharConstant ( char&, ifstream& );
int main()
{
	ofstream out;
	ifstream in;
	OpenOutFile ( out );
	OpenInFile ( in, out );
	CheckForBrackets ( in, out );
	out << "\n\nThank you for choosing Jon's Software.\n\n";
return 0;
}
void OpenInFile ( ifstream& SomeInFile, ofstream& out )
{
	string FileName;
	cout << "This Program counts Curly Brackets for a C++ program file and\n"
		<<	"validates if they are in balance. Please make sure you have placed your\n"
		<<	"file in the same folder as this program.\n\n";
		cout << "Please type the name of your input file: " << endl << endl;
	cin >> FileName;
	cout << FileName;
	cout << endl << endl;
	SomeInFile.open(FileName.c_str());
	if ( !SomeInFile )
	{
		out << "** Can't open " << FileName << " **" << endl;
		cout << "** Can't open " << FileName << " **" << endl;
		exit (1);
	}
}
void OpenOutFile ( ofstream& out )
{
	out.open( "output.dat" );			// Open the OutFile
		if ( !out )						// Test Outfile and warn if not opened
		{
			out << "*** Can't open output file ***\n\n";
			cout << "*** Can't open output file ***\n\n";
			exit (1);
		}
}
void CheckForBrackets ( ifstream& in, ofstream& out )
{
	char character;
	int countLeft = 0;
	int countRight = 0;
	int totalCount = 0;
	int runningCount = 0;
	in.get(character);
		while ( in && runningCount >= 0 )
		{
			while ( in && (character == ' ' || character == '\n') )
			{
				in.get(character);
			}
			switch ( character )
			{
			case '/':	GetComments (character, in);
				break;
			case '"':	GetStringConstant (character, in);
				break;
			case '\'':	GetCharConstant (character, in);
				break;
			}
			if ( character == '{' )
			{
				countLeft++;
				runningCount++;
				totalCount++;
			}
			else if (character == '}' )
			{
				countRight++;
				runningCount--;
				totalCount++;
			}
			if ( runningCount < 0 )
			{
				cout << "Your curly brackets are out of balance !!! " << endl << endl;
				cout << "The Program has stopped AND the counters are at the following positions: "
					<< endl << endl;
				cout << "Left's: " << countLeft << endl <<
					"Right's: " << countRight << endl << "Total count: "
					<< totalCount << endl << endl
					<< "** USE THE ABOVE COUNTS TO FIND THE ERROR!! **" << endl << endl;
				exit (1);
			}
			in.get(character);
		}
			cout << "The number of left brackets is " << countLeft << endl; //give totals
			cout << "The number of right brackets is " << countRight << endl;
			cout << "The total bracket count is " << totalCount << endl;
			if ( runningCount == 0 )
			{
																				// validate counts
				cout << "The curly brackets {} are in balance." << endl;			// OK
			}
			if ( runningCount != 0 )
			{
				cout << "The curly brackets {} are out of balance. " << endl;	// shun a bad count
																				// Not OK
			}
}
void GetComments ( char& c, ifstream& in )
{
	in.get(c);
	if ( c == '/' )
	{
		while ( in && c != '\n' )
		{
			in.get(c);
		}
	}
	else if ( c == '*' )
	{
		in.get(c);
		while (in)
		{
			while ( in && c != '*' )
			{
				in.get(c);
			}
			if ( c == '*' )
			{
				in.get(c);
				if ( c == '/' )
					return;
			}
		}
	}
	else
		return;
}
void GetStringConstant ( char& c, ifstream& in )
{
		do
		{
			in.get(c);
			while ( c == '\"' ) // get the escape sequence for \"
				in.get(c);
			if ( c == '\n' )
				return;
		}while ( in && c != '"' );
}
void GetCharConstant ( char& c, ifstream& in )
{
		do
		{
			in.get(c);
			while ( c == '\'' )
				in.get(c);
			if ( c == '\n')
				return;
		}while ( in && c != '\'' );
}
```

