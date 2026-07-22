## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char matrix[5][5];
void generateMatrix(char key[])
{
    int used[26] = {0};
    int i, j, k = 0;

    used['J' - 'A'] = 1; 

    for(i = 0; key[i] != '\0'; i++)
    {
        char ch = toupper(key[i]);
        if(ch == 'J')
            ch = 'I';

        if(ch >= 'A' && ch <= 'Z' && !used[ch - 'A'])
        {
            matrix[k / 5][k % 5] = ch;
            used[ch - 'A'] = 1;
            k++;
        }
    }

    for(i = 0; i < 26; i++)
    {
        if(!used[i])
        {
            matrix[k / 5][k % 5] = i + 'A';
            k++;
        }
    }
}
void findPosition(char ch, int *row, int *col)
{
    if(ch == 'J')
        ch = 'I';

    int i, j;
    for(i = 0; i < 5; i++)
    {
        for(j = 0; j < 5; j++)
        {
            if(matrix[i][j] == ch)
            {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

int main()
{
    char key[30], text[30];
    int i;

    printf("Enter Key: ");
    scanf("%s", key);

    printf("Enter Plain Text: ");
    scanf("%s", text);

    generateMatrix(key);

    printf("\nPlayfair Matrix:\n");
    for(i = 0; i < 5; i++)
    {
        int j;
        for(j = 0; j < 5; j++)
        {
            printf("%c ", matrix[i][j]);
        }
        printf("\n");
    }

    printf("\nEncrypted Text: ");

    for(i = 0; i < strlen(text); i += 2)
    {
        char a = toupper(text[i]);
        char b;

        if(i + 1 < strlen(text))
            b = toupper(text[i + 1]);
        else
            b = 'X';

        int r1, c1, r2, c2;

        findPosition(a, &r1, &c1);
        findPosition(b, &r2, &c2);

        if(r1 == r2)   
        {
            printf("%c%c",
                   matrix[r1][(c1 + 1) % 5],
                   matrix[r2][(c2 + 1) % 5]);
        }
        else if(c1 == c2)  
        {
            printf("%c%c",
                   matrix[(r1 + 1) % 5][c1],
                   matrix[(r2 + 1) % 5][c2]);
        }
        else  
        {
            printf("%c%c",
                   matrix[r1][c2],
                   matrix[r2][c1]);
        }
    }

    return 0;
}
```

Output:

<img width="1677" height="637" alt="image" src="https://github.com/user-attachments/assets/ffd38041-d3e8-4eb0-a37c-171a9c3f2ff5" />

result:

Thus, the Playfair Cipher algorithm was successfully implemented.


