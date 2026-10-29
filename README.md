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

void generateMatrix(char key[]) {
    int used[26] = {0};
    int k = 0;

    used['J' - 'A'] = 1;

    for (int i = 0; key[i]; i++) {
        char ch = toupper(key[i]);

        if (ch < 'A' || ch > 'Z')
            continue;

        if (ch == 'J')
            ch = 'I';

        if (!used[ch - 'A']) {
            matrix[k / 5][k % 5] = ch;
            used[ch - 'A'] = 1;
            k++;
        }
    }

    for (char ch = 'A'; ch <= 'Z'; ch++) {
        if (!used[ch - 'A']) {
            matrix[k / 5][k % 5] = ch;
            used[ch - 'A'] = 1;
            k++;
        }
    }
}

void findPosition(char ch, int *row, int *col) {
    if (ch == 'J')
        ch = 'I';

    for (int i = 0; i < 5; i++) {
        for (int j = 0; j < 5; j++) {
            if (matrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}

void encrypt(char text[]) {
    int r1, c1, r2, c2;

    for (int i = 0; text[i]; i += 2) {
        findPosition(text[i], &r1, &c1);
        findPosition(text[i + 1], &r2, &c2);

        if (r1 == r2) {
            text[i] = matrix[r1][(c1 + 1) % 5];
            text[i + 1] = matrix[r2][(c2 + 1) % 5];
        }
        else if (c1 == c2) {
            text[i] = matrix[(r1 + 1) % 5][c1];
            text[i + 1] = matrix[(r2 + 1) % 5][c2];
        }
        else {
            text[i] = matrix[r1][c2];
            text[i + 1] = matrix[r2][c1];
        }
    }
}

void decrypt(char text[]) {
    int r1, c1, r2, c2;

    for (int i = 0; text[i]; i += 2) {
        findPosition(text[i], &r1, &c1);
        findPosition(text[i + 1], &r2, &c2);

        if (r1 == r2) {
            text[i] = matrix[r1][(c1 + 4) % 5];
            text[i + 1] = matrix[r2][(c2 + 4) % 5];
        }
        else if (c1 == c2) {
            text[i] = matrix[(r1 + 4) % 5][c1];
            text[i + 1] = matrix[(r2 + 4) % 5][c2];
        }
        else {
            text[i] = matrix[r1][c2];
            text[i + 1] = matrix[r2][c1];
        }
    }
}

int main() {
    char key[100], text[200];
    char processed[200], encrypted[200];
    int i, j = 0;

    printf("Enter the key: ");
    scanf("%99s", key);

    printf("Enter plaintext: ");
    scanf("%199s", text);

    generateMatrix(key);

    printf("\nPlayfair Matrix:\n");

    for (i = 0; i < 5; i++) {
        for (int k = 0; k < 5; k++)
            printf("%c ", matrix[i][k]);
        printf("\n");
    }

    for (i = 0; text[i]; i++) {
        char ch = toupper(text[i]);

        if (ch < 'A' || ch > 'Z')
            continue;

        if (ch == 'J')
            ch = 'I';

        if (j > 0 && processed[j - 1] == ch)
            processed[j++] = 'X';

        processed[j++] = ch;
    }

    if (j % 2 != 0)
        processed[j++] = 'X';

    processed[j] = '\0';

    strcpy(encrypted, processed);

    encrypt(encrypted);

    printf("\nEncrypted Text: %s\n", encrypted);

    decrypt(encrypted);

    printf("Decrypted Text: %s\n", encrypted);

    return 0;
}
```




Output:
<img width="481" height="427" alt="image" src="https://github.com/user-attachments/assets/0a34bb2f-a0cc-4174-b9b9-e15469fa7007" />

RESULT:
The program for playfair cipher method is executed successfully

