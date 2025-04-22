
## Static-Dynamic 
- **Static Content:**  
    Pre-built, unchanging files served directly to the client.
    
- **Dynamic Content:**  
    Content generated on the fly based on logic, user input, or external data. This can happen either on the server (e.g., PHP generating HTML) or on the client (e.g., JavaScript modifying the DOM).

## php nin kendisinde gelen server hakkında bilgiler : 

- **Requesting the base URL (e.g., `example.com/`):**  
    The server looks in the document root for a default file (commonly `index.php`, `index.html`, or `index.htm`).
    
- **Requesting a full file by name (e.g., `example.com/about.php`):**  
    The server directly looks for that file in the document root (or the specified subdirectory), and serves it if found.
    
- **Requesting a URL that doesn't include a file extension (e.g., `example.com/foo`):**  
    If `foo` is a directory, the server looks inside that directory for a default file (like `index.php`, `index.html`, or `index.htm`). If `foo` is intended to be a file, you'll need to include the proper extension in the URL.