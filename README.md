// Standard Input Output Library

#include <stdio.h> 

// Console Input Output (for getch())

#include <conio.h> 

// Windows-specific functions (for system(), Beep()) 

#include <windows.h> 

// Defining ENTER key ASCII value

#define ENTER 13 

// Defining TAB key ASCII value 

#define TAB 9 

// Defining BACKSPACE key ASCII value 

#define BCKSPC 8 

// Structure to hold user information

struct user {

 char fullName[50]; // User's full name

 char email[50]; // User's email address

 char password[50]; // User's password

 char username[50]; // Username generated from the email

 char phone[50]; // User's contact number
 };
 // Function to take string input from the user

void takeinput(char ch[50]) {

// Get input from user with a maximum of 50 characters

 fgets(ch, 50, stdin); 

// Remove the newline character at the end of the string

 ch[strlen(ch) - 1] = 0; 

}

// Function to generate username from the email

char generateUsername(char email[50], char username[50]) {

 // Iterate through the email until '@' is found

 for (int i = 0; i < strlen(email); i++) {

 if (email[i] == '@') break; // Stop when '@' is found

 else username[i] = email[i]; // Copy characters to username

 }

}

// Function to take password input and hide it with '*'

void takepassword(char pwd[50]) {

 int i = 0;

 char ch;

 while (1) {

 ch = getch(); // Read one character without displaying it on screen
 // End input on ENTER or TAB

 if (ch == ENTER || ch == TAB) {
 // Null terminate the password string 

pwd[i] = '\0'; 

 break;

}

// Handle BACKSPACE

 else if (ch == BCKSPC) { 

 if (i > 0) {

// Move the index back

 i--; 

// Erase the character on screen

 printf("\b \b"); 

 }

 } else {

// Add character to the password

 pwd[i++] = ch; 

// Print '*' to hide the actual character

 printf("* \b"); 

 }

 }

}

int main() {

// Set console text color (light cyan on black)
system("color 0b");
// File pointer for user data storage

FILE *fp; 

// Option for menu and flag for user found 

 int opt, usrFound = 0;

// Instance of struct user 

 struct user user; 

// Variable to store password confirmation

 char password2[50]; 

 // Display welcome message and options

 printf("\n\t\t\t\t----------Welcome to authentication system----------");

 printf("\nPlease choose your operation");

 printf("\n1. Signup");

 printf("\n2. Login");

 printf("\n3. Exit");

 printf("\n\nYour choice:\t");

// Take the user's choice

 scanf("%d", &opt); 

// Consume the newline left by scanf

 fgetc(stdin); 

 switch (opt) {

// Signup process

 case 1:
 // Clear the screen

system("cls"); 

printf("\nEnter your full name:\t");

// Take full name input

 takeinput(user.fullName); 

 printf("Enter your email:\t");

// Take email input

 takeinput(user.email); 

 printf("Enter your contact no:\t");

// Take phone number input

 takeinput(user.phone); 

 printf("Enter your password:\t");

// Take password input

 takepassword(user.password); 

 printf("\nConfirm your password:\t");

// Confirm password

 takepassword(password2); 

 // Check if passwords match

 if (!strcmp(user.password, password2)) {
 // Generate username 

generateUsername(user.email, user.username); 

// Open the file to append data

 fp = fopen("Users.dat", "a+"); 

// Write user data to file

 fwrite(&user, sizeof(struct user), 1, fp); 

 // Check if data is written successfully

 if (fwrite != 0) 

 printf("\n\nUser registration success, Your username is 

%s", user.username);

 else 

 printf("\n\nSorry! Something went wrong :(");

// Close the file 

fclose(fp); 

 } else {

 printf("\n\nPasswords do not match");

// Beep sound to indicate error

 Beep(750, 300); 

 }

 break;
 // Login process 

case 2: 

 {

// Variables for input

 char username[50], pword[50]; 

// Instance of struct user for login

 struct user usr; 

 printf("\nEnter your username:\t");

// Take username input

 takeinput(username); 

 printf("Enter your password:\t");

// Take password input

 takepassword(pword); 

// Open file in read mode 

fp = fopen("Users.dat", "r"); 

 // Read data from file and compare

 while (fread(&usr, sizeof(struct user), 1, fp)) {

// Check username

 if (!strcmp(usr.username, username)) {
 // Check password

 if (!strcmp(usr.password, pword)) { 

// Clear screen on successful login

 system("cls"); 

 printf("\n\t\t\t\t\t\tWelcome %s", usr.fullName);

 printf("\n\n| Full Name:\t%s", usr.fullName);

 printf("\n| Email:\t%s", usr.email);

 printf("\n| Username:\t%s", usr.username);

 printf("\n| Contact no.:\t%s", usr.phone);

 } else {

 printf("\n\nInvalid Password!");

// Beep for wrong password

 Beep(800, 300); 

 }

// Set flag if user is found

 usrFound = 1; 

 }

 }

 if (!usrFound) {

 printf("\n\nUser is not registered!");

// Beep if user not found

 Beep(800, 300); 

 }
 // Close the file

 fclose(fp); 

 }

 break;

// Exit

 case 3: 

 printf("\t\t\tBye Bye :)");

// Exit the program

 return 0; 

 }

 

 return 0;

}
