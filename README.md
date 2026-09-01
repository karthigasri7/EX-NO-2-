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

char keyTable[5][5];

void generateKeyTable(char key[])
{
    int used[26] = {0};
    int i, j = 0, k = 0;

    used['J' - 'A'] = 1;

    for(i = 0; key[i] != '\0'; i++)
    {
        char ch = toupper(key[i]);

        if(ch == 'J')
            ch = 'I';

        if(ch >= 'A' && ch <= 'Z' && !used[ch - 'A'])
        {
            keyTable[j][k++] = ch;
            used[ch - 'A'] = 1;

            if(k == 5)
            {
                j++;
                k = 0;
            }
        }
    }

    for(i = 0; i < 26; i++)
    {
        if(!used[i])
        {
            keyTable[j][k++] = i + 'A';

            if(k == 5)
            {
                j++;
                k = 0;
            }
        }
    }
}

void findPosition(char ch, int *row, int *col)
{
    int i, j;

    if(ch == 'J')
        ch = 'I';

    for(i = 0; i < 5; i++)
    {
        for(j = 0; j < 5; j++)
        {
            if(keyTable[i][j] == ch)
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
    char key[50], text[100], cipher[100], decrypted[100];
    int i, r1, c1, r2, c2, len = 0;

    printf("Enter the keyword: ");
    scanf("%s", key);

    printf("Enter the plain text: ");
    scanf("%s", text);

    generateKeyTable(key);

    printf("\nKey Matrix:\n");

    for(i = 0; i < 5; i++)
    {
        printf("%c %c %c %c %c\n",
               keyTable[i][0], keyTable[i][1],
               keyTable[i][2], keyTable[i][3],
               keyTable[i][4]);
    }

    /* ENCRYPTION */

    printf("\nCipher Text: ");

    for(i = 0; text[i] != '\0'; i += 2)
    {
        char a = toupper(text[i]);
        char b = text[i + 1] ? toupper(text[i + 1]) : 'X';

        if(a == 'J')
            a = 'I';

        if(b == 'J')
            b = 'I';

        if(a == b)
            b = 'X';

        findPosition(a, &r1, &c1);
        findPosition(b, &r2, &c2);

        if(r1 == r2)
        {
            cipher[len++] = keyTable[r1][(c1 + 1) % 5];
            cipher[len++] = keyTable[r2][(c2 + 1) % 5];
        }
        else if(c1 == c2)
        {
            cipher[len++] = keyTable[(r1 + 1) % 5][c1];
            cipher[len++] = keyTable[(r2 + 1) % 5][c2];
        }
        else
        {
            cipher[len++] = keyTable[r1][c2];
            cipher[len++] = keyTable[r2][c1];
        }
    }

    cipher[len] = '\0';

    printf("%s\n", cipher);

    /* DECRYPTION */

    printf("Decrypted Plain Text: ");

    for(i = 0; cipher[i] != '\0'; i += 2)
    {
        findPosition(cipher[i], &r1, &c1);
        findPosition(cipher[i + 1], &r2, &c2);

        if(r1 == r2)
        {
            decrypted[i] = keyTable[r1][(c1 + 4) % 5];
            decrypted[i + 1] = keyTable[r2][(c2 + 4) % 5];
        }
        else if(c1 == c2)
        {
            decrypted[i] = keyTable[(r1 + 4) % 5][c1];
            decrypted[i + 1] = keyTable[(r2 + 4) % 5][c2];
        }
        else
        {
            decrypted[i] = keyTable[r1][c2];
            decrypted[i + 1] = keyTable[r2][c1];
        }
    }

    decrypted[len] = '\0';

    printf("%s\n", decrypted);

    return 0;
}
```

Output:

<img width="474" height="428" alt="image" src="https://github.com/user-attachments/assets/22e7ff8f-2ab0-43a5-9047-4809c0ee0570" />

result:

Thus, the Playfair Cipher algorithm was successfully implemented.


