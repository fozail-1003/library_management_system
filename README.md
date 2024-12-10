# library_management_system
class Book:
    def __init__(self, book_id, title, author):
        self.book_id = book_id
        self.title = title
        self.author = author

class Library:
    def __init__(self):
        self.catalog = []
        self.borrowed_books = []
        self.recently_returned_books = []

    def add_book(self, book):
        self.catalog.append(book)
        print(f"Book '{book.title}' added to the catalog.")

    def display_catalog(self):
        if not self.catalog:
            print("No books in the catalog.")
        else:
            print("\nCatalog of Books:")
            for book in self.catalog:
                print(f"ID: {book.book_id}, Title: {book.title}, Author: {book.author}")

    def borrow_book(self, book_id):
        for book in self.catalog:
            if book.book_id == book_id:
                self.catalog.remove(book)
                self.borrowed_books.append(book)
                print(f"Book '{book.title}' borrowed.")
                return
        print("Book not found!")

    def return_book(self, book_id):
        for book in self.borrowed_books:
            if book.book_id == book_id:
                self.borrowed_books.remove(book)
                self.catalog.append(book)
                self.recently_returned_books.append(book)
                print(f"Book '{book.title}' returned.")
                return
        print("Book not found!")

    def display_recently_returned_books(self):
        if not self.recently_returned_books:
            print("No recently returned books.")
        else:
            print("\nRecently Returned Books:")
            for book in self.recently_returned_books:
                print(f"ID: {book.book_id}, Title: {book.title}, Author: {book.author}")

    def search_for_book(self, keyword):
        found_books = [book for book in self.catalog if keyword.lower() in book.title.lower() or keyword.lower() in book.author.lower()]
        if found_books:
            print("\nFound Books:")
            for book in found_books:
                print(f"ID: {book.book_id}, Title: {book.title}, Author: {book.author}")
        else:
            print("No books found with that keyword.")

    def dfs_search(self, keyword):
        print("\nPerforming Depth First Search...")
        visited = set()

        def dfs(index):
            if index < 0 or index >= len(self.catalog) or index in visited:
                return []
            visited.add(index)
            book = self.catalog[index]
            results = []
            if keyword.lower() in book.title.lower() or keyword.lower() in book.author.lower():
                results.append(book)
            # Recursive DFS: Explore the next book in the catalog
            results += dfs(index + 1)
            return results

        results = dfs(0)
        if results:
            print("\nDFS Found Books:")
            for book in results:
                print(f"ID: {book.book_id}, Title: {book.title}, Author: {book.author}")
        else:
            print("No books found with that keyword.")

    def insertion_sort(self):
        for i in range(1, len(self.catalog)):
            key = self.catalog[i]
            j = i - 1
            while j >= 0 and self.catalog[j].title > key.title:
                self.catalog[j + 1] = self.catalog[j]
                j -= 1
            self.catalog[j + 1] = key
        print("Catalog sorted using Insertion Sort.")

    def bubble_sort(self):
        n = len(self.catalog)
        for i in range(n):
            for j in range(0, n-i-1):
                if self.catalog[j].title > self.catalog[j+1].title:
                    self.catalog[j], self.catalog[j+1] = self.catalog[j+1], self.catalog[j]
        print("Catalog sorted using Bubble Sort.")

    def menu(self):
        while True:
            print("\nLibrary Management System")
            print("1. Add New Book")
            print("2. Display Catalog")
            print("3. Borrow Book")
            print("4. Return Book")
            print("5. Display Recently Returned Books")
            print("6. Search for a Book")
            print("7. Sort Catalog (Insertion Sort)")
            print("8. Sort Catalog (Bubble Sort)")
            print("9. Search for a Book (DFS)")
            print("10. Exit")
            choice = input("Enter your choice: ")

            if choice == '1':
                book_id = input("Enter book ID: ")
                title = input("Enter book title: ")
                author = input("Enter author name: ")
                book = Book(book_id, title, author)
                self.add_book(book)
            elif choice == '2':
                self.display_catalog()
            elif choice == '3':
                book_id = input("Enter book ID to borrow: ")
                self.borrow_book(book_id)
            elif choice == '4':
                book_id = input("Enter book ID to return: ")
                self.return_book(book_id)
            elif choice == '5':
                self.display_recently_returned_books()
            elif choice == '6':
                keyword = input("Enter title or author to search: ")
                self.search_for_book(keyword)
            elif choice == '7':
                self.insertion_sort()
            elif choice == '8':
                self.bubble_sort()
            elif choice == '9':
                keyword = input("Enter title or author to search (DFS): ")
                self.dfs_search(keyword)
            elif choice == '10':
                print("Exiting the system.")
                break
            else:
                print("Invalid choice. Please try again.")

if __name__ == "__main__":
    library = Library()
    library.menu()
