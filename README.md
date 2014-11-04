Java
====

1104
package roseindia.net;

import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.Statement;

import javax.sql.RowSetEvent;
import javax.sql.RowSetListener;
import javax.sql.rowset.JdbcRowSet;

import com.sun.rowset.JdbcRowSetImpl;

public class JDBCRowSetExample {
	public static void main(String[] args) throws Exception {
		Connection connection = getMySqlConnection();
		System.out.println("Connection Done");
		Statement statement = connection.createStatement();
		JdbcRowSet jdbcRowSet;
		jdbcRowSet = new JdbcRowSetImpl(connection);
		jdbcRowSet.setType(ResultSet.TYPE_SCROLL_INSENSITIVE);
		String queryString = "SELECT * FROM student";
		jdbcRowSet.setCommand(queryString);
		jdbcRowSet.execute();
		jdbcRowSet.addRowSetListener(new ExampleListener());

		while (jdbcRowSet.next()) {
			// Generating cursor Moved event
			System.out.println("Roll No- " + jdbcRowSet.getString(1));
			System.out.println("name- " + jdbcRowSet.getString(2));
		}
		connection.close();
	}

	// My Sql connection method
	public static Connection getMySqlConnection() throws Exception {
		String driver = "com.mysql.jdbc.Driver";
		String url = "jdbc:mysql://localhost:3306/student";
		String username = "root";
		String password = "root";

		Class.forName(driver);
		Connection connection = DriverManager.getConnection(url, username,
				password);
		return connection;
	}

}

class ExampleListener implements RowSetListener {

	@Override
	public void cursorMoved(RowSetEvent event) {
		// TODO Auto-generated method stub
		System.out.println("Cursor Moved Listener");
		System.out.println(event.toString());
	}

	@Override
	public void rowChanged(RowSetEvent event) {
		// TODO Auto-generated method stub
		System.out.println("Cursor Changed Listener");
		System.out.println(event.toString());
	}

	@Override
	public void rowSetChanged(RowSetEvent event) {
		// TODO Auto-generated method stub
		System.out.println("RowSet changed Listener");
		System.out.println(event.toString());
	}
}
