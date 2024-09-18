# TÜRKÇE VERSİYON
# Metabase Veritabanı Yedeğini MySQL'e Taşıma Rehberi

Bu rehber, Windows Server 2022 üzerinde kurulu WSL üzerinden çalışan Docker için metabase veritabanı yedeğini MySQL'e taşıma adımlarını açıklamaktadır.

## Adımlar

1. **Metabase JAR Dosyasını İndirin**
   - Metabase JAR dosyasını [metabase.io](https://www.metabase.com/) sitesinden indirin. Docker'da kurulu sürüm ile aynı olmalıdır.

2. **Java İndirin ve Kurun (JRE VE JDK)**
   - [Java 11](https://www.oracle.com/java/technologies/downloads/#java11-windows) (Windows için) ve [Java](https://www.java.com/tr/download/) (Windows için) indirin ve kurun.

3. **Docker'dan Veritabanı Dosyasını Kopyalayın**
   - Docker üzerinde konsola veya Windows PowerShell'e şu komutu girin:
     ```bash
     docker cp metabase:/metabase.db/metabase.db.mv.db ./
     ```
   - Bu işlem, genellikle `C:\Users\YOUR_USER_NAME` altına `metabase.db.mv` dosyasını oluşturacaktır.

4. **MySQL'i İndirin ve Kurun**
   - [MySQL İndir](https://dev.mysql.com/downloads/installer/) ve kurun. MySQL Workbench veya DBeaver gibi bir araç kullanarak MySQL veritabanınızı yönetin.

5. **Yeni Bir Veritabanı Oluşturun**
   - MySQL yönetim aracında şu komutu kullanarak boş bir Metabase veritabanı oluşturun:
     ```sql
     CREATE DATABASE metabase CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
     ```

6. **Metabase JAR Dosyasını ve Yedek Dosyayı Kopyalayın**
   - Önce indirdiğiniz `metabase.jar` dosyasını ve oluşturduğunuz `metabase.db.mv.db` dosyasını örneğin `C:\metabase` dizinine kopyalayın.

7. **Veriyi Taşıyın**
   - PowerShell'i yönetici olarak çalıştırın ve şu komutu girin:
     ```bash
     java -jar -DMB_DB_TYPE=mysql -DMB_DB_DBNAME=metabase -DMB_DB_PORT=3306 -DMB_DB_USER=YOUR_ADMIN_NAME -DMB_DB_PASS=YOUR_PASSWORD metabase.jar load-from-h2 ./metabase.db
     ```
   - Veri taşımanın bittiğine dair bir ibare görene kadar bekleyin.

8. **Yeni Bir Docker Konteyneri Oluşturun**
   - Yeni bilgilerle yeni bir Docker konteyneri oluşturun:
     ```bash
     docker run -d -p 3000:3000 -e "MB_DB_TYPE=mysql" -e "MB_DB_DBNAME=metabase" -e "MB_DB_PORT=3306" -e "MB_DB_USER=YOUR_ADMIN_NAME" -e "MB_DB_PASS=YOUR_PASSWORD" -e "MB_DB_HOST=host.docker.internal" --name metabase2 metabase/metabase
     ```

   - Eğer yeni konteyner oluştururken hata alırsanız, eski konteyneri silip şu komutu çalıştırarak yeni bir konteyner oluşturabilirsiniz:
     ```bash
     docker run -d -p 3000:3000 -e "MB_DB_TYPE=mysql" -e "MB_DB_DBNAME=metabase" -e "MB_DB_PORT=3306" -e "MB_DB_USER=YOUR_ADMIN_NAME" -e "MB_DB_PASS=YOUR_PASSWORD" -e "MB_DB_HOST=host.docker.internal" --name metabase metabase/metabase
     ```

9. **Daha Fazlası İçin Resmi Belgeleri İnceleyin**
   - Bu rehber, sadece kişisel deneyimlerime dayanmaktadır. Daha fazla bilgi için [Metabase Resmi Belgeleri](https://www.metabase.com/docs/) ile [Docker Belgeleri](https://docs.docker.com/)ni inceleyebilirsiniz.

# ENGLISH VERSION
# Metabase Database Backup Migration to MySQL Guide

This guide explains the steps to migrate a Metabase database backup to MySQL for Docker running on WSL installed on Windows Server 2022.

## Steps

1. **Download Metabase JAR File**
   - Download the Metabase JAR file from [metabase.io](https://www.metabase.com/). It should match the version installed in Docker.

2. **Download and Install Java (JRE and JDK)**
   - Download and install [Java 11](https://www.oracle.com/java/technologies/downloads/#java11-windows) and [Java](https://www.java.com/tr/download/) for Windows.

3. **Copy Database File from Docker**
   - Run the following command in Docker console or Windows PowerShell:
     ```bash
     docker cp metabase:/metabase.db/metabase.db.mv.db ./
     ```
   - This will typically create the `metabase.db.mv` file under `C:\Users\YOUR_USER_NAME`.

4. **Download and Install MySQL**
   - Download and install MySQL from [MySQL Downloads](https://dev.mysql.com/downloads/installer/). Use MySQL Workbench or a tool like DBeaver to manage your MySQL database.

5. **Create a New Database**
   - Use the following command in your MySQL management tool to create an empty Metabase database:
     ```sql
     CREATE DATABASE metabase CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
     ```

6. **Copy Metabase JAR and Backup File**
   - Copy the downloaded `metabase.jar` file and the `metabase.db.mv.db` file to a directory such as `C:\metabase`.

7. **Migrate Data**
   - Run PowerShell as an administrator and use the following command:
     ```bash
     java -jar -DMB_DB_TYPE=mysql -DMB_DB_DBNAME=metabase -DMB_DB_PORT=3306 -DMB_DB_USER=YOUR_ADMIN_NAME -DMB_DB_PASS=YOUR_PASSWORD metabase.jar load-from-h2 ./metabase.db
     ```
   - Wait until you see a message indicating that the data migration is complete.

8. **Create a New Docker Container**
   - Create a new Docker container with the updated information:
     ```bash
     docker run -d -p 3000:3000 -e "MB_DB_TYPE=mysql" -e "MB_DB_DBNAME=metabase" -e "MB_DB_PORT=3306" -e "MB_DB_USER=YOUR_ADMIN_NAME" -e "MB_DB_PASS=YOUR_PASSWORD" -e "MB_DB_HOST=host.docker.internal" --name metabase2 metabase/metabase
     ```

   - If you encounter an error when creating the new container, delete the old container and create a new one with the following command:
     ```bash
     docker run -d -p 3000:3000 -e "MB_DB_TYPE=mysql" -e "MB_DB_DBNAME=metabase" -e "MB_DB_PORT=3306" -e "MB_DB_USER=YOUR_ADMIN_NAME" -e "MB_DB_PASS=YOUR_PASSWORD" -e "MB_DB_HOST=host.docker.internal" --name metabase metabase/metabase
     ```

9. **Consult Official Documentation for More Information**
   - This guide is based on personal experience. For more details, refer to the [Metabase Official Documentation](https://www.metabase.com/docs/) and [Docker Documentation](https://docs.docker.com/).
