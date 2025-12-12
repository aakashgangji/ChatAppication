# Chat Application

A PHP-based real-time chat system with user authentication, chat rooms, and role-based access control (Admin and User).

## Features

### General Features
- **User Authentication**: Login and registration system
- **Real-time Messaging**: AJAX-based chat functionality
- **Chat Rooms**: Create, join, and manage chat rooms
- **Password Protection**: Optional password protection for chat rooms
- **Profile Management**: Update profile photo and account details
- **Responsive Design**: Bootstrap-based UI with DataTables integration

### Admin Features
- **User Management**: Create, update, and delete users
- **Room Management**: Full CRUD operations on chat rooms
- **Member Management**: Add/remove members from chat rooms
- **User List**: View and manage all registered users

### User Features
- **Join Chat Rooms**: Join public or password-protected rooms
- **Create Chat Rooms**: Create new chat rooms with optional passwords
- **My Chat Rooms**: View all chat rooms you're a member of
- **Leave Rooms**: Leave chat rooms you no longer want to participate in
- **Profile Updates**: Update your profile information and photo

## Technology Stack

- **Backend**: PHP (Procedural)
- **Database**: MySQL/MariaDB
- **Frontend**: HTML, CSS, JavaScript
- **Libraries**:
  - jQuery (AJAX requests)
  - Bootstrap 3.3.6 (UI framework)
  - DataTables (for data display)

## Requirements

- PHP 5.6+ (or PHP 7.x/8.x)
- MySQL/MariaDB
- Apache/Nginx web server
- Web browser with JavaScript enabled

## Installation

1. **Navigate to the project directory**
   ```bash
   cd /path/to/CA
   ```

2. **Start MySQL service** (if not already running)
   
   **For macOS (Homebrew):**
   ```bash
   brew services start mysql
   ```
   
   **For Linux:**
   ```bash
   sudo systemctl start mysql
   # or
   sudo service mysql start
   ```
   
   **For Windows:**
   - Start MySQL from Services or use MySQL Workbench
   
   **Verify MySQL is running:**
   ```bash
   mysql -u root -e "SELECT 1;" 2>&1
   ```
   If you see an error like `ERROR 2002 (HY000): Can't connect to local MySQL server`, the service is not running.

3. **Create the database**
   
   **If your MySQL root user has a password:**
   ```bash
   mysql -u root -p -e "CREATE DATABASE IF NOT EXISTS chat_system;"
   ```
   Enter your MySQL root password when prompted.
   
   **If your MySQL root user has NO password (common with fresh Homebrew installations):**
   ```bash
   mysql -u root -e "CREATE DATABASE IF NOT EXISTS chat_system;"
   ```
   Or use `-p` and simply press Enter when prompted for password.

4. **Import the database schema**
   
   **If your MySQL root user has a password:**
   ```bash
   mysql -u root -p chat_system < ChatAppication/db/chat_system.sql
   ```
   
   **If your MySQL root user has NO password:**
   ```bash
   mysql -u root chat_system < ChatAppication/db/chat_system.sql
   ```
   Or use `-p` and press Enter when prompted.

5. **Verify the database import**
   
   **If your MySQL root user has a password:**
   ```bash
   mysql -u root -p -e "USE chat_system; SHOW TABLES;"
   ```
   
   **If your MySQL root user has NO password:**
   ```bash
   mysql -u root -e "USE chat_system; SHOW TABLES;"
   ```
   
   You should see four tables: `chat`, `chat_member`, `chatroom`, and `user`.
   
   > **Note**: If you get `ERROR 1045 (28000): Access denied`, it means you entered a password when your MySQL root user has none. Either omit the `-p` flag or press Enter (empty password) when prompted.

6. **Configure database connection**
   - Edit `ChatAppication/conn.php` and update the database credentials:
   
   **If your MySQL root user has a password:**
   ```php
   $conn = mysqli_connect("localhost","root","your_password","chat_system");
   ```
   
   **If your MySQL root user has NO password (default for many installations):**
   ```php
   $conn = mysqli_connect("localhost","root","","chat_system");
   ```
   The empty string `""` means no password. This is the default configuration and works for most fresh MySQL installations.

7. **Set up file permissions**
   - Ensure the `upload/` directory is writable for profile photo uploads:
   ```bash
   chmod 755 ChatAppication/upload/
   ```

8. **Start your web server**
   
   **Using PHP built-in server (for development):**
   ```bash
   cd ChatAppication
   php -S localhost:8000
   ```
   Then access at `http://localhost:8000`
   
   **Using Apache/Nginx:**
   - Ensure your web server is configured and running
   - Access at `http://localhost/ChatAppication/` (or your configured URL)

9. **Access the application**
   - Navigate to the login page in your web browser
   - Use the default admin credentials to log in (see Default Login Credentials below)

## Default Login Credentials

The database includes default users:

### Admin Account
- **Username**: `admin`
- **Password**: `admin`
- **Access Level**: Admin (access = 1)

### Regular User Accounts
- **Username**: `aakash` / **Password**: `aakash`
- **Username**: `test1` / **Password**: `test1`
- **Access Level**: User (access = 2)

> **Note**: Passwords are stored as MD5 hashes. For production use, consider upgrading to more secure hashing methods (bcrypt, Argon2).

## Project Structure

