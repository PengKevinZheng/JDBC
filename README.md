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

attributte of resultset: 

    1.Type
    
        ResultSet.TYPE_FORWARD_ONLY: TYPE_FORWARD_ONLY means that the ResultSet can only be navigated forward. That is,               you can only move from row 1, to row 2, to row 3 etc. You cannot move backwards in the ResultSet.
        
        ResultSet.TYPE_SCROLL_INSENSITIVE: TYPE_SCROLL_INSENSITIVE means that the ResultSet can be navigated (scrolled)               both forward and backwards. You can also jump to a position relative to the current position, or jump to an                   absolute position. The ResultSet is insensitive to changes in the underlying data source while the ResultSet is               open. That is, if a record in the ResultSet is changed in the database by another thread or process, it will not              be reflected in already opened ResulsSet's of this type.
        
        ResultSet.TYPE_SCROLL_SENSITIVE:TYPE_SCROLL_SENSITIVE means that the ResultSet can be navigated (scrolled) both               forward and backwards. You can also jump to a position relative to the current position, or jump to an absolute               position. The ResultSet is sensitive to changes in the underlying data source while the ResultSet is open. That               is, if a record in the ResultSet is changed in the database by another thread or process, it will be reflected in             already opened ResulsSet's of this type.

    2.Concurrency
            
        CONCUR_READ_ONLY means that the ResultSet can only be read.

        CONCUR_UPDATABLE means that the ResultSet can be both read and updated.


    3.Holdability
    
        The CLOSE_CURSORS_OVER_COMMIT holdability means that all ResultSet instances are closed when connection.commit()             method is called on the connection that created the ResultSet.

        The HOLD_CURSORS_OVER_COMMIT holdability means that the ResultSet is kept open when the connection.commit() method is         called on the connection that created the ResultSet.

        The HOLD_CURSORS_OVER_COMMIT holdability might be useful if you use the ResultSet to update values in the database.          Thus, you can open a ResultSet, update rows in it, call connection.commit() and still keep the same ResultSet open           for future transactions on the same rows.
  
There are four basic JDBC use cases around which most JDBC work evolves:

    1.Query the database (read data from it).
    2.Query the database meta data.
    3.Update the database.
    4.Perform transactions.
    
The PreparedStatement's primary features are:
    
    Easy to insert parameters into the SQL statement.
    Easy to reuse the PreparedStatement with new parameters.
    May increase performance of executed statements.
    Enables easier batch updates.
    
Batch updates:

    A batch update is a batch of updates grouped together, and sent to the database in one "batch", rather than sending the
    
    updates one by one.
    
    Sending a batch of updates to the database in one go, is faster than sending them one by one, waiting for each one to
    
    finish. There is less network traffic involved in sending one batch of updates (only 1 round trip), and the database
    
    might be able to execute some of the updates in parallel. The speed up compared to executing the updates one by one, can
    
    be quite big.
    
    There are two ways to execute batch updates:

        Using a Statement and addBatch();
        Using a PreparedStatement and addBatch();
        
How to make a transaction in JDBC?

    You start a transaction by this invocation:

    connection.setAutoCommit(false);
    
    Now you can continue to perform database queries and updates. All these actions are part of the transaction.

    If any action attempted within the transaction fails, you should rollback the transaction. This is done like this:
    connection.rollback();
    
    If all actions succeed, you should commit the transaction. Committing the transaction makes the actions permanent in the     database. Once committed, there is no going back. Committing the transaction is done like this:

    connection.commit();

    

    
