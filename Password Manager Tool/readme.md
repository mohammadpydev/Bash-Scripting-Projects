# Password Manager Bash Script

## Overview
This script is a simple password manager written in Bash. It allows users to:

1. Add a new password for a specific domain.
2. Retrieve a password for a specific domain.
3. Store passwords in a plain-text file (`password.txt`).

> **Note**: This script is for demonstration purposes and stores passwords in plain text. For production use, consider adding encryption for enhanced security.

## Features
- **Add Passwords**: Save domain, username/email, and password.
- **Retrieve Passwords**: Fetch stored credentials by domain.
- **Simple Menu**: Interactive menu for easy usage.

## Requirements
- Bash shell (default on most Linux/Unix systems).

## Usage
1. Copy the script below and save it as `password_manager.sh`.
2. Make it executable:
   ```bash
   chmod +x password_manager.sh
   ```
3. Run the script:
   ```bash
   ./password_manager.sh
   ```

## Code
```bash
#!/bin/bash

# Password database file
password_file="password.txt"

# Check if the database exists; if not, create one
if [ ! -e "$password_file" ]; then
    echo "The file $password_file has been created."
    touch "$password_file"
else
    echo "The file $password_file is ready."
fi

# Function to add a new password
add_password() {
    read -p "Enter the website domain: " domain
    read -p "Enter the username or your email: " username
    read -p "Enter the password: " password

    # Validate inputs
    if [[ -z "$domain" || -z "$username" || -z "$password" ]]; then
        echo "Error: All fields are required."
        return
    fi

    # Append to the password file
    echo "$domain:$username:$password" >> "$password_file"
    echo "Password added successfully for $domain!"
}

# Function to get a password
get_password() {
    read -p "Enter the domain name: " website

    # Check if the domain exists in the file
    record=$(grep "^$website:" "$password_file")
    if [[ -n "$record" ]]; then
        user=$(echo "$record" | cut -d ":" -f 2)
        pass=$(echo "$record" | cut -d ":" -f 3)
        echo "================================="
        echo "=== Password Manager Tool ==="
        echo "Domain Name   : $website"
        echo "Username/Email: $user"
        echo "Password      : $pass"
        echo "================================="
    else
        echo "No password found for $website."
    fi
}

# Menu for password manager
while true; do
    echo "========================"
    echo " Password Manager Menu"
    echo "========================"
    echo "1. Add a new password"
    echo "2. Retrieve a password"
    echo "3. Quit"
    echo "========================"

    read -p "Choose an option (1/2/3): " choice

    case $choice in
        1) add_password ;;
        2) get_password ;;
        3) echo "Exiting Password Manager. Goodbye!"; exit ;;
        *) echo "Invalid option. Please try again." ;;
    esac
done
```

## Security Warning
This script stores passwords in plain text, which is not secure. For improved security:

- Use tools like `openssl` or `gpg` to encrypt stored passwords.

## Contribution
Feel free to fork the repository, submit issues, or create pull requests to improve this script!
