#include <stdio.h>
#include <conio.h>
#include <map>

std::map<char,int> resources;

int a, b, c, d, num;
int main()
{
	printf("Enter number of A, B, C, and D resources found respectively:\n");
	scanf("%d", &a);
	scanf("%d", &b);
	scanf("%d", &c);
	scanf("%d", &d);
	resources['A'] = a;
	resources['B'] = b;
	resources['C'] = c;
	resources['D'] = d;
	
	printf("Enter number of processes: ");
	scanf("%d", &num);
	int current[20][4];
	int need[20][4];
	char car = 'A';
	printf ("Enter current allocation:\n"); // reading the current allocation for resources
	for (int i=0; i<num; i++)
	{
		
		for (int j=0; j<4; j++)
		{
			if (j == 0)
			{
				car = 'A';
			}
			if (j==1)
			{
				car = 'B';
			}
			else if (j==2)
			{
				car = 'C';
			}
			else if (j==3)
			{
				car = 'D';
			}
			scanf("%d", & current[i][j]);
			resources[car] -= current[i][j]; // calculating available resources
		}
	}
	printf ("Enter max need:\n"); // read the maximum number of resources needed by each process
	for (int i=0; i<num; i++)
	{
		for (int j=0; j<4; j++)
		{
			scanf("%d", & need[i][j]);
			need[i][j] -= current[i][j]; // calculating the left number of resources needed
		}
	}
	 int p;
	 bool discard[20];
	 
	 printf ("A B C D\n");
	printf ("%d %d %d %d\n\n", resources['A'], resources['B'], resources['C'], resources['D']);

	// start of algorithm calculations and checks

	int discarded = 0;
	for (int i=0; i<num; i++)
	{
		discard[i] = false;
	}
	 while (true)
	 {
		 p = 0;
		while (true)
		{
			printf ("p = %d ", p);
			// check if all processes are checked
			if (p == num)
			{
				break;
			}
			
			// check if the process can take its resources
			else if ((need[p][0] <= resources['A']) && (need[p][1] <= resources['B']) && (need[p][2] <= resources['C']) && (need[p][3] <= resources['D']) && (!discard[p]))
			{
				printf("\n\nI'm here\n\n");
				// release all allocated resources for this process after termination
				resources['A'] += current[p][0];
				resources['B'] += current[p][1];
				resources['C'] += current[p][2];
				resources['D'] += current[p][3];
				
				// discard this process to mark its termination
				discard[p] = true;
				discarded ++;
				break;
			}
			else 
			{
				p++;
			}
		}
		printf("\n");
		if(p == num)
		{
			break;
		}
	 }
	
	 // check if number of terminated processes is less than the total number of processes
	 // check for system saftey
	if (discarded < num)
	{
		printf("It's unsafe, deadlock may occur");
	}

	else
	{
		printf ("The system is safe from deadlock");
	}
	_getch();
	return 0;
}