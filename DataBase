package pong2;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.logging.Level;
import java.util.logging.Logger;
import javax.swing.JOptionPane;

/**
 *
 * @author Mark
 */
public class DataBase {
    private String connection;
    Connection dbConnection;
    
    public DataBase() {
        try {
            Class.forName("com.microsoft.sqlserver.jdbc.SQLServerDriver");
            System.out.println("Driver Successfully Loaded!");
            String driver = "jdbc:sqlserver:";
            String url = "//lcmdb.cbjmpwcdjfmq.us-east-1.rds.amazonaws.com:";
            String port = "1433";
            String username = "DS7";
            String password = "Touro123";
            String database = "DS7";
            connection = driver + url + port + ";databaseName=" + database + ";user=" + username + ";password=" + password + ";";
        } catch (ClassNotFoundException ex) {
            Logger.getLogger(DataBase.class.getName()).log(Level.SEVERE, null, ex);
        }
    }
    
    public void updateStandings(String name, int score) throws SQLException {
        try {
            dbConnection = DriverManager.getConnection(connection);
                System.out.println("Connected to Database!");
                PreparedStatement state = dbConnection.prepareStatement ("INSERT INTO highScores" + "(Name, Score) VALUES" + "(?, ?)");
                state.setString(1, name);
                state.setInt(2, score);
                state.executeUpdate();
                System.out.println("Query Executed Successfully!");
        } catch (SQLException ex) {
                 Logger.getLogger(DataBase.class.getName()).log(Level.SEVERE, null, ex);
             }
    }
    
    public ResultSet getHighScores() throws SQLException {
         ResultSet rs = null;
         try {
             dbConnection = DriverManager.getConnection(connection);
                 
                System.out.println("Connected to Database!");
             try {
                PreparedStatement state = dbConnection.prepareStatement("SELECT TOP 10 Score, Name FROM HighScores ORDER BY Score DESC");
                System.out.println("Query Executed Successfully!");
                rs = state.executeQuery();
             } catch (SQLException ex) {
                 Logger.getLogger(DataBase.class.getName()).log(Level.SEVERE, null, ex);
             }
        } catch (SQLException ex) {
                 Logger.getLogger(DataBase.class.getName()).log(Level.SEVERE, null, ex);
             }
        return rs;
    }
    
    public void checkScores(int score, ResultSet rs) throws SQLException {
            rs.last();
            int low = rs.getInt("Score");
            if (score >= low) {
                String name = promptUserInfo();
                updateStandings(name, score);
                rs = getHighScores();
                displayHighScores(rs);
            }      
    }
    
    public void displayHighScores(ResultSet rs) {
        try {
            String highScores = "Top 10 high scores" + "\n";
            while (rs.next()) {
                highScores += rs.getString("Name") + ": ";
                highScores += rs.getInt("Score") + "\n";
            }
            JOptionPane.showMessageDialog(null, highScores);
        } catch (SQLException ex) {
            Logger.getLogger(DataBase.class.getName()).log(Level.SEVERE, null, ex);
        }
    }
    
    public void checkStandings(int score) throws SQLException {
        int i = 0;
        ResultSet rs = getHighScores();
        while (rs.next()) {
            i++;
        }
        if (i < 10) {
            String winnerName = promptUserInfo();
            try {
                updateStandings(winnerName, score);
                ResultSet highScores = getHighScores();
                displayHighScores(highScores);
            } catch (SQLException ex) {
                Logger.getLogger(DataBase.class.getName()).log(Level.SEVERE, null, ex);
            }
        }
        else {
            try {
                checkScores(score, rs);
            } catch (SQLException ex) {
                Logger.getLogger(DataBase.class.getName()).log(Level.SEVERE, null, ex);
            }
        }
        dbConnection.close();
    }
    
    public static String promptUserInfo() {
        return JOptionPane.showInputDialog("Congratulations!!! you are in the top 10 scores, please enter your name");
    } 
}
