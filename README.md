# JDBC

The JDBC API consists of the following core parts:

    1.JDBC Drivers: Class.forName("driverClassName");
    
    2.Connections:
      tring url      = "";   //database specific url.
      String user     = "";
      String password = "";
      Connection connection =DriverManager.getConnection(url, user, password);
      
    3.Statements
      Statement statement = connection.createStatement();
      
    4.Result Sets
      String sql = "select * from people";
      ResultSet result = statement.executeQuery(sql);
      
      while(result.next()) {
        String name = result.getString("name");
        long   age  = result.getLong  ("age");
      }
      
      result.close();
      statement.close();
      
    Not all types are supported by all databases and JDBC drivers. You will have to check your database and JDBC driver to see
    
    if it supports the type you want to use. The DatabaseMetaData.supportsResultSetType(int type) method returns true or false
    
    depending on whether the given type is supported or not. The DatabaseMetaData class is covered in a later text.


      
  
There are four basic JDBC use cases around which most JDBC work evolves:

    1.Query the database (read data from it).
    2.Query the database meta data.
    3.Update the database.
    4.Perform transactions.
    
