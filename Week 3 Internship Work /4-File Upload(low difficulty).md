# File Upload Lab

## Overview
This lab demonstrates **File Upload vulnerabilities** in a web application.  

File Upload vulnerabilities occur when an application allows users to upload files without proper validation or restrictions.  
Attackers can exploit this to:
- Upload malicious scripts or backdoors.  
- Execute code on the server (Remote Code Execution – RCE).  
- Access sensitive server information.

---

## Objective
- Understand the **risks of unvalidated file uploads**.  
- Learn to upload both **safe and malicious files** to test server behavior.  
- Explore how attackers can gain **reverse shell access** using uploaded files.  

---

## Steps to Test File Upload

1. Open the lab environment (DVWA or local web app).  
2. Set **Security Level → Low**.  
3. Navigate to **File Upload module**.  
4. Inspect the upload form and check for server-side validation:  
   - File type restrictions  
   - File size restrictions  
   - Execution prevention  

5. Upload a **regular file** (e.g., `.jpg`) to verify functionality.  
6. Upload a **malicious file** (e.g., `.php`) to test execution:  
   - Copy the file path from the upload confirmation.  
   - Access it directly in the browser to see if it executes.  

---

## Observations

- **Low Security**:  
  - No validation or restrictions.  
  - Any file type can be uploaded.  
  - Server executes uploaded PHP scripts.  

- **Implications**:  
  - Attackers can upload backdoors or scripts to control the server.  
  - Sensitive information can be accessed.  
  - Reverse shell attacks become possible.  

---

## Security Notes
- Always test in **controlled lab environments**.  
- File Upload vulnerabilities are highly dangerous because they allow **arbitrary code execution**.  

---

## Mitigation Strategies
1. **Validate file types**: Only allow trusted formats (e.g., images: `.jpg`, `.png`).  
2. **Rename uploaded files**: Avoid using original filenames to prevent overwriting or execution.  
3. **Store files outside the web root**: Prevent direct access via the browser.  
4. **Scan files for malware**: Use antivirus or security scanners.  
5. **Limit file size**: Prevent denial-of-service attacks via large files.  
6. **Disable script execution**: Configure server to not execute uploaded files.  

---

## References
- [OWASP File Upload](https://owasp.org/www-community/vulnerabilities/Unrestricted_File_Upload)  
- DVWA Documentation
```
