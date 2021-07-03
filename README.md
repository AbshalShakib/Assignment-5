import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.SQLException;

public class ConnectionUtil{

  
 public static Connection getConnection() throws SQLException
 {
 Connection connection = null;
 try
 {
Class.forName("com.mysql.jdbc.Connection");
connection = DriverManager.getConnection("jdbc:mysql://192.168.216.250:3306/vijju","root",
"root");
 }
 catch (ClassNotFoundException e)
 {
 e.printStackTrace();
 }
 catch (SQLException e)
{
 e.printStackTrace();
 }
System.out.println("message for connection open"+connection);
 return connection; 

 }
 }
StudentDetails.java
public class StudentDetails
{
 private long st_id;
 private String st_name;
 private long st_mobile;
 public StudentDetails(long st_id,String st_name,long st_mobile)
 {
 this.st_id = st_id;
 this.st_name = st_name;
 this.st_mobile = st_mobile;
}
 public long getSt_id()
 {
 return st_id;
 }
 public void setSt_id(long st_id)
 {
 this.st_id = st_id;
 }
 public String getSt_name()
 {
 return st_name;
 }
 public void setSt_name(String st_name)
{ 

 this.st_name = st_name;
 }
 public long getSt_mobile()
{
 return st_mobile;
 }
 public void setSt_mobile(long st_mobile)
{
 this.st_mobile = st_mobile;
 }
}
Prog6.java
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;
public class Prog6
{
String createTableQuery = "create table test.student_details (st_namevarchar(50), st_mobile
numeric(10), st_id numeric(10))";
 public static void main(String[] args)
{ Connection conn = null;
 try
 {
 conn = ConnectionUtil.getConnection();
 Prog6 prog6 = new Prog6();
 System.out.println("Student_details table data before inserting:");
 prog6.retrieveData(conn); 

 StudentDetails st1 = new StudentDetails(1, "GNIT", 23232323);
 StudentDetails st2 = new StudentDetails(2, "GNEC", 24242424);
 StudentDetails st3 = new StudentDetails(3, "GNITC", 25252525);
 prog6.insertData(conn, st1);
 prog6.insertData(conn, st2);
 prog6.insertData(conn, st3);
 System.out.println("Student_details table data after inserting:");
 prog6.retrieveData(conn);
 prog6.deleteARow(conn,2);
 System.out.println("Student_details table data after deleting:");
 prog6.retrieveData(conn);
 prog6.modifyData(conn, 26262626, 3);
 System.out.println("Student_details table data after modifying:");
 prog6.retrieveData(conn);
 } catch (SQLException e)
 {
 e.printStackTrace();
 } finally
 {
 try {
 conn.close();
 } catch (SQLException e)
 {
 e.printStackTrace();
 }
 }
}
 private void modifyData(Connection conn, long st_mobile, long st_id) 

{
 String query = "update student_details set st_mobile=? wherest_id=?";
 try {
 PreparedStatement pstmt = conn.prepareStatement(query);
 pstmt.setLong(1, st_mobile);
 pstmt.setLong(2, st_id);
 int row = pstmt.executeUpdate();
 //System.out.println(row);
 } catch (SQLException e)
{ e.printStackTrace();
}
}
 private void deleteARow(Connection conn, long st_id)
 {
 String query = "delete from student_details where st_id=?" ;
 try
{
 PreparedStatement pstmt = conn.prepareStatement(query);
 pstmt.setLong(1, st_id);
 int row = pstmt.executeUpdate();
 //System.out.println(row);
 }
catch (SQLException e)
 {
 e.printStackTrace();
 }
 }
 private void insertData(Connection conn, StudentDetails details) 

 {
 String query = "insert into test.student_details values (?,?,?)";
 try
{
 PreparedStatement pstmt = conn.prepareStatement(query);
 pstmt.setString(1,details.getSt_name());
 pstmt.setLong(2, details.getSt_mobile());
 pstmt.setLong(3, details.getSt_id());
 int row = pstmt.executeUpdate();
 //System.out.println(row);
 }
catch (SQLException e)
{
 e.printStackTrace();
 }
}
 private void retrieveData(Connection conn)
 {
 try {
 String query = "select * from test.student_details";
 PreparedStatement pstmt = conn.prepareStatement(query);
 ResultSet rs = pstmt.executeQuery();
 System.out.println("ST_ID\tST_NAME\t\tST_MOBILE");
 while(rs.next())
{
 System.out.println(rs.getString("st_id")+"\t"+rs.getString("st_name")+"\t\t"+rs.getString(
"st_mobile"));
 } 

 }
catch (SQLException e)
 {
 e.printStackTrace();
 }
 }
 }
