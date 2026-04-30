# PortSwigger Lab: Username Enumeration via Different Responses

## 🎯 Objective
The goal of this lab was to bypass the login mechanism by enumerating a valid username from a provided list and then brute-forcing the corresponding password to successfully log into the user's account.

## ⚠️ Vulnerability & Business Impact
The application suffers from an **Authentication Flaw** that allows username enumeration. By analyzing subtle differences in the server's HTTP responses (such as response lengths or status codes), an attacker can reliably identify which usernames exist in the database. Once a valid user is identified, the attacker can perform a targeted password brute-force attack, ultimately leading to an Account Takeover (ATO).

## 🛠️ Tools Used
*   **Burp Suite Professional** (Repeater and Intruder)

## 📝 Step-by-Step Exploitation

**Step 1: Intercepting the Authentication Traffic**
To begin, I attempted a login with dummy credentials and intercepted the `POST /login` request using Burp Suite to analyze the parameters being sent to the server.

<!-- 📸 ADD YOUR REPEATER SCREENSHOT HERE (The one showing the POST /login request and 302 Found response) -->

**Step 2: Configuring the Payload**
After successfully enumerating the valid username (`antivirus`), I sent the request to **Burp Intruder** to brute-force the password. I configured the payload position and loaded the provided dictionary of candidate passwords.

<!-- 📸 ADD YOUR INTRUDER SETUP SCREENSHOT HERE (The one showing the Payloads tab and the list of numbers) -->

**Step 3: Analyzing Intruder Results**
I initiated the Sniper attack and monitored the results. I paid close attention to the HTTP status codes and response lengths. While incorrect passwords returned a standard `200 OK` status, the payload `777777` returned a `302 Found` status with a significantly different response length (191 bytes). This anomaly confirmed a successful authentication attempt.

<!-- 📸 ADD YOUR INTRUDER RESULTS SCREENSHOT HERE (The one highlighting the '777777' payload and the 302 status code) -->

**Step 4: Account Takeover**
With the confirmed credentials (`antivirus` : `777777`), I navigated back to the login page, submitted the credentials, and successfully accessed the account to solve the lab.

<!-- 📸 ADD YOUR SOLVED LAB SCREENSHOT HERE (The one showing the "Congratulations, you solved the lab!" banner) -->

## 🧠 Key Takeaways
This lab highlights the critical importance of analyzing HTTP response variations. Using Burp Intruder to monitor status codes (`302 Found` vs `200 OK`) and response lengths is a highly effective methodology for confirming successful brute-force attacks without needing to write custom automation scripts.

