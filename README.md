cd /opt

# Download Tomcat 9
sudo wget sudo wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.115/bin/apache-tomcat-9.0.115.tar.gz

# Extract
sudo tar -xvzf apache-tomcat-9.0.115.tar.gz

# Configure users
sudo tee /opt/apache-tomcat-9.0.115/conf/tomcat-users.xml > /dev/null <<EOF
<?xml version="1.0" encoding="UTF-8"?>
<tomcat-users>

  <role rolename="manager-gui"/>
  <role rolename="manager-script"/>
  <role rolename="manager-status"/>
  <role rolename="admin-gui"/>

  <user username="tomcat"
        password="tomcat"
        roles="manager-gui,manager-script,manager-status,admin-gui"/>

</tomcat-users>
EOF


# Remove IP restriction (Manager)
sudo sed -i 's/<Valve/<!-- <Valve/g' \
/opt/apache-tomcat-9.0.115/webapps/manager/META-INF/context.xml

sudo sed -i 's/\/>/\/> -->/g' \
/opt/apache-tomcat-9.0.115/webapps/manager/META-INF/context.xml


# Remove IP restriction (Host Manager)
sudo sed -i 's/<Valve/<!-- <Valve/g' \
/opt/apache-tomcat-9.0.115/webapps/host-manager/META-INF/context.xml

sudo sed -i 's/\/>/\/> -->/g' \
/opt/apache-tomcat-9.0.115/webapps/host-manager/META-INF/context.xml


# Change port to 9000
sudo sed -i 's/Connector port="8080"/Connector port="9000"/' \
/opt/apache-tomcat-9.0.115/conf/server.xml


# Start Tomcat
/opt/apache-tomcat-9.0.115/bin/startup.sh
