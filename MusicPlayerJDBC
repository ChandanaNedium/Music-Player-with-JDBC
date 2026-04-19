import java.sql.*;
import java.util.Scanner;

public class MusicPlayerJDBC {

    static final String URL = "jdbc:mysql://localhost:3306/musicdb";
    static final String USER = "root";
    static final String PASS = ""; // XAMPP default = empty

    static Scanner sc = new Scanner(System.in);

    public static void main(String[] args) {

        try {
            Class.forName("com.mysql.cj.jdbc.Driver");
            Connection con = DriverManager.getConnection(URL, USER, PASS);

            System.out.println("Connected successfully!");

            createTables(con);

            while (true) {
                System.out.println("\n--- MUSIC PLAYER MENU ---");
                System.out.println("1. Add Song");
                System.out.println("2. View Songs");
                System.out.println("3. Create Playlist");
                System.out.println("4. View Playlists");
                System.out.println("5. Add Song to Playlist");
                System.out.println("6. View Playlist Songs");
                System.out.println("7. Exit");

                int choice = sc.nextInt();

                switch (choice) {
                    case 1: addSong(con); break;
                    case 2: viewSongs(con); break;
                    case 3: createPlaylist(con); break;
                    case 4: viewPlaylists(con); break;
                    case 5: addSongToPlaylist(con); break;
                    case 6: viewPlaylistSongs(con); break;
                    case 7:
                        System.out.println("Exiting...");
                        return;
                    default:
                        System.out.println("Invalid choice!");
                }
            }

        } catch (Exception e) {
            System.out.println(e);
        }
    }

    // 🔧 CREATE TABLES
    static void createTables(Connection con) throws Exception {
        Statement st = con.createStatement();

        st.executeUpdate("CREATE TABLE IF NOT EXISTS songs (" +
                "id INT AUTO_INCREMENT PRIMARY KEY, " +
                "title VARCHAR(100), " +
                "artist VARCHAR(100))");

        st.executeUpdate("CREATE TABLE IF NOT EXISTS playlists (" +
                "id INT AUTO_INCREMENT PRIMARY KEY, " +
                "name VARCHAR(100))");

        st.executeUpdate("CREATE TABLE IF NOT EXISTS playlist_songs (" +
                "playlist_id INT, " +
                "song_id INT)");

        System.out.println("Tables ready!");
    }

    // 🎵 ADD SONG
    static void addSong(Connection con) throws Exception {
        sc.nextLine();
        System.out.print("Enter song title: ");
        String title = sc.nextLine();

        System.out.print("Enter artist: ");
        String artist = sc.nextLine();

        PreparedStatement ps = con.prepareStatement(
                "INSERT INTO songs(title, artist) VALUES (?, ?)");
        ps.setString(1, title);
        ps.setString(2, artist);

        ps.executeUpdate();
        System.out.println("Song added!");
    }

    // 🎵 VIEW SONGS
    static void viewSongs(Connection con) throws Exception {
        Statement st = con.createStatement();
        ResultSet rs = st.executeQuery("SELECT * FROM songs");

        System.out.println("\n--- SONG LIST ---");
        while (rs.next()) {
            System.out.println(rs.getInt("id") + ". "
                    + rs.getString("title") + " - "
                    + rs.getString("artist"));
        }
    }

    // 📂 CREATE PLAYLIST
    static void createPlaylist(Connection con) throws Exception {
        sc.nextLine();
        System.out.print("Enter playlist name: ");
        String name = sc.nextLine();

        PreparedStatement ps = con.prepareStatement(
                "INSERT INTO playlists(name) VALUES (?)");
        ps.setString(1, name);

        ps.executeUpdate();
        System.out.println("Playlist created!");
    }

    // 📂 VIEW PLAYLISTS
    static void viewPlaylists(Connection con) throws Exception {
        Statement st = con.createStatement();
        ResultSet rs = st.executeQuery("SELECT * FROM playlists");

        System.out.println("\n--- PLAYLISTS ---");
        while (rs.next()) {
            System.out.println(rs.getInt("id") + ". "
                    + rs.getString("name"));
        }
    }

    // ➕ ADD SONG TO PLAYLIST
    static void addSongToPlaylist(Connection con) throws Exception {
        System.out.print("Enter Playlist ID: ");
        int pid = sc.nextInt();

        System.out.print("Enter Song ID: ");
        int sid = sc.nextInt();

        PreparedStatement ps = con.prepareStatement(
                "INSERT INTO playlist_songs VALUES (?, ?)");
        ps.setInt(1, pid);
        ps.setInt(2, sid);

        ps.executeUpdate();
        System.out.println("Song added to playlist!");
    }

    // 🎧 VIEW PLAYLIST SONGS
    static void viewPlaylistSongs(Connection con) throws Exception {
        System.out.print("Enter Playlist ID: ");
        int pid = sc.nextInt();

        String query = "SELECT s.id, s.title, s.artist FROM songs s " +
                "JOIN playlist_songs ps ON s.id = ps.song_id " +
                "WHERE ps.playlist_id = ?";

        PreparedStatement ps = con.prepareStatement(query);
        ps.setInt(1, pid);

        ResultSet rs = ps.executeQuery();

        System.out.println("\n--- PLAYLIST SONGS ---");
        while (rs.next()) {
            System.out.println(rs.getInt("id") + ". "
                    + rs.getString("title") + " - "
                    + rs.getString("artist"));
        }
    }
}
