#include <iostream>
#include <string>
#include <algorithm>


using namespace std;


// define the SearchEngine class
class SearchEngine {
private:
   string* data;  // pointer to dynamic array for storing data
   int size;  // size of the array


public:
   // Constructor: initializes the array with the given size
   SearchEngine(int size) : size(size) {
       data = new string[size];
   }


   // Destructor: frees the dynamically allocated memory
   ~SearchEngine() {
       delete[] data;
   }


   // function to add data to the array at a specific index
   void addData(int index, string value) {
       if (index >= 0 && index < size) {
           data[index] = value;
       } else {
           cout << "Index out of range." << endl;
       }
   }


   // Function to search for a query in the data
   void search(string query) {
       transform(query.begin(), query.end(), query.begin(), ::tolower);
       bool found = false;
       for (int i = 0; i < size; i++) {
           string dataLowerCase = data[i];
           transform(dataLowerCase.begin(),
                     dataLowerCase.end(),
                     dataLowerCase.begin(), ::tolower);
           if (dataLowerCase == query) {
               cout << "Found " << data[i] << " at index " << i << endl;
               found = true;
           }
       }
       if (!found) {
           cout << query << " not found in the data." << endl;
       }
   }
};


// Define the Database class
class Database {
private:
   SearchEngine** engines;  // Pointer to dynamic array for storing SearchEngine pointers
   int size;  // Size of the array


public:
   // Constructor: initializes the array with the given size
   Database(int size) : size(size) {
       engines = new SearchEngine*[size];
       for (int i = 0; i < size; i++) {
           engines[i] = nullptr;
       }
   }


   // Destructor: frees the dynamically allocated memory
   ~Database() {
       for (int i = 0; i < size; i++) {
           delete engines[i];
       }
       delete[] engines;
   }


   // Function to add a SearchEngine to the database at a specific index
   void addEngine(int index, SearchEngine* engine) {
       if (index >= 0 && index < size) {
           engines[index] = engine;
       } else {
           cout << "Index out of range." << endl;
       }
   }


   // Function to search for a query in a specific SearchEngine
   void search(int index, string query) {
       if (index >= 0 && index < size && engines[index] != nullptr) {
           engines[index]->search(query);
       } else {
           cout << "Invalid engine index." << endl;
       }
   }
};


// Main function
int main() {
   int size = 5;
   // Create a new SearchEngine
   SearchEngine* engine = new SearchEngine(size);
   // Add some data to the SearchEngine
   string data[] = {"apple", "banana", "cherry", "home", "3"};
   for (int i = 0; i < size; i++) {
       engine->addData(i, data[i]);
   }


   // Create a new Database
   Database db(10);
   // Add the SearchEngine to the Database
   db.addEngine(0, engine);


   string query;
   // enter a loop where the user can search for words
   while (true) {
       cout << "Enter a word to search (or 'STOP' to quit): ";
       cin >> query;
       if (query == "STOP") {
           break;
       }
       db.search(0, query);
   }


   return 0;
}

