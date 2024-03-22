Despliega mi primera aplicación en Azure
Mi primer despliegue en la nube
Parte I - Despliegue app React (frontend) en Azure
Busca Azure for Students en tu buscador de preferencias e ingresa con el correo institucional.
Crea un budget de 1 dólar para la cuenta
El profesor guía el resto de pasos
Parte II - Despliegue app web spring MVC (o spring-boot backend)
Inicie Azure Cloud Shell desde el portal. Para implementar en un grupo de recursos, ingrese el siguiente comando
az group create --name MyResourceGroup --location westus
Para crear un plan de servicio de aplicaciones (App service plan)
az appservice plan create --resource-group MyResourceGroup --name MyPlan --sku F1
Finalmente, cree el servidor MySQL con un nombre de servidor único.
az account list-locations --query "[].{DisplayName:displayName, Name:name}" -o table # choose region
az configure --defaults location=eastus # set region
az mysql flexible-server create --resource-group MyResourceGroup --name pongaunnombreunico --admin-user mysqldbuser --admin-password P2ssw0rd@123 --sku-name Standard_B1ms
Importante: Introduzca un nombre de servidor SQL único. Dado que el nombre de Azure SQL Server no admite las convenciones de nomenclatura de mayúsculas y minúsculas UPPER / Camel , utilice minúsculas para el valor del campo Nombre del servidor de base de datos.

Navegue hasta el grupo de recursos que ha creado. Debería ver un servidor Azure Database for MySQL server aprovisionado. Seleccione el servidor de base de datos.
image

Seleccione Properties. Guarde el Server name y el Server admin login name en un bloc de notas.
image

En este ejemplo, el nombre del servidor es myshuttle-1-mysqldbserver.mysql.database.azure.com y el nombre de usuario administrador es mysqldbuser@myshuttle-1-mysqldbserver.

Seleccione Connection security. Habilite la opción Allow access to Azure services y guarde los cambios. Esto proporciona acceso a los servicios de Azure para todas las bases de datos de su servidor MySQL.
Ejercicio 2: actualización de la configuración de la aplicación web
A continuación, navegue hasta la aplicación web que ha creado. Mientras implementa una aplicación Java, debe cambiar el contenedor web de la aplicación web a Apache Tomcat.

Seleccione Configuration. Establezca Stack settings como se muestra en la imagen a continuación y haga clic en Guardar.
image
Seleccione Overview y click en Browse.
image

La página web se verá como la imagen de abajo.
image

A continuación, debe actualizar las cadenas de conexión para que la aplicación web se conecte correctamente a la base de datos. Hay varias formas de hacerlo, pero para los fines de esta práctica de laboratorio, adoptará un enfoque simple actualizándolo directamente en Azure Portal.

Desde Azure Portal, seleccione la aplicación web que aprovisionó. Ir a Configuración | Configuración de la aplicación | Cadenas de conexión y haga clic en + Nueva cadena de conexión.
image

En la ventana Agregar/Editar cadena de conexión, agregue una nueva cadena de conexión MySQL con MyDatabase como nombre, pegue la siguiente cadena para el valor y reemplace MySQL Server Name, su nombre de usuario y su contraseña con los valores apropiados. Haga clic en Actualizar.
jdbc:mysql://{MySQL Server Name}:3306/alm?useSSL=true&requireSSL=false&autoReconnect=true&user={your user name}&password={your password}
image

Nombre del servidor MySQL: Valor que copió previamente de las Propiedades del servidor MySQL.
su nombre de usuario: Valor que copió previamente de las Propiedades del servidor MySQL.
su contraseña: valor que proporcionó durante la creación del servidor de base de datos MySQL.
Haga clic en Guardar para guardar la cadena de conexión.
Nota: Las cadenas de conexión configuradas aquí estarán disponibles como variables de entorno, con el prefijo del tipo de conexión para aplicaciones Java (también para aplicaciones PHP, Python y Node). En el archivo src/main/resources/application.properties, recuperamos la cadena de conexión reemplazando el siguiente código:

# ORM
# next line deletes the database on startup or shutdown
# spring.jpa.hibernate.ddl-auto=create-drop
# next line updates the database on startup
spring.jpa.hibernate.ddl-auto=update
spring.datasource.url=${MYSQLCONNSTR_MyDatabase}
#spring.datasource.username=root
#spring.datasource.password=my-secret-pw
spring.datasource.driverClassName=com.mysql.cj.jdbc.Driver
spring.jpa.show-sql=true
Ahora ha instalado y configurado todos los recursos necesarios para implementar y ejecutar la aplicación.

Ejercicio 3: implementar los cambios en la aplicación web
Apaga los servicios, conectate con un cliente FTP y sube el jar de la aplicación Java disponible en este enlace https://github.com/PDSW-ECI/spring-mvc-with-bootstrap, sigue este tutorial https://learn.microsoft.com/en-us/azure/app-service/deploy-ftp?tabs=portal
Configuración de la base de datos: image

Configuración del servicio FTP: image

Ejemplo conexión Filezilla: image image

Ejemplo conexión Cyberduck: image image

Configuración necesaria para acceder a FTP: image

Entrega
El enlace de la aplicación React y Spring MVC desplegada en Azure
