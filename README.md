# Lab-Exercise---2---Playfair-Cipher

## Aim:
To implement the Playfair Cipher encryption algorithm using Python to encrypt plaintext based on a 5 × 5 key matrix and the given key.

## Procedure:

1.Define a function caesar_cipher(text, shift) to perform encryption and decryption.
2.Initialize an empty string result to store the converted message.
3.Iterate through each character in the input text.
4.Check whether the character is uppercase or lowercase and shift it according to the given key using ASCII values and modulo 26.
5.Keep spaces, numbers, and special characters unchanged.
6.Get the user's choice (e for encryption or d for decryption), the message, and the shift value as input.
7.If the choice is e, encrypt the message using the given shift value.
8.If the choice is d, decrypt the message by reversing the shift direction.
9.Display the resulting encrypted or decrypted message. If the choice is invalid, display an error message.
## Program:
```
def generate_matrix(key):
    key = key.upper().replace("J", "I")
    seen = set()
    matrix_letters = []

    for char in key:
        if char.isalpha() and char not in seen:
            seen.add(char)
            matrix_letters.append(char)

    alphabet = "ABCDEFGHIKLMNOPQRSTUVWXYZ"  # Note: 'J' is omitted
    for char in alphabet:
        if char not in seen:
            seen.add(char)
            matrix_letters.append(char)

    # Build 5x5 matrix
    matrix = [matrix_letters[i : i + 5] for i in range(0, 25, 5)]
    return matrix


def prepare_text(text):
    text = text.upper().replace("J", "I")
    cleaned = [c for c in text if c.isalpha()]

    pairs = []
    i = 0
    while i < len(cleaned):
        a = cleaned[i]
        if i + 1 < len(cleaned):
            b = cleaned[i + 1]
            if a == b:
                pairs.append(a + "X")
                i += 1
            else:
                pairs.append(a + b)
                i += 2
        else:
            pairs.append(a + "X")
            i += 1
    return pairs


def playfair_encrypt(plaintext, key):
    matrix = generate_matrix(key)
    pairs = prepare_text(plaintext)

    def position(char):
        for r in range(5):
            for c in range(5):
                if matrix[r][c] == char:
                    return r, c
        return None

    ciphertext = ""

    for pair in pairs:
        r1, c1 = position(pair[0])
        r2, c2 = position(pair[1])

        if r1 == r2:  # Same row
            ciphertext += matrix[r1][(c1 + 1) % 5]
            ciphertext += matrix[r2][(c2 + 1) % 5]
        elif c1 == c2:  # Same column
            ciphertext += matrix[(r1 + 1) % 5][c1]
            ciphertext += matrix[(r2 + 1) % 5][c2]
        else:  # Rectangle rule
            ciphertext += matrix[r1][c2]
            ciphertext += matrix[r2][c1]

    return matrix, pairs, ciphertext


key = input("Enter the key: ")
plaintext = input("Enter the plaintext: ")

matrix, pairs, ciphertext = playfair_encrypt(plaintext, key)

print("\nKey Matrix:")
for row in matrix:
    print(" ".join(row))

print("\nPlaintext pairs:", ", ".join(pairs))
print("Encrypted text:", ciphertext)
```
## Output:
<img width="1512" height="835" alt="image" src="https://github.com/user-attachments/assets/44081a83-d6db-43f9-9d06-dd0874ac6c2b" />


## Result:
Thus, the Playfair Cipher encryption algorithm was implemented successfully using Python.
