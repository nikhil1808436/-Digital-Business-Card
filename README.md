# hangman game
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <time.h>
#include <ctype.h>

#define MAX_WORDS 10
#define MAX_LEN 20
#define MAX_ATTEMPTS 6

// Hangman ASCII stages
const char *hangman[] = {
    " \n\n\n\n=====",
    " |\n |\n |\n |\n=====",
    " _______\n |\n |\n |\n |\n=====",
    " _______\n |     |\n |\n |\n |\n=====",
    " _______\n |     |\n |     O\n |\n |\n=====",
    " _______\n |     |\n |     O\n |    /|\\\n |\n=====",
    " _______\n |     |\n |     O\n |    /|\\\n |    / \\\n====="
};

// Word list
char words[MAX_WORDS][MAX_LEN] = {
    "apple", "banana", "grapes", "orange", "mango",
    "peach", "cherry", "lemon", "papaya", "guava"
};

// Function to choose random word
void getRandomWord(char *word) {
    int index = rand() % MAX_WORDS;
    strcpy(word, words[index]);
}

// Display current progress
void displayWord(char *word, int guessed[]) {
    for (int i = 0; i < strlen(word); i++) {
        if (guessed[i])
            printf("%c ", word[i]);
        else
            printf("_ ");
    }
    printf("\n");
}

// Check if word is fully guessed
int isWordGuessed(char *word, int guessed[]) {
    for (int i = 0; i < strlen(word); i++) {
        if (!guessed[i])
            return 0;
    }
    return 1;
}

int main() {
    char playAgain;

    srand(time(NULL));

    do {
        char word[MAX_LEN];
        getRandomWord(word);

        int len = strlen(word);
        int guessed[MAX_LEN] = {0};
        int attempts = MAX_ATTEMPTS;
        char guessedLetters[26] = {0};

        printf("\n🎮 Welcome to Hangman!\n");

        while (attempts > 0) {
            printf("\n%s\n", hangman[MAX_ATTEMPTS - attempts]);
            printf("Attempts left: %d\n", attempts);

            displayWord(word, guessed);

            printf("Guessed letters: ");
            for (int i = 0; i < 26; i++) {
                if (guessedLetters[i])
                    printf("%c ", i + 'a');
            }
            printf("\n");

            char guess;
            printf("Enter a letter: ");
            scanf(" %c", &guess);

            guess = tolower(guess);

            if (guess < 'a' || guess > 'z') {
                printf("Invalid input! Enter a letter.\n");
                continue;
            }

            if (guessedLetters[guess - 'a']) {
                printf("You already guessed that letter!\n");
                continue;
            }

            guessedLetters[guess - 'a'] = 1;

            int found = 0;
            for (int i = 0; i < len; i++) {
                if (word[i] == guess) {
                    guessed[i] = 1;
                    found = 1;
                }
            }

            if (!found) {
                attempts--;
                printf("Wrong guess!\n");
            } else {
                printf("Correct guess!\n");
            }

            if (isWordGuessed(word, guessed)) {
                printf("\n🎉 You WON! The word was: %s\n", word);
                break;
            }
        }

        if (attempts == 0) {
            printf("\n%s\n", hangman[MAX_ATTEMPTS]);
            printf("💀 You LOST! The word was: %s\n", word);
        }

        printf("\nDo you want to play again? (y/n): ");
        scanf(" %c", &playAgain);

    } while (playAgain == 'y' || playAgain == 'Y');

    printf("Thanks for playing! 👋\n");

    return 0;
}
