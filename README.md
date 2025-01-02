**Full Project** : https://github.com/yaniskrm/KeepPass

Password Manager Project

Pages Overview

1. Login/Sign-Up
	•	A standard page for user login or sign-up.

2. Password Generator
	•	Options to define the characters included in the password (uppercase, lowercase, etc.).
	•	Ability to set the password length.
	•	Options for uppercase and/or lowercase letters.
	•	Buttons:
	•	Regenerate: Generate a new password with the selected criteria.
	•	Validate: Redirects to a password saving page with fields for:
	•	Website
	•	Username
	•	Password (pre-filled with the generated password).
	•	Returns the three values associated with the user account (identified by the field PID).

Admin and root accounts are restricted for administrative management.

3. Password Testing
	•	Test an external password (user provides the text to be tested).
	•	Test a password stored in the database for the logged-in user (passwords selectable via a dropdown menu).
	•	Password testing functions (default: brute force).

4. Password Manager
	•	Displays all passwords associated with the user’s account once identity verification is completed (requires re-authentication).
	•	Problem: How to manipulate user passwords in the database while ensuring they remain encrypted?

Database Creation

The database consists of two tables:
	1.	UserKP Table: Stores the username-password pairs for the site.
	2.	UserStorage Table: Stores user-specific data, including the associated website, username, and encrypted passwords.

SQL Script for Database Creation:

-- Create the database (optional, depending on your current environment)
CREATE DATABASE IF NOT EXISTS KeepPass;
USE KeepPass;

-- Create the UserKP table
CREATE TABLE IF NOT EXISTS UserKP (
    idUserKP BIGINT(64) NOT NULL AUTO_INCREMENT,
    pseudoKP VARCHAR(64) NOT NULL,
    passwordKP VARCHAR(64) NOT NULL,
    PRIMARY KEY (idUserKP)
);

-- Create the UserStorage table
CREATE TABLE IF NOT EXISTS UserStorage (
    idUsersKP BIGINT(64) NOT NULL,
    website VARCHAR(64) NOT NULL,
    pseudo VARCHAR(64) NOT NULL,
    password VARCHAR(64) NOT NULL,
    idAccount BIGINT(64) NOT NULL,
    PRIMARY KEY (idAccount),
    FOREIGN KEY (idUsersKP) REFERENCES UserKP(idUserKP)
);

Bloom Filter and Password Robustness

A Bloom Filter is a probabilistic data structure used to test the membership of an element in a set. It may produce false positives (indicating that an element is in the set when it is not) but never false negatives (indicating that an element is not in the set when it actually is). While its primary use lies in its efficiency and memory optimization, its application to password robustness is indirect.

Key Applications in Password Security:
	1.	Checking against common or compromised password lists:
	•	A Bloom Filter can store a large list of weak, commonly used, or previously compromised passwords.
	•	When creating a new password, the system can quickly verify if the password is likely part of this list without storing the entire list in memory or performing slow searches.
	•	If the filter indicates a match (even with false positives), the user can be prompted to choose a different password, discouraging the use of weak or compromised ones.
	2.	Performance optimization in password management systems:
	•	Bloom Filters can optimize checks and queries in large datasets, such as hashed password databases, reducing the number of necessary queries.
	•	For example, before performing an expensive database query to ensure a hashed password’s uniqueness, a Bloom Filter can determine if the password is likely unique.

Considerations:
	•	Bloom Filters should be carefully integrated into password systems, considering their probabilistic properties and the risk of false positives.
	•	Their primary benefit in password security lies in improving performance and reducing storage load while indirectly enhancing password robustness by discouraging weak or previously compromised passwords.

This password manager project is designed to ensure user security while providing essential features like password generation, storage, and testing. It leverages modern cryptographic techniques and innovative data structures for enhanced functionality and security.
