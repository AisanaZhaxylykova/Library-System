# Library-System
import java.sql.*;

public class LibrarySystem {
    static final String JDBC_URL = "jdbc:postgresql://localhost:5432/LS";
    static final String USERNAME = "Aisana";
    static final String PASSWORD = "qwerty123";
    public static void main(String[] args) {
        try (Connection connection = DriverManager.getConnection(JDBC_URL, USERNAME, PASSWORD)) {
            Book book = new Book("Pride and Prejudice", "Jane Austen", "1234567890");
            createBook(connection, book);
            ResultSet resultSet = getAllBooks(connection);
            while (resultSet.next()) {
                String title = resultSet.getString("title");
                String author = resultSet.getString("author");
                String isbn = resultSet.getString("isbn");
                System.out.println("Book: " + title + ", Author: " + author + ", ISBN: " + isbn);
            }
        } catch (SQLException e) {
            e.printStackTrace();
        }
    }
    static void createBook(Connection connection, Book book) throws SQLException {
        String sql = "INSERT INTO books (title, author, isbn) VALUES (?, ?, ?)";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setString(1, book.getTitle());
            statement.setString(2, book.getAuthor());
            statement.setString(3, book.getIsbn());
            statement.executeUpdate();
        }
    }
    static ResultSet getAllBooks(Connection connection) throws SQLException {
        String sql = "SELECT * FROM books";
        Statement statement = connection.createStatement();
        return statement.executeQuery(sql);
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
    static void updateBook(Connection connection, int bookId, Book newBookDetails) throws SQLException {
        String sql = "UPDATE books SET title = ?, author = ?, isbn = ? WHERE id = ?";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setString(1, newBookDetails.getTitle());
            statement.setString(2, newBookDetails.getAuthor());
            statement.setString(3, newBookDetails.getIsbn());
            statement.setInt(4, bookId);
            statement.executeUpdate();
        }
    }
    static void deleteBook(Connection connection, int bookId) throws SQLException {
        String sql = "DELETE FROM books WHERE id = ?";
        try (PreparedStatement statement = connection.prepareStatement(sql)) {
            statement.setInt(1, bookId);
            statement.executeUpdate();
        }
    }
}
