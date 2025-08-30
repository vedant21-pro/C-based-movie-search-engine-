#include <stdio.h>
#include <string.h>

#define MAX 100

typedef struct {
    int id;
    char name[100];
    int year;
    float rating;
} Movie;

Movie movies[MAX];
int count = 0;

// Function to add a movie
void addMovie() {
    if (count >= MAX) {
        printf("Database is full!\n");
        return;
    }
    printf("Enter Movie ID: ");
    scanf("%d", &movies[count].id);
    printf("Enter Movie Name: ");
    getchar();  // To consume the newline
    fgets(movies[count].name, 100, stdin);
    movies[count].name[strcspn(movies[count].name, "\n")] = '\0';  // Remove newline
    printf("Enter Release Year: ");
    scanf("%d", &movies[count].year);
    printf("Enter Rating (0.0 - 10.0): ");
    scanf("%f", &movies[count].rating);
    count++;
    printf("Movie added successfully.\n");
}

// Function to display all movies
void displayMovies() {
    if (count == 0) {
        printf("No movies to display.\n");
        return;
    }
    printf("\n%-5s %-30s %-10s %-10s\n", "ID", "Name", "Year", "Rating");
    printf("-------------------------------------------------------------\n");
    for (int i = 0; i < count; i++) {
        printf("%-5d %-30s %-10d %-10.1f\n", movies[i].id, movies[i].name, movies[i].year, movies[i].rating);
    }
}

// Linear search by name
void searchByName() {
    char target[100];
    printf("Enter movie name to search: ");
    getchar();
    fgets(target, 100, stdin);
    target[strcspn(target, "\n")] = '\0';

    int found = 0;
    for (int i = 0; i < count; i++) {
        if (strcmp(movies[i].name, target) == 0) {
            printf("Movie found: ID: %d, Name: %s, Year: %d, Rating: %.1f\n",
                   movies[i].id, movies[i].name, movies[i].year, movies[i].rating);
            found = 1;
        }
    }
    if (!found) {
        printf("Movie not found.\n");
    }
}

// Sort by year (ascending) using Insertion Sort
void sortByYear() {
    for (int i = 1; i < count; i++) {
        Movie key = movies[i];
        int j = i - 1;
        while (j >= 0 && movies[j].year > key.year) {
            movies[j + 1] = movies[j];
            j--;
        }
        movies[j + 1] = key;
    }
}

// Binary search by year (after sorting)
void searchByYear() {
    if (count == 0) {
        printf("No movies to search.\n");
        return;
    }

    sortByYear();  // Ensure it's sorted
    int targetYear;
    printf("Enter release year to search: ");
    scanf("%d", &targetYear);

    int low = 0, high = count - 1, found = 0;
    while (low <= high) {
        int mid = (low + high) / 2;
        if (movies[mid].year == targetYear) {
            // Display all matching years
            int i = mid;
            while (i >= 0 && movies[i].year == targetYear) i--;
            i++;
            printf("Movies released in %d:\n", targetYear);
            while (i < count && movies[i].year == targetYear) {
                printf("ID: %d, Name: %s, Rating: %.1f\n", movies[i].id, movies[i].name, movies[i].rating);
                i++;
            }
            found = 1;
            break;
        } else if (movies[mid].year < targetYear) {
            low = mid + 1;
        } else {
            high = mid - 1;
        }
    }
    if (!found) {
        printf("No movies found for year %d.\n", targetYear);
    }
}

// Sort by rating (descending) using Bubble Sort
void sortByRating() {
    for (int i = 0; i < count - 1; i++) {
        for (int j = 0; j < count - i - 1; j++) {
            if (movies[j].rating < movies[j + 1].rating) {
                Movie temp = movies[j];
                movies[j] = movies[j + 1];
                movies[j + 1] = temp;
            }
        }
    }
    printf("Movies sorted by rating (descending).\n");
}

int main() {
    int choice;
    do {
        printf("\n===== Movie Database Menu =====\n");
        printf("1. Add Movie\n");
        printf("2. Display Movies\n");
        printf("3. Search by Name\n");
        printf("4. Search by Year\n");
        printf("5. Sort by Rating\n");
        printf("6. Sort by Year\n");
        printf("0. Exit\n");
        printf("Enter your choice: ");
        scanf("%d", &choice);

        switch (choice) {
            case 1:
                addMovie();
                break;
            case 2:
                displayMovies();
                break;
            case 3:
                searchByName();
                break;
            case 4:
                searchByYear();
                break;
            case 5:
                sortByRating();
                displayMovies();
                break;
            case 6:
                sortByYear();
                displayMovies();
                break;
            case 0:
                printf("Exiting Movie Database. Goodbye!\n");
                break;
            default:
                printf("Invalid choice. Try again.\n");
        }

    } while (choice != 0);

    return 0;
}
