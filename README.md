# HILL CIPHER

EX. NO: 3 HILL CIPHER
## AIM: 

To implement a C program for Hill Cipher.
 
## To write a C program to implement the hill cipher substitution techniques.

## DESCRIPTION:

Each letter is represented by a number modulo 26. Often the simple scheme A = 0, B
= 1... Z = 25, is used, but this is not an essential feature of the cipher. To encrypt a message, each block of n letters is  multiplied by an invertible n × n matrix, against modulus 26. To
decrypt the message, each block is multiplied by the inverse of the m trix used for encryption. The matrix used for encryption is the cipher key, and it shold be chosen randomly from the set of invertible n × n matrices (modulo 26).


## ALGORITHM:

STEP-1: Read the plain text and key from the user. 

STEP-2: Split the plain text into groups of length three. 

STEP-3: Arrange the keyword in a 3*3 matrix.

STEP-4: Multiply the two matrices to obtain the cipher text of length three.

STEP-5: Combine all these groups to get the complete cipher text.

## PROGRAM:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Function to find modular inverse (for determinant)
int modInverse(int det) {
    det = det % 26;
    for(int i = 1; i < 26; i++) {
        if((det * i) % 26 == 1)
            return i;
    }
    return -1;
}

// Encryption
void encrypt(char text[], int key[2][2]) {
    int i;
    for(i = 0; text[i] != '\0'; i += 2) {
        int a = text[i] - 'A';
        int b = text[i+1] - 'A';

        text[i]   = ((key[0][0]*a + key[0][1]*b) % 26) + 'A';
        text[i+1] = ((key[1][0]*a + key[1][1]*b) % 26) + 'A';
    }
}

// Decryption
void decrypt(char text[], int key[2][2]) {
    int det = key[0][0]*key[1][1] - key[0][1]*key[1][0];
    det = (det % 26 + 26) % 26;

    int invDet = modInverse(det);
    if(invDet == -1) {
        printf("Key not invertible!\n");
        return;
    }

    // Adjoint matrix
    int adj[2][2];
    adj[0][0] = key[1][1];
    adj[0][1] = -key[0][1];
    adj[1][0] = -key[1][0];
    adj[1][1] = key[0][0];

    // Inverse key matrix
    int invKey[2][2];
    for(int i = 0; i < 2; i++) {
        for(int j = 0; j < 2; j++) {
            invKey[i][j] = (adj[i][j] * invDet) % 26;
            if(invKey[i][j] < 0)
                invKey[i][j] += 26;
        }
    }

    // Apply inverse for decryption
    for(int i = 0; text[i] != '\0'; i += 2) {
        int a = text[i] - 'A';
        int b = text[i+1] - 'A';

        text[i]   = ((invKey[0][0]*a + invKey[0][1]*b) % 26) + 'A';
        text[i+1] = ((invKey[1][0]*a + invKey[1][1]*b) % 26) + 'A';
    }
}

int main() {
    char text[100];
    int key[2][2];

    // Input key matrix
    printf("Enter 2x2 key matrix:\n");
    for(int i = 0; i < 2; i++) {
        for(int j = 0; j < 2; j++) {
            scanf("%d", &key[i][j]);
        }
    }

    // Input plaintext
    printf("Enter plaintext (uppercase only): ");
    scanf("%s", text);

    // If length is odd, add 'X'
    int len = strlen(text);
    if(len % 2 != 0) {
        text[len] = 'X';
        text[len+1] = '\0';
    }

    // Encrypt
    encrypt(text, key);
    printf("Encrypted text: %s\n", text);

    // Decrypt
    decrypt(text, key);
    printf("Decrypted text: %s\n", text);

    return 0;
}
```

## OUTPUT:
<img width="1746" height="984" alt="image" src="https://github.com/user-attachments/assets/16e47c34-8abe-4fde-bf1d-68982ad63934" />

## RESULT:
Thus the C program for Hill Cipher was executed successfully.
