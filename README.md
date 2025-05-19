#include <iostream>
#include <iomanip>
#include <string>
#include <cstdlib>
#include <ctime>

using namespace std;

struct Team {
    string name;
    int played;
    int wins;
    int draws;
    int losses;
    int goals_for;
    int goals_against;
    int points;
};

// Update statistical groups
void updateStats(Team& home, Team& away, int goalsHome, int goalsAway) {
    home.played++;
    away.played++;

    home.goals_for += goalsHome;
    home.goals_against += goalsAway;

    away.goals_for += goalsAway;
    away.goals_against += goalsHome;

    if (goalsHome > goalsAway) {
        home.wins++;
        away.losses++;
        home.points += 3;
    } else if (goalsHome < goalsAway) {
        away.wins++;
        home.losses++;
        away.points += 3;
    } else {
        home.draws++;
        away.draws++;
        home.points += 1;
        away.points += 1;
    }
}

// Ranking of teams based on points and goal difference
void sortTeams(Team teams[], int size) {
    for (int i = 0; i < size - 1; ++i) {
        for (int j = i + 1; j < size; ++j) {
            int diff_i = teams[i].goals_for - teams[i].goals_against;
            int diff_j = teams[j].goals_for - teams[j].goals_against;
            if (teams[j].points > teams[i].points ||
                (teams[j].points == teams[i].points && diff_j > diff_i)) {
                Team temp = teams[i];
                teams[i] = teams[j];
                teams[j] = temp;
            }
        }
    }
}

int main() {
    srand(time(0));

    const int teamCount = 14;
    Team teams[teamCount];

    string names[teamCount] = {
        "Panathinaikos", "Olympiakos", "AEK", "PAOK",
        "Aris", "OFI", "Asteras", "Atromitos",
        "Volos", "Lamia", "Levadiakos", "Panserraikos",
        "Kallithea", "Panetolikos"
    };

    // initializing names and resetting statistics
    for (int i = 0; i < teamCount; ++i) {
        teams[i].name = names[i];
        teams[i].played = 0;
        teams[i].wins = 0;
        teams[i].draws = 0;
        teams[i].losses = 0;
        teams[i].goals_for = 0;
        teams[i].goals_against = 0;
        teams[i].points = 0;
    }

    // 2 rounds: each team plays each other twice (home-away)
    for (int i = 0; i < teamCount; ++i) {
        for (int j = i + 1; j < teamCount; ++j) {
            // first game
            int goalsHome1 = rand() % 5;
            int goalsAway1 = rand() % 5;
            updateStats(teams[i], teams[j], goalsHome1, goalsAway1);

            // second game
            int goalsHome2 = rand() % 5;
            int goalsAway2 = rand() % 5;
            updateStats(teams[j], teams[i], goalsHome2, goalsAway2);
        }
    }


    sortTeams(teams, teamCount);

    // Show leaderboard
    cout << "Super League Table:\n";
    cout << "-----------------------------------------------------------------------------\n";
    cout << left << setw(18) << "Team"
         << right << setw(5) << "P"
         << setw(5) << "W"
         << setw(5) << "D"
         << setw(5) << "L"
         << setw(7) << "GF"
         << setw(7) << "GA"
         << setw(8) << "Points" << endl;
    cout << "-----------------------------------------------------------------------------\n";


    for (int i = 0; i < teamCount; ++i) {
    Team t = teams[i];
    cout << right << setw(2) << (i + 1) << ". "
         << left << setw(16) << t.name
         << right << setw(5) << t.played
         << setw(5) << t.wins
         << setw(5) << t.draws
         << setw(5) << t.losses
         << setw(7) << t.goals_for
         << setw(7) << t.goals_against
         << setw(8) << t.points << endl;
}
std::cout << "\nThe winner is: " << teams[0].name << " with " << teams[0].points << " points!" << std::endl;


    return 0;
}

