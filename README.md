# Library-Management-System
class Book:
    def __init__(self, book_id, title, author, available=True):
        self.book_id = book_id
        self.title = title
        self.author = author
        self.available = available

    def __str__(self):
        return f"{self.title} by {self.author} (ID: {self.book_id})"

class Library:
    def __init__(self):
        self.books = []

    def add_book(self, book):
        self.books.append(book)

    def display_books(self):
        for book in self.books:
            print(book)

class Member:
    def __init__(self, member_id, name):
        self.member_id = member_id
        self.name = name
        self.borrowed_books = []

    def borrow_book(self, book):
        if book.available:
            book.available = False
            self.borrowed_books.append(book)
            return True
        else:
            print(f"Book '{book.title}' is not available for borrowing.")
            return False

    def return_book(self, book):
        if book in self.borrowed_books:
            book.available = True
            self.borrowed_books.remove(book)
            print(f"Book '{book.title}' has been returned.")
        else:
            print(f"You didn't borrow '{book.title}'. Check the book or member ID.")

def main():
    library = Library()

    while True:
        add_book_choice = input("Do you want to add a book to the library? (yes/no): ").lower()
        if add_book_choice != 'yes':
            break
        book_id = int(input("Enter Book ID: "))
        title = input("Enter Book Title: ")
        author = input("Enter Author: ")
        book = Book(book_id, title, author)
        library.add_book(book)
        print(f"Book '{title}' has been added to the library.\n")
    print("\nAvailable Books in the Library:")
    
    library.display_books()
    while True:
        member_name = input("\nEnter your name (or 'exit' to end): ")
        if member_name.lower() == 'exit':
            break
        member_id = int(input("Enter your Member ID: "))
        member = Member(member_id, member_name)
        while True:
            borrow_book_choice = input("\nDo you want to borrow a book? (yes/no): ").lower()
            if borrow_book_choice != 'yes':
                break

            book_id_to_borrow = int(input("Enter the Book ID you want to borrow: "))
            selected_book = next((book for book in library.books if book.book_id == book_id_to_borrow), None)

            if selected_book:
                member.borrow_book(selected_book)
                print(f"Book '{selected_book.title}' has been borrowed by {member.name}.")
            else:
                print(f"Book with ID {book_id_to_borrow} not found in the library.")

        print(f"\nBooks Borrowed by {member.name} (ID: {member.member_id}):")
        for borrowed_book in member.borrowed_books:
            print(f"   {borrowed_book}")
        while True:
            return_book_choice = input("\nDo you want to return a book? (yes/no): ").lower()
            if return_book_choice != 'yes':
                break

            book_id_to_return = int(input("Enter the Book ID you want to return: "))
            selected_book = next((book for book in member.borrowed_books if book.book_id == book_id_to_return), None)

            if selected_book:
                member.return_book(selected_book)
            else:
                print(f"You didn't borrow a book with ID {book_id_to_return}. Check the book or member ID.")

if __name__ == "__main__":
    main()
