```
#include <stdio.h>
#include <string.h>

#define MAX_PLANETS 100

struct atmosphere
{
    float oxygen;
    float carbonDioxide;
    float nitrogen;
};

struct planet
{
    char planetName[50];
    float distanceFromEarth;
    float gravity;
    struct atmosphere atmo;
    char habitable;
};

int main()
{
    int n, i, in;
    char nameKeyword[50];
    float oxyThreshold, co2Threshold;

    printf("Enter the number of planets: ");
    scanf("%d", &n);

    struct planet planets[MAX_PLANETS];

    // Input handling for planet details
    for (i = 0; i < n; i++)
    {
        // Prompt the user to enter details for each planet.
        printf("Enter Planet Name: ");
        // Assume single word name (no spaces)
        scanf("%s", planets[i].planetName);

        printf("Enter Distance from Earth (in light-years): ");
        scanf("%f", &planets[i].distanceFromEarth);

        printf("Enter Planet Gravity (in terms of g): ");
        scanf("%f", &planets[i].gravity);

        printf("Enter Atmosphere Composition (Oxygen, Carbon Dioxide, Nitrogen) as percentages: ");
        scanf("%f", &planets[i].atmo.oxygen);
        scanf("%f", &planets[i].atmo.carbonDioxide);
        scanf("%f", &planets[i].atmo.nitrogen);

        // Check habitability and store result in 'habitable' field
        // Habitability: gravity in [0.8, 1.2] AND oxygen in [18, 25]
        if (planets[i].gravity >= 0.8f && planets[i].gravity <= 1.2f &&
            planets[i].atmo.oxygen >= 18.0f && planets[i].atmo.oxygen <= 25.0f)
        {
            planets[i].habitable = 'Y';
        }
        else
        {
            planets[i].habitable = 'N';
        }
    }

    while (1)
    {
        // Provide the menu options
        printf("\nMENU\n1. Find and print all habitable planets\n2. Search by oxygen and carbon dioxide levels\n3. Search by keyword in planet names\n4. Exit\n");
        scanf("%d", &in);

        switch (in)
        {
        case 1:
        {
            // Sort planets by distance using Selection Sort
            int j, minIdx;
            for (i = 0; i < n - 1; i++)
            {
                minIdx = i;
                for (j = i + 1; j < n; j++)
                {
                    if (planets[j].distanceFromEarth < planets[minIdx].distanceFromEarth)
                        minIdx = j;
                }
                if (minIdx != i)
                {
                    struct planet temp = planets[i];
                    planets[i] = planets[minIdx];
                    planets[minIdx] = temp;
                }
            }

            printf("Habitable planets (sorted by distance from Earth):\n");
            // Print habitable planets in ascending order of distance
            for (i = 0; i < n; i++)
            {
                if (planets[i].habitable == 'Y')
                {
                    printf("%s - %.2f light-years\n",
                           planets[i].planetName,
                           planets[i].distanceFromEarth);
                }
            }
            break;
        }

        case 2:
            printf("Enter minimum oxygen level threshold: ");
            scanf("%f", &oxyThreshold);

            printf("Enter maximum carbon dioxide level threshold: ");
            scanf("%f", &co2Threshold);

            printf("Planets with oxygen > %.2f%% and carbon dioxide < %.2f%%:\n", oxyThreshold, co2Threshold);
            for (i = 0; i < n; i++)
            {
                if (planets[i].atmo.oxygen > oxyThreshold &&
                    planets[i].atmo.carbonDioxide < co2Threshold)
                {
                    printf("%s\n", planets[i].planetName);
                }
            }
            break;

        case 3:
            printf("Enter a keyword to search in planet names: ");
            scanf("%s", nameKeyword);

            printf("Planets matching the keyword \"%s\":\n", nameKeyword);
            for (i = 0; i < n; i++)
            {
                if (strstr(planets[i].planetName, nameKeyword) != NULL)
                {
                    printf("%s\n", planets[i].planetName);
                }
            }
            break;

        case 4:
            printf("Exiting...\n");
            return 0;

        default:
            printf("Invalid input\n");
            break;
        }
    }
}

```
