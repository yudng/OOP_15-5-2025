cái này là để tạm thoi chứ có chỗ nào gửi coi chung đâu
import java.io.*;
import java.util.ArrayList;
import java.util.List;

class User {
    private String Usrname;
    private String Passwd;
    private String Role;

    public User(String Usrname, String Passwd, String Role) {
        this.Usrname = Usrname;
        this.Passwd = Passwd;
        this.Role = Role;
    }

    public String getUsrname() {
        return Usrname;
    }

    public String getPasswd() {
        return Passwd;
    }

    public String getRole() {
        return Role;
    }

    public String Infousr() {
        return "Username: " + Usrname + ", Password: " + Passwd + ", Role: " + Role;
    }

    // Save a single user to a file
    public void saveToFile(String filePath) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filePath, true))) {
            writer.write(Usrname + "," + Passwd + "," + Role);
            writer.newLine();
            System.out.println("User saved: " + Usrname);
        } catch (IOException e) {
            System.out.println("Error saving user to file: " + e.getMessage());
        }
    }

    // Save a list of users to a file
    public static void saveUsersToFile(String filePath, List<User> users) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filePath))) {
            for (User user : users) {
                writer.write(user.getUsrname() + "," + user.getPasswd() + "," + user.getRole());
                writer.newLine();
            }
            System.out.println("All users saved to file: " + filePath);
        } catch (IOException e) {
            System.out.println("Error saving users to file: " + e.getMessage());
        }
    }

    // Load users from a file
    public static List<User> loadFromFile(String filePath) {
        List<User> users = new ArrayList<>();
        try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = reader.readLine()) != null) {
                String[] parts = line.split(",");
                if (parts.length == 3) {
                    users.add(new User(parts[0], parts[1], parts[2]));
                }
            }
            System.out.println("Users loaded from file: " + filePath);
        } catch (IOException e) {
            System.out.println("Error loading users from file: " + e.getMessage());
        }
        return users;
    }
}

import java.io.*;
import java.util.ArrayList;
import java.util.List;

class Admin extends User {
    private List<User> userList;

    public Admin(String Usrname, String Passwd, String Role) {
        super(Usrname, Passwd, Role);
        this.userList = new ArrayList<>();
    }

    // Add a new user
    public void addUser(String Usrname, String Passwd, String Role) {
        User newUser = new User(Usrname, Passwd, Role);
        userList.add(newUser);
        System.out.println("User added: " + Usrname);
    }

    // Get the list of users
    public List<User> getUserList() {
        return userList;
    }

    // Remove a user by username
    public void removeUser(String Usrname) {
        for (int i = 0; i < userList.size(); i++) {
            if (userList.get(i).getUsrname().equals(Usrname)) {
                userList.remove(i);
                System.out.println("User removed: " + Usrname);
                return;
            }
        }
        System.out.println("User not found: " + Usrname);
    }

    // List all users
    public void listUsers() {
        System.out.println("User List:");
        for (User user : userList) {
            System.out.println("Username: " + user.getUsrname() + ", Password: " + user.getPasswd() + ", Role: " + user.getRole());
        }
    }

    // Save user list to a file
    public void saveUsersToFile(String filePath) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filePath))) {
            for (User user : userList) {
                writer.write(user.getUsrname() + "," + user.getPasswd() + "," + user.getRole());
                writer.newLine();
            }
            System.out.println("User list saved to file: " + filePath);
        } catch (IOException e) {
            System.out.println("Error saving user list to file: " + e.getMessage());
        }
    }

    // Load user list from a file
    public void loadUsersFromFile(String filePath) {
        userList.clear(); // Clear the current list before loading
        try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = reader.readLine()) != null) {
                String[] parts = line.split(",");
                if (parts.length == 3) {
                    userList.add(new User(parts[0], parts[1], parts[2]));
                }
            }
            System.out.println("User list loaded from file: " + filePath);
        } catch (IOException e) {
            System.out.println("Error loading user list from file: " + e.getMessage());
        }
    }
}

import java.io.*;
import java.util.ArrayList;
import java.util.List;

class Librarian extends User {
    private List<Book> bookList;

    public Librarian(String Usrname, String Passwd, String Role) {
        super(Usrname, Passwd, Role);
        this.bookList = new ArrayList<>();
    }

