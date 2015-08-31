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
      
        Not all types are supported by all databases and JDBC drivers. You will have to check your database and JDBC driver to          see
    
        if it supports the type you want to use. The DatabaseMetaData.supportsResultSetType(int type) method returns true or            false
    
        depending on whether the given type is supported or not. The DatabaseMetaData class is covered in a later text.
        
        When iterating the ResultSet you want to access the column values of each record. You do so by calling one or more of           the many getXXX() methods. You pass the name of the column to get the value of, to the many getXXX() methods：
        
        
            while(result.next()) {

                result.getString    ("name");
                result.getInt       ("age");
                result.getBigDecimal("coefficient");

            // etc.
            }
        
            while(result.next()) {

                result.getString    (1);
                result.getInt       (2);
                result.getBigDecimal(3);

            // etc.
            }
            
            
    If you do not know the index of a certain column you can find the index of that column using the                                        ResultSet.findColumn(String columnName) method
            int nameIndex   = result.findColumn("name");
            int ageIndex    = result.findColumn("age");
            int coeffIndex  = result.findColumn("coefficient");

            while(result.next()) {
                String name = result.getString(nameIndex);
                int age = result.getInt(ageIndex);
                BigDecimal coefficient = result.getBigDecimal (coeffIndex);
            }

      
  
There are four basic JDBC use cases around which most JDBC work evolves:

    1.Query the database (read data from it).
    2.Query the database meta data.
    3.Update the database.
    4.Perform transactions.
    

    
