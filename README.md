# naveen9755-SpringMVCSpringSecurityJNDIEncryptPassword

============================================================================================================

Need to change three files in tomcat For JNDI DataSource.
1- Server.xml
2- Web.xml
3- Context.xml

============================================================================================================
      

        ==============================================Server.xml==============================================

         <!--     <Realm className="org.apache.catalina.realm.LockOutRealm">
        This Realm uses the UserDatabase configured in the global JNDI
             resources under the key "UserDatabase".  Any edits
             that are performed against this UserDatabase are immediately
             available for use by the Realm. 
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm" resourceName="UserDatabase"/>
      </Realm> --> 

      Note: Search Realm and comment it as i do.




       =================================================Context.xml============================================    
                    Add it in  Context.xml

                           <Context>


	                            <Resource auth="Container" driverClassName="com.mysql.jdbc.Driver"
                                global="jdbc/naveen" maxActive="100" maxIdle="20" maxWait="10000"
                                minIdle="5" name="jdbc/naveen" password="N5aL8VxueHE=" type="javax.sql.DataSource" 
                                url="jdbc:mysql://localhost:3306/naveen" username="root"/> 
 


                           </Context>




      ==========================================================Web.xml==================================================                     Add it in Web.xml 

                            <resource-ref>

		                    <description>MySQL Datasource </description>
		                    <res-ref-name>java:comp/env/jdbc/naveen</res-ref-name>
		                    <res-type>javax.sql.DataSource</res-type>
		                    <res-auth>Container</res-auth>

	                        </resource-ref>


	 

	 =========================================================================================================================                        