🌸 Milestone 1: Secure Password Reset with Email OTP

Infosys Springboard Internship 7.0 · Batch 4

A password reset module built with Streamlit, JWT, bcrypt, SQLite and Gmail SMTP. It runs in Google Colab and is shared through an ngrok public URL.

Features
Forgot Password with a 6-digit OTP sent by email
OTP expires after 5 minutes and is verified through a signed JWT
Strong password rules (10+ characters, upper and lower case, number, symbol)
Passwords stored as bcrypt hashes in SQLite
Secrets loaded from Colab Secrets, not hard-coded
Soft pink, responsive interface

Tech Stack

Python · Streamlit · PyJWT · bcrypt · SQLite · Gmail SMTP · ngrok · Google Colab

How to Run
Install the packages:
python
   !pip install -q streamlit pyjwt bcrypt pyngrok

   
In Colab Secrets (🔑), add these and turn Notebook access on:
Secret	Value
JWT_SECRET	Long random string
NGROK_AUTHTOKEN	Token from ngrok dashboard
EMAIL_PASSWORD	Gmail App Password
EMAIL_ADDRESS	Gmail address that sends the OTP
Run the three notebook cells in order: db.py, app.py, then the launcher.
Open the ngrok URL printed by the last cell.
