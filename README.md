# Multiple Vulnerabilities in U.are.U 4500 Fingerprint Reader and its Linux/Windows Drivers
- Cleartext transmission of sensitive information (e.g., encryption key)
- Use of insufficiently random values when generating initialization vector  

# Basic Operation
When a user try to use fingerprint authentication, a user might touch a finger on the fingerprint reader device.
Just after the capturing fingerprint, the driver generates an initialization vector, and sends it to the reader device.
The device makes a key for image obfuscation and do obfuscate the fingerprint image using provided initialization vector and the key.
After then, the obfuscated fingerprint image and the key are transferred to the driver.
Finally, the driver can de-obfuscate the image using provided key from the device and initialization vector that was held before.

# Vulnerabilities
[CVE-2019-12813] Key used for obfuscating fingerprint image exhibits cleartext when fingerprint scanner device transfers a fingerprint image, that is just scanned, to the driver.  
- Key should remain encrypted during transfer operation  
- CWE-319: Cleartext Transmission of Sensitive Information

[CVE-2019-13603, CVE-2019-13621] Initialization vector used for generating keystream is the same sequence of numbers each time.  
In Windows, U.are.U 4500 Fingerprint Reader Windows Biometric Framework (WBF) 5.0.0.5 driver even has a statically coded initialization vector.  
In Linux, libfprint 0.99 driver uses predictable initialization vector, that are generated by 'rand()' in libc by default.  
- Initialization vector should also be in no predictable pattern  
- CWE-330: Use of Insufficiently Random Values  

Unfortunately, it does not comply the above criteria.  

# Attack Vector 
An attacker who sniffs an encrypted fingerprint image can easily decrypt that image using the key and initialization vector.
Here, the key used for obfuscating the fingerprint image exhibit cleartext when the fingerprint scanner device transfers a fingerprint image to the driver.
The initialization vector can be also extrapolated since it has been a statically coded or predictable random value.

# Timeline
2019-03-23: The issue has been identified, documented and reported to the vendor. -> no answer  
2019-04-18: The issue has been reported to CERT/CC.  
2019-04-19: CERT/CC have contacted the vendor and demanded immediate return with a reasonable timeline where the vendor will provide patch information. -> fail to respond  
2019-05-04, 2019-05-29: CERT/CC have re-contacted the vendor and re-demanded a prompt response and providing a timeline for full mitigation of this vulnerability. -> fail to respond again  
2019-06-13: CERT/CC have recommended choosing a target date for public disclosure and proceeding with disclosure.  
2019-06-13: CVE-2019-12813 has been assigned.  
2019-07-15: CVE-2019-13603 has been assigned.  
2019-07-17: CVE-2019-13621 has been assigned.  

# How to build
You require the following to build this project:  
- glib-2.0

# Demo video
It is a PoC for de-obfuscating encrypted fingerprint image.  

- Using cleartext key & initialization vector after MITMing fingerprint reader device  
  - In Windows:  
    [![Video Label](https://img.youtube.com/vi/wEXJDyEOatM/0.jpg)](https://youtu.be/wEXJDyEOatM=0s)    
  - In Linux:  
    [![Video Label](https://img.youtube.com/vi/Grirez2xeas/0.jpg)](https://youtu.be/Grirez2xeast=0s)  
- Using cleartext key & reproducible initialization vector  
  - In Windows (U.are.U 4500 Fingerprint Reader WBF 5.0.0.5):  
    [![Video Label](https://img.youtube.com/vi/g-kI5P4HFN0/0.jpg)](https://youtu.be/g-kI5P4HFN0=0s)
  - In Linux (libfprint 0.99):  
    [![Video Label](https://img.youtube.com/vi/TyHRVvkDCHo/0.jpg)](https://youtu.be/TyHRVvkDCHo=0s)  

