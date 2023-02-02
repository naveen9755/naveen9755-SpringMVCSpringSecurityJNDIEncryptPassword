# naveen9755-SpringMVCSpringSecurityJNDIEncryptPassword

============================================================================================================

Need to change three files in tomcat For JNDI DataSource.
1- Server.xml
2- Web.xml
3- Context.xml
4- First User:  sam , Password: abc125
5- Run the all Query Which is mentioned below. 
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
	 
	 
	 /*All User's gets stored in APP_USER table*/
	 
create table APP_USER (
   id BIGINT NOT NULL AUTO_INCREMENT,
   sso_id VARCHAR(30) NOT NULL,
   password VARCHAR(100) NOT NULL,
   first_name VARCHAR(30) NOT NULL,
   last_name  VARCHAR(30) NOT NULL,
   email VARCHAR(30) NOT NULL,
   PRIMARY KEY (id),
   UNIQUE (sso_id)
);
   
/* USER_PROFILE table contains all possible roles */ 
create table USER_PROFILE(
   id BIGINT NOT NULL AUTO_INCREMENT,
   type VARCHAR(30) NOT NULL,
   PRIMARY KEY (id),
   UNIQUE (type)
);
   
/* JOIN TABLE for MANY-TO-MANY relationship*/  
CREATE TABLE APP_USER_USER_PROFILE (
    user_id BIGINT NOT NULL,
    user_profile_id BIGINT NOT NULL,
    PRIMARY KEY (user_id, user_profile_id),
    CONSTRAINT FK_APP_USER FOREIGN KEY (user_id) REFERENCES APP_USER (id),
    CONSTRAINT FK_USER_PROFILE FOREIGN KEY (user_profile_id) REFERENCES USER_PROFILE (id)
);
  
/* Populate USER_PROFILE Table */
INSERT INTO USER_PROFILE(type)
VALUES ('USER');
  
INSERT INTO USER_PROFILE(type)
VALUES ('ADMIN');
  
INSERT INTO USER_PROFILE(type)
VALUES ('DBA');
  
  
/* Populate one Admin User which will further create other users for the application using GUI */
INSERT INTO APP_USER(sso_id, password, first_name, last_name, email)
VALUES ('sam','$2a$10$4eqIF5s/ewJwHK1p8lqlFOEm2QIA0S8g6./Lok.pQxqcxaBZYChRm', 'Sam','Smith','samy@xyz.com');
  
  
/* Populate JOIN Table */
INSERT INTO APP_USER_USER_PROFILE (user_id, user_profile_id)
  SELECT user.id, profile.id FROM app_user user, user_profile profile
  where user.sso_id='sam' and profile.type='ADMIN';
 
/* Create persistent_logins Table used to store rememberme related stuff*/
CREATE TABLE persistent_logins (
    username VARCHAR(64) NOT NULL,
    series VARCHAR(64) NOT NULL,
    token VARCHAR(64) NOT NULL,
    last_used TIMESTAMP NOT NULL,
    PRIMARY KEY (series)
);
	 
	                        
