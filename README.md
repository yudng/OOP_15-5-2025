# OOP_15-5-2025
import java.util.ArrayList;
import java.util.List;

class User {
    private String Usrname;
    private String Passwd;
    private String Role;
    public User(String Usrname, String Passwd, String Role){
        this.Usrname = Usrname;
        this.Passwd =  Passwd;
        this.Role = Role;
    }
    public String getUsrname(){
        return Usrname;
    }
    public String getPasswd(){
        return Passwd;
    }
    public String getRole(){
        return Role;
    }
    public String Infousr(){
        return "Username: " + Usrname + ", Password: " + Passwd + ", Role: " + Role;
    }

}

class Admin extends User {
    private List<User> userList;

    public Admin(String Usrname, String Passwd, String Role) {
        super(Usrname, Passwd, Role);
        this.userList = new ArrayList<>();
    }

    public void addUser(String Usrname, String Passwd, String Role) {
        User newUser = new User(Usrname, Passwd, Role); // Assuming User has a constructor
        userList.add(newUser);
        System.out.println("User added: " + Usrname);
    }

    public List<User> getUserList() {
        return userList;
    }

    public void removeUser(String Usrname){
        for (int i = 0; i < userList.size(); i++) {
            if (userList.get(i).getUsrname().equals(Usrname)) {
                userList.remove(i);
                System.out.println("User removed: " + Usrname);
                return;
            }
        }
        System.out.println("User not found: " + Usrname);
    }
    public void listUsers() {
        System.out.println("User List:");
        for (User user : userList) {
            System.out.println("Username: " + user.getUsrname() + "Password: " + user.getPasswd() + ", Role: " + user.getRole());
        }
    }
}

class Book {
    private String title;
    private String author;
    private String isbn;

    public Book(String title, String author, String isbn) {
        this.title = title;
        this.author = author;
        this.isbn = isbn;
    }
    
    public String getTitle() {
        return title;
    }

    public String getAuthor() {
        return author;
    }

    public String getIsbn() {
        return isbn;
    }
    public String Infobook() {
        return "Title: " + title + ", Author: " + author + ", ISBN: " + isbn;
    }
}

class Librarian extends User {
    private List<Book> bookList;

    public Librarian(String Usrname, String Passwd, String Role) {
        super(Usrname, Passwd, Role);
        this.bookList = new ArrayList<>();
    }

    public void addBook(String title, String author, String isbn) {
        Book newBook = new Book(title, author, isbn); // Assuming Book has a constructor
        bookList.add(newBook);
        System.out.println("Book added: " + title);
    }

    public List<Book> getBookList() {
        return bookList;
    }

    public void removeBook(String isbn) {
        for (int i = 0; i < bookList.size(); i++) {
            if (bookList.get(i).getIsbn().equals(isbn)) { // Assuming Book has a getIsbn() method
                bookList.remove(i);
                System.out.println("Book removed: " + isbn);
                return;
            }
        }
        System.out.println("Book not found: " + isbn);
    }

    public void listBooks() {
        System.out.println("Book List:");
        for (Book book : bookList) {
            System.out.println("Title: " + book.getTitle() + ", Author: " + book.getAuthor() + ", ISBN: " + book.getIsbn());
        }
    }
}

public class Reader extends User {
    private List<Book> borrowedBooks;

    public Reader(String Usrname, String Passwd, String Role) {
        super(Usrname, Passwd, Role);
        this.borrowedBooks = new ArrayList<>();
    }

    public void borrowBook(Book book) {
        if (book == null) {
            System.out.println("Invalid book.");
            return;
        }
        borrowedBooks.add(book);
        System.out.println("Book borrowed: " + book.getTitle());
    }

    // Return a book
    public void returnBook(String isbn) {
        for (int i = 0; i < borrowedBooks.size(); i++) {
            if (borrowedBooks.get(i).getIsbn().equals(isbn)) {
                System.out.println("Book returned: " + borrowedBooks.get(i).getTitle());
                borrowedBooks.remove(i);
                return;
            }
        }
        System.out.println("Book not found in borrowed list: " + isbn);
    }

    public void listBorrowedBooks() {
        System.out.println("Borrowed Books:");
        for (Book book : borrowedBooks) {
            System.out.println("Title: " + book.getTitle() + ", Author: " + book.getAuthor() + ", ISBN: " + book.getIsbn());
        }
    }
}
