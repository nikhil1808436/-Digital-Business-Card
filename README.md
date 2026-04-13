# hangman game
#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include <string.h>

#define MAX_ATTEMPTS 6

void displayWord(char word[], char guessed[]) {
    for (int i = 0; i < strlen(word); i++) {
        if (guessed[i])
            printf("%c ", word[i]);
        else
            printf("_ ");
    }
    printf("\n");
}

int main() {
    char *words[] = {"apple", "banana", "grape"};
    srand(time(0));

    char *word = words[rand() % 3];
    int len = strlen(word);

    int guessed[20] = {0};
    int attempts = MAX_ATTEMPTS;

    while (attempts > 0) {
        displayWord(word, guessed);

        char guess;
        printf("Enter a letter: ");
        scanf(" %c", &guess);

        int found = 0;
        for (int i = 0; i < len; i++) {
            if (word[i] == guess) {
                guessed[i] = 1;
                found = 1;
            }
        }

        if (!found) attempts--;

        int complete = 1;
        for (int i = 0; i < len; i++) {
            if (!guessed[i]) complete = 0;
        }

        if (complete) {
            printf("You win! 🎉 Word: %s\n", word);
            return 0;
        }
    }

    printf("Game over! Word was: %s\n", word);
    return 0;
}
