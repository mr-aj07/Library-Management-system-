# Library-Management-system-
# Create a library class
# Create methods to Display books, Lend books, Add books and Return books

"""             #######          WARNING        #######
Before running the program make sure that you have "Bookslist.txt" containing all the
list of available library books and "Lendersdict.txt" containing borrowed books.
These two files should be in the same directory of the program.

 """

from datetime import datetime


def getdate():
    return datetime.now()                      # Returns current date time


with open("Bookslist", "r") as f:           # Open the txt file of Bookslist in read mode
    Books_list = []                                 # Initialize Books_list as an  empty list
    for item in f:
        Books_list.append(item.strip())    # Copy all the items of Bookslist.txt into the list

Lenders_dict = {}                                 # Initialize Lenders_dict as an empty dictionary
with open("Lendersdict", "r") as f:
    for line in f:
        (Book, Name) = line.strip().split(" :- ", 1)          # Converts the items of Lendersdict.txt into a dictionary
        Lenders_dict[Book] = Name.strip()


def Display():                                                           # Function for displaying books in the library line by line
    for x in Books_list:
        print(x)


def AddBook():                                                       # Function to add a new book in the library
    book = str(input("Enter the name of the book: "))
    with open("Bookslist", "a") as a:
        a.write(f"{book}")                                             # Appends the the books in Bookslist.txt
    Books_list.append(book)                                      # Appends the books in the list
    print("The Book has been added successfully\n")


def Search():                                                         # Function to search the book
    global line, f, Book, Name
    with open("Lendersdict", "r") as f:
        for line in f:
            (Book, Name) = line.strip().split(" :- ", 1)
            Lenders_dict[Book] = Name.strip()            # Re-format the dictionary again to keep it up to date

    Book = str(input("Enter the name of the book: "))
    if Book in Books_list:
        print("\nThe book is in the library\n")
    elif Book in Lenders_dict:
        print(f"\nThis book has been borrowed by {Lenders_dict[Book]}.\n")             # If the book is borrowed
    else:
        print("\nThis book is not the property of the Library\n")


class MyLibrary:
    def __init__(self):                                                                                       # The Function will take the name of book and the name of borrower
        self.book = str(input("Enter the name of the book: "))
        self.name = str(input("Enter the name of the borrower: "))

    def Lend(self):                                                      # Function to lend the book
        global line, f, Book, Name, Books_list
        if self.book in Books_list:
            Books_list.remove(self.book)                        # Remove the book from the list
            f = open("Bookslist", "w")
            for element in Books_list:
                f.write(element)                                        # Updates the Bookslist.txt from the list by re-formatting it
                f.write("\n")
            f.close()
            with open("Lendersdict", "a") as f:
                f.write(f"{self.book} :- {self.name} at time {str(getdate())}\n")    # Appends the name of book, Name of borrower in Lendersdict.txt
            with open("Lendersdict", "r") as f:
                for line in f:
                    (Book, Name) = line.strip().split(" :- ", 1)                         # Re-formats the dictionary from Lendersdict.txt to keep it updated
                    Lenders_dict[Book] = Name.strip()

            print("The book has been lended successfully\n")
        else:
            print("Sorry the book is not in the Library right now\n")

    def Return(self):                                                             # Function to return the borrowed book
        global line, f, Book, Name
        with open("Lendersdict", "r") as f:
            for line in f:
                (Book, Name) = line.strip().split(" :- ", 1)              # Updates the dictionary from Lendersdict.txt
                Lenders_dict[Book] = Name.strip()
        if self.book in Lenders_dict:
            Lenders_dict.pop(self.book)                                    # Removes the item from the dictionary
            f = open("Lendersdict", "w")
            for Book, Name in Lenders_dict.items():
                f.write(f"{Book} :- {Name}\n")                             # Reformats Lenderdict.txt
            f.close()
            Books_list.append(self.book)                                   # Appends the book in the list
            with open("Bookslist", "a") as f:
                f.write(f"{self.book}\n")                                       # Appends the Book in Bookslist.txt
            print("\nThe book has been returned successfully\n")
        elif self.book in Books_list:
            print("\nThis book has not been borrowed right now\n")
        else:
            print("\nThis book is not the property of the Library\n")


# print(Books_list)
def Main():                                                              # Main function with all operations
    task_dict = {1: "Display Books in the Library",
                 2: "Add books in the library",
                 3: "Lend books",
                 4: "Return Books",
                 5: "Search books"}

    while True:
        for x, y in task_dict.items():
            print(x, y)
        n = int(input("\nEnter the key number corresponding to your task you wish to perform: "))
        if n == 1:
            Display()              # Displays the books
            break
        elif n == 2:
            AddBook()          # Adds the book
            break
        elif n == 3:
            manager = MyLibrary()  # manager is object of the class
            manager.Lend()              # Lends the book
            break
        elif n == 4:
            manager = MyLibrary()
            manager.Return()           # Returns the book
            break
        elif n == 5:
            Search()                       # Search the books
            break
        else:
            print("Please type a valid number in 1 to 5\n")


Main()                                  # Main execution of the program
while True:
    res = str(input("\nDo you want to perform the operation again(y/n): "))
    if res == "y":
        Main()                          # Re-execution of the program if response of the user is yes
    elif res == "n":
        print("\nHave a nice day")
        break
    else:
        print("Please enter a valid response\n")


""" 
Program by Aditya Abhijit Joshi
Third Year Btech Mechanical
"""
