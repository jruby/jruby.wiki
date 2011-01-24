It is possible to use JDBC directly from within JRuby. A simple function using the MySQL JDBC driver to perform a SELECT is shown below:

    def vegetableFinder(vegetable)
    	# This function takes a hashmap of vegetables and attempts to 
    	# find them from our grocery database. For each item found, we 
    	# call our 'makevegetablesoup' function.
    
    	# Load all required gems
    	require "rubygems"
    	require "jdbc/mysql"
    	require "java"
    	
    	begin
    		# Prep the connection
                    Java::com.mysql.jdbc.Driver
    		userurl = "jdbc:mysql://HOST/DATABASE"
                    connSelect = java.sql.DriverManager.get_connection(userurl, "USERNAME", "PASSWORD")
    		stmtSelect = connSelect.create_statement
    		
    		# Define the query
    		selectquery = "SELECT name, type, size, price
    					FROM vegetables
    					WHERE type = \"#{vegetable["type"]}\" 
    					AND size = \"#{vegetable["size"]}\";"
    
    		# Execute the query
    		rsS = stmtSelect.execute_query(selectquery)
    
    		# For each row returned do some stuff
    		while (rsS.next) do
    			veg = Hash.new
    			veg["vegname"] = rsS.getObject("name")
    			veg["vegtype"] = rsS.getObject("type")
    			veg["vegprice"] = rsS.getObject("size")
    			veg["vegsize"] = rsS.getObject("price")
    			makevegetablesoup(veg)
    		end
    	end
    	# Close off the connection
    	stmtSelect.close
            connSelect.close
    	return truth
    end

INSERTs and UPDATEs are much the same. Again, using the MySQL JDBC driver a simple insert is shown below:

    def groceryInsert(vegetable)
    	# Inserts a new vegetable type into the grocery database
    
    	# Load all required gems
    	require "rubygems"
    	require "jdbc/mysql"
    	require "java"
    	
    	begin
    		# Prep the connection
                    Java::com.mysql.jdbc.Driver
    		userurl = "jdbc:mysql://HOST/DATABASE"
    		connInsert = java.sql.DriverManager.get_connection(userurl, "USERNAME", "PASSWORD")
    		stmtInsert = connInsert.create_statement
    
    		# Do the insert
    		rsI = stmtInsert.execute_update("INSERT INTO vegetables										(name,type,size,price)
    						VALUES
    							   (
    							   '#{vegetable["name"]}',
    							   '#{vegetable["type"]}',
    							   '#{vegetable["size"]}',
    							   '#{vegetable["price"]}'
    							   );")
    		end
    	end
    	# Close off the connection
    	stmtInsert.close
            connInsert.close
    
    	# Return the number of records inserted
    	return rsI
    end

Remember to have the JDBC driver for your database in your CLASSPATH!