    // Add a new book
    public void addBook(String title, String author, String genre, String isbn, boolean isAvailable) {
        Book newBook = new Book(title, author, genre, isbn, isAvailable);
        bookList.add(newBook);
        System.out.println("Book added: " + title);
    }

    // Get the list of books
    public List<Book> getBookList() {
        return bookList;
    }

    // Remove a book by ISBN
    public void removeBook(String isbn) {
        for (int i = 0; i < bookList.size(); i++) {
            if (bookList.get(i).getIsbn().equals(isbn)) {
                System.out.println("Book removed: " + bookList.get(i).getTitle());
                bookList.remove(i);
                return;
            }
        }
        System.out.println("Book not found: " + isbn);
    }

    // List all books
    public void listBooks() {
        System.out.println("Book List:");
        for (Book book : bookList) {
            System.out.println(book);
        }
    }

    // Save book list to a file
    public void saveBooksToFile(String filePath) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filePath))) {
            for (Book book : bookList) {
                writer.write(book.toFileFormat());
                writer.newLine();
            }
            System.out.println("Book list saved to file: " + filePath);
        } catch (IOException e) {
            System.out.println("Error saving book list to file: " + e.getMessage());
        }
    }

    // Load book list from a file
    public void loadBooksFromFile(String filePath) {
        bookList.clear(); // Clear the current list before loading
        try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = reader.readLine()) != null) {
                Book book = Book.fromFileFormat(line);
                if (book != null) {
                    bookList.add(book);
                }
            }
            System.out.println("Book list loaded from file: " + filePath);
        } catch (IOException e) {
            System.out.println("Error loading book list from file: " + e.getMessage());
        }
    }
}

import java.io.*;
import java.util.ArrayList;
import java.util.List;

public class Reader extends User {
    private List<Book> borrowedBooks;

    public Reader(String Usrname, String Passwd, String Role) {
        super(Usrname, Passwd, Role);
        this.borrowedBooks = new ArrayList<>();
    }

    // Borrow a book
    public void borrowBook(Book book) {
        if (book == null) {
            System.out.println("Invalid book.");
            return;
        }
        if (!book.isAvailable()) {
            System.out.println("Book is not available for borrowing: " + book.getTitle());
            return;
        }
        borrowedBooks.add(book);
        book.setAvailable(false); // Mark the book as unavailable
        System.out.println("Book borrowed: " + book.getTitle());
    }

    // Return a book
    public void returnBook(String isbn) {
        for (int i = 0; i < borrowedBooks.size(); i++) {
            if (borrowedBooks.get(i).getIsbn().equals(isbn)) {
                Book returnedBook = borrowedBooks.remove(i);
                returnedBook.setAvailable(true); // Mark the book as available
                System.out.println("Book returned: " + returnedBook.getTitle());
                return;
            }
        }
        System.out.println("Book not found in borrowed list: " + isbn);
    }

    // List borrowed books
    public void listBorrowedBooks() {
        System.out.println("Borrowed Books:");
        for (Book book : borrowedBooks) {
            System.out.println("Title: " + book.getTitle() + ", Author: " + book.getAuthor() + ", ISBN: " + book.getIsbn());
        }
    }

    // Save borrowed books to a file
    public void saveBorrowedBooksToFile(String filePath) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filePath))) {
            for (Book book : borrowedBooks) {
                writer.write(book.toFileFormat());
                writer.newLine();
            }
            System.out.println("Borrowed books saved to file: " + filePath);
        } catch (IOException e) {
            System.out.println("Error saving borrowed books to file: " + e.getMessage());
        }
    }

    // Load borrowed books from a file
    public void loadBorrowedBooksFromFile(String filePath) {
        borrowedBooks.clear(); // Clear the current list before loading
        try (BufferedReader reader = new BufferedReader(new FileReader(filePath))) {
            String line;
            while ((line = reader.readLine()) != null) {
                Book book = Book.fromFileFormat(line);
                if (book != null) {
                    borrowedBooks.add(book);
                }
            }
            System.out.println("Borrowed books loaded from file: " + filePath);
        } catch (IOException e) {
            System.out.println("Error loading borrowed books from file: " + e.getMessage());
        }
    }
}