```
ChatAppication/
├── admin/              # Admin panel files
│   ├── index.php      # Admin dashboard
│   ├── user.php       # User management
│   ├── chatroom.php   # Chat room interface
│   └── ...
├── user/              # User panel files
│   ├── index.php      # User dashboard
│   ├── chatroom.php   # Chat room interface
│   └── ...
├── db/
│   └── chat_system.sql # Database schema
├── css/               # Stylesheets
├── js/                # JavaScript files
├── upload/            # Uploaded profile photos
├── conn.php           # Database connection
├── index.php          # Login page
├── login.php          # Login handler
├── register.php       # Registration handler
└── signup.php         # Signup page
```

## Database Schema

### Tables

- **`user`**: Stores user accounts (username, password, name, photo, access level)
- **`chatroom`**: Stores chat room information (name, password, creator, creation date)
- **`chat`**: Stores chat messages (user, room, message, timestamp)
- **`chat_member`**: Junction table linking users to chat rooms

## Usage

### For Administrators

1. Log in with admin credentials
2. Access the admin dashboard at `admin/index.php`
3. Manage users, chat rooms, and members from the dashboard
4. Use the navigation bar to access different management sections

### For Regular Users

1. Register a new account or log in with existing credentials
2. Access the user dashboard at `user/index.php`
3. Browse available chat rooms or create new ones
4. Join chat rooms (enter password if required)
5. Send messages in chat rooms
6. Manage your profile and account settings

## Security Considerations

⚠️ **Important Security Notes**:

1. **Password Hashing**: The application currently uses MD5 for password hashing, which is considered insecure. For production use, upgrade to:
   - `password_hash()` with `PASSWORD_BCRYPT` or `PASSWORD_ARGON2ID`
   - `password_verify()` for password verification

2. **SQL Injection**: While some input sanitization exists, consider using prepared statements throughout the application.

3. **XSS Protection**: Input sanitization is implemented, but ensure all user-generated content is properly escaped when displayed.

4. **Session Security**: Review and enhance session security settings for production deployment.

5. **File Upload**: Implement proper validation and restrictions for uploaded files (profile photos).

## Development

### Adding New Features

- Follow the existing code structure
- Maintain separation between admin and user functionality
- Use AJAX for dynamic updates
- Ensure proper input validation and sanitization

### Customization

- Modify CSS files in the `css/` directory for styling changes
- Update Bootstrap theme or DataTables configuration as needed
- Customize database schema in `db/chat_system.sql`

## Troubleshooting

### Common Issues

1. **MySQL Connection Error: `ERROR 2002 (HY000): Can't connect to local MySQL server`**
   - **Problem**: MySQL service is not running
   - **Solution**: 
     - macOS (Homebrew): `brew services start mysql`
     - Linux: `sudo systemctl start mysql` or `sudo service mysql start`
     - Windows: Start MySQL from Services panel
   - **Verify**: Run `mysql -u root -e "SELECT 1;"` to test connection

2. **MySQL Access Denied Error: `ERROR 1045 (28000): Access denied for user 'root'@'localhost'`**
   - **Problem**: You entered a password when your MySQL root user has no password set (or entered wrong password)
   - **Solution**: 
     - Try without password: `mysql -u root -e "SELECT 1;"`
     - Or use `-p` and press Enter (empty password) when prompted
     - If you have a password but forgot it, you may need to reset MySQL root password

3. **Database Connection Error in Application**
   - Verify database credentials in `conn.php`
   - Ensure MySQL service is running (see issue #1)
   - Check database name matches `chat_system`
   - Verify the database exists: `mysql -u root -e "SHOW DATABASES LIKE 'chat_system';"`
   - Check if tables exist: `mysql -u root -e "USE chat_system; SHOW TABLES;"`
   - Ensure password in `conn.php` matches your MySQL root password (use `""` for no password)

4. **Import Error: Database doesn't exist**
   - **Problem**: Trying to import SQL before creating the database
   - **Solution**: Create the database first:
     ```bash
     mysql -u root -p -e "CREATE DATABASE chat_system;"
     mysql -u root -p chat_system < ChatAppication/db/chat_system.sql
     ```

5. **Upload Directory Permissions**
   - Ensure `upload/` directory has write permissions:
     ```bash
     chmod 755 ChatAppication/upload/
     ```
   - Check PHP `upload_max_filesize` and `post_max_size` settings in `php.ini`

6. **Session Issues**
   - Verify PHP sessions are enabled
   - Check session storage directory permissions
   - Ensure `session_start()` is called before any output

7. **AJAX Not Working**
   - Ensure jQuery is loaded correctly
   - Check browser console for JavaScript errors
   - Verify file paths are correct
   - Check that your web server is properly configured

8. **MySQL Service Not Starting Automatically**
   - **macOS (Homebrew)**: MySQL should auto-start, but if not:
     ```bash
     brew services start mysql
     ```
   - **Linux**: Enable MySQL to start on boot:
     ```bash
     sudo systemctl enable mysql
     ```

## License

This project appears to be a learning/demonstration project. Please review and add appropriate licensing information as needed.

## Contributing

Contributions are welcome! Please ensure:
- Code follows existing style conventions
- Security best practices are maintained
- New features are tested before submission

## Support

For issues or questions, please review the code comments or create an issue in the project repository.

---

**Note**: This is a basic chat application suitable for learning and development purposes. For production deployment, additional security measures, error handling, and performance optimizations should be implemented.

