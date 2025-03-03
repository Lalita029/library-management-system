# library-management-system
#include <iostream>
#include <string>
#include <vector>
using namespace std;

class Book {
public:
    string name;
    string author;
    int isbn;
    bool isIssued;

    Book(string n, string a, int isbn) {
        name = n;
        author = a;
        this->isbn = isbn;
        isIssued = false;  // By default, books are not issued
    }
};

class Library {
public:
    vector<Book> books;

    void addBook(string name, string author, int isbn) {
        Book newBook(name, author, isbn);
        books.push_back(newBook);
        cout << "Book added successfully!" << endl;
    }

    void deleteBookByISBN(int isbn) {
        bool found = false;
        for (auto it = books.begin(); it != books.end(); ++it) {
            if (it->isbn == isbn) {
                books.erase(it);
                cout << "Book deleted successfully!" << endl;
                found = true;
                break;
            }
        }
        if (!found) {
            cout << "Book with ISBN " << isbn << " not found!" << endl;
        }
    }

    void updateBook(int isbn, string newName, string newAuthor) {
        bool found = false;
        for (auto& book : books) {
            if (book.isbn == isbn) {
                book.name = newName;
                book.author = newAuthor;
                cout << "Book updated successfully!" << endl;
                found = true;
                break;
            }
        }
        if (!found) {
            cout << "Book with ISBN " << isbn << " not found!" << endl;
        }
    }

    void displayBooks() {
        if (books.empty()) {
            cout << "No books available in the library." << endl;
            return;
        }
        cout << "Listing all books in the library: " << endl;
        for (const auto& book : books) {
            cout << "Book Name: " << book.name << ", Author: " << book.author << ", ISBN: " << book.isbn << ", Issued: " << (book.isIssued ? "Yes" : "No") << endl;
        }
    }

    int countBooks() {
        return books.size();
    }

    bool issueBook(int isbn) {
        for (auto& book : books) {
            if (book.isbn == isbn) {
                if (book.isIssued) {
                    cout << "Book already issued!" << endl;
                    return false;
                }
                book.isIssued = true;
                cout << "Book issued successfully!" << endl;
                return true;
            }
        }
        cout << "Book with ISBN " << isbn << " not found!" << endl;
        return false;
    }

    bool returnBook(int isbn) {
        for (auto& book : books) {
            if (book.isbn == isbn) {
                if (!book.isIssued) {
                    cout << "Book wasn't issued!" << endl;
                    return false;
                }
                book.isIssued = false;
                cout << "Book returned successfully!" << endl;
                return true;
            }
        }
        cout << "Book with ISBN " << isbn << " not found!" << endl;
        return false;
    }
};

// Librarian Class
class Librarian {
public:
    Library& library;

    Librarian(Library& lib) : library(lib) {}

    void addBook() {
        string name, author;
        int isbn;

        cout << "Enter book name: ";
        cin.ignore();  // To clear the buffer before using getline
        getline(cin, name);
        cout << "Enter author name: ";
        getline(cin, author);
        cout << "Enter ISBN: ";
        cin >> isbn;

        library.addBook(name, author, isbn);
    }

    void deleteBook() {
        int isbn;
        cout << "Enter ISBN of the book to delete: ";
        cin >> isbn;

        library.deleteBookByISBN(isbn);
    }

    void updateBook() {
        int isbn;
        string newName, newAuthor;

        cout << "Enter ISBN of the book to update: ";
        cin >> isbn;
        cin.ignore(); // To clear the buffer before using getline

        cout << "Enter new name for the book: ";
        getline(cin, newName);
        cout << "Enter new author for the book: ";
        getline(cin, newAuthor);

        library.updateBook(isbn, newName, newAuthor);
    }

    void displayBooks() {
        library.displayBooks();
    }
};

// Counter User A Class
class CounterUserA {
public:
    Library& library;

    CounterUserA(Library& lib) : library(lib) {}

    void countBooks() {
        cout << "Total number of books in the library: " << library.countBooks() << endl;
    }

    void issueBook() {
        int isbn;
        cout << "Enter ISBN of the book to issue: ";
        cin >> isbn;

        library.issueBook(isbn);
    }
};

// Counter User B Class
class CounterUserB {
public:
    Library& library;

    CounterUserB(Library& lib) : library(lib) {}

    void returnBook() {
        int isbn;
        cout << "Enter ISBN of the book to return: ";
        cin >> isbn;

        library.returnBook(isbn);
    }
};

int main() {
    Library library;

    Librarian librarian(library);
    CounterUserA userA(library);
    CounterUserB userB(library);

    int choice;
    do {
        cout << "\nLibrary Management System\n";
        cout << "1. Librarian\n";
        cout << "2. Counter User A\n";
        cout << "3. Counter User B\n";
        cout << "4. Exit\n";
        cout << "Enter your choice: ";
        cin >> choice;

        switch (choice) {
        case 1:
            int librarianChoice;
            cout << "\nLibrarian Menu\n";
            cout << "1. Add Book\n";
            cout << "2. Delete Book\n";
            cout << "3. Update Book\n";
            cout << "4. Display Books\n";
            cout << "Enter your choice: ";
            cin >> librarianChoice;

            switch (librarianChoice) {
            case 1:
                librarian.addBook();
                break;
            case 2:
                librarian.deleteBook();
                break;
            case 3:
                librarian.updateBook();
                break;
            case 4:
                librarian.displayBooks();
                break;
            default:
                cout << "Invalid choice!" << endl;
            }
            break;

        case 2:
            int userAChoice;
            cout << "\nCounter User A Menu\n";
            cout << "1. Count Books\n";
            cout << "2. Issue Book\n";
            cout << "Enter your choice: ";
            cin >> userAChoice;

            switch (userAChoice) {
            case 1:
                userA.countBooks();
                break;
            case 2:
                userA.issueBook();
                break;
            default:
                cout << "Invalid choice!" << endl;
            }
            break;

        case 3:
            int userBChoice;
            cout << "\nCounter User B Menu\n";
            cout << "1. Return Book\n";
            cout << "Enter your choice: ";
            cin >> userBChoice;

            switch (userBChoice) {
            case 1:
                userB.returnBook();
                break;
            default:
                cout << "Invalid choice!" << endl;
            }
            break;

        case 4:
            cout << "Exiting the system. Goodbye!" << endl;
            break;

        default:
            cout << "Invalid choice! Please try again." << endl;
        }

    } while (choice != 4);

    return 0;
}
