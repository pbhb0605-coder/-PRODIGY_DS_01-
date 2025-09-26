#include <iostream>
#include <vector>
#include <cstdlib>
#include <ctime>
#include <iomanip>
using namespace std;

// -------------------------------
// Histogram Class
// -------------------------------
class Histogram {
private:
    int binSize;
    int numBins;
    vector<int> bins;

public:
    Histogram(int maxValue, int binSize) : binSize(binSize) {
        numBins = (maxValue + binSize) / binSize;
        bins.resize(numBins, 0);
    }

    // Add a data point into the correct bin
    void addData(int value) {
        int index = value / binSize;
        if (index >= 0 && index < numBins) {
            bins[index]++;
        }
    }

    // Display histogram
    void display() const {
        cout << "\nAge Distribution Histogram\n";
        cout << "-----------------------------------------------------\n";

        for (int i = 0; i < numBins; i++) {
            int lower = i * binSize;
            int upper = lower + binSize - 1;

            cout << setw(2) << lower << "-" << setw(2) << upper << " | ";
            
            // Draw bar
            for (int j = 0; j < bins[i]; j++) {
                cout << "*";
            }
            cout << " (" << bins[i] << ")\n";
        }
    }
};

// -------------------------------
// Data Generator Class
// -------------------------------
class DataGenerator {
public:
    static vector<int> generateRandomAges(int count, int maxAge) {
        vector<int> data(count);
        for (int i = 0; i < count; i++) {
            data[i] = rand() % (maxAge + 1); // age between 0–maxAge
        }
        return data;
    }
};

// -------------------------------
// Main Application
// -------------------------------
int main() {
    srand(time(0)); // Seed random generator

    int populationSize, maxAge, binSize;

    // User input
    cout << "Enter population size: ";
    cin >> populationSize;

    cout << "Enter maximum age: ";
    cin >> maxAge;

    cout << "Enter bin size (e.g., 10 for decades): ";
    cin >> binSize;

    cout << "\nGenerating random dataset...\n";

    // Generate random dataset
    vector<int> ages = DataGenerator::generateRandomAges(populationSize, maxAge);

    // Create histogram
    Histogram hist(maxAge, binSize);

    // Add data points
    for (int age : ages) {
        hist.addData(age);
    }

    // Display results
    cout << "\nPopulation Size: " << populationSize << endl;
    hist.display();

    return 0;
}
