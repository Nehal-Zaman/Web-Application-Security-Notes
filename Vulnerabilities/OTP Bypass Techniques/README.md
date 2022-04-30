# OTP Verification Bypass Techniques

# Using brute force technique:

- Login using a test account and identify the properties of URL like length, using digits etc.
- Now logout and login again.
- Enter a random OTP and intercept it using burp and send the request to intruder.
- Based on the properties of OTP, you can generate a wordlist.
- Start the intruder attack using the wordlist.
- If any request deviates from the other requests in the intruder (status code, response length etc), most probably you have found the OTP.

# OTP getting leaked in response:

- Login using a random invalid OTP.
- Intercept the response for that request.
- The response may contain the OTP.

# OTP bypass through response manipulation:

- Login using a random invalid OTP.
- Intercept the response for that request.
- The response may contain some sort of flag like ‘error’, ‘false’ or 0.
- Change the value to positive like ‘success’, ‘true’, or 1.
- If the server parses this, you have bypassed the OTP.

# Entering numbers instead of alphabets:

- Login using a random invalid OTP.
- Intercept the request.
- Change the number to alphabets, like 0000 → ABCD
- See if it is processed.

# OTP Bypass through Inspect Element:

- Go to the page that initiates OTP verification.
- Right click and open the Inspect Element.
- Check the button/element that initiates OTP generation.
- See what function is called when you click it, e.g. `checkOTP()`.
- If there are such function, check that in the developer console.
- You may find the OTP that is sent to your mobile.