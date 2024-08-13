To prevent unauthorized users from downloading files while still allowing them to view the content, you can implement several strategies that involve both server-side and client-side techniques. Below is a step-by-step guide and an example program to achieve this:

### 1. **Serve Files via a Secure Endpoint**

Instead of serving files directly from the server's file system, serve them through a secure endpoint that checks the user's permissions. This way, you can control access to the files based on user roles or authentication status.

#### Example: Java Servlet for Secure File Serving

```java
@WebServlet("/secure-file")
public class SecureFileServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // Check if the user is authenticated and authorized
        boolean isAuthenticated = checkUserAuthentication(request);
        boolean isAuthorized = checkUserAuthorization(request);

        if (!isAuthenticated || !isAuthorized) {
            response.sendError(HttpServletResponse.SC_FORBIDDEN, "You are not authorized to access this file.");
            return;
        }

        // Serve the file in a way that prevents downloading
        File file = new File("/path/to/your/file.pdf");
        response.setContentType("application/pdf");
        response.setHeader("Content-Disposition", "inline; filename=\"" + file.getName() + "\"");

        try (FileInputStream in = new FileInputStream(file);
             OutputStream out = response.getOutputStream()) {

            byte[] buffer = new byte[1024];
            int bytesRead;
            while ((bytesRead = in.read(buffer)) != -1) {
                out.write(buffer, 0, bytesRead);
            }
        }
    }

    private boolean checkUserAuthentication(HttpServletRequest request) {
        // Implement your authentication logic (e.g., session check, token validation)
        return true; // Example: return true if authenticated
    }

    private boolean checkUserAuthorization(HttpServletRequest request) {
        // Implement your authorization logic (e.g., role check)
        return true; // Example: return true if authorized
    }
}
```

**Explanation:**
- **Authentication and Authorization**: Before serving the file, the servlet checks if the user is authenticated and authorized to view the content.
- **Content-Disposition**: The `Content-Disposition` header is set to `inline`, which instructs the browser to display the file (e.g., a PDF) in the browser rather than prompting a download.

### 2. **Use Content Security Policy (CSP) and JavaScript**

To add an extra layer of protection, you can use a Content Security Policy (CSP) and JavaScript to disable the ability to download the file directly.

#### Example: Adding CSP and JavaScript

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Secure File Viewer</title>
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'; object-src 'none';">
    <style>
        /* Disable user selection and dragging to prevent easy copying */
        body {
            user-select: none;
            -webkit-user-select: none;
        }
        img, iframe {
            -webkit-user-drag: none;
            -khtml-user-drag: none;
            -moz-user-drag: none;
            -o-user-drag: none;
            user-drag: none;
        }
    </style>
    <script>
        // Disable right-click to prevent 'Save As' or other download options
        document.addEventListener('contextmenu', event => event.preventDefault());

        // Disable keyboard shortcuts like Ctrl+S
        document.addEventListener('keydown', function(event) {
            if (event.ctrlKey && (event.key === 's' || event.key === 'S')) {
                event.preventDefault();
                alert('Saving this content is not allowed.');
            }
        });
    </script>
</head>
<body>

<h1>Secure Document Viewer</h1>
<iframe src="/secure-file" width="100%" height="600px"></iframe>

</body>
</html>
```

**Explanation:**
- **CSP**: The Content Security Policy is set to allow scripts only from the same origin and to block the embedding of objects that could be used to download files.
- **JavaScript**: JavaScript is used to disable right-clicks and certain keyboard shortcuts that could be used to download or save the file.
- **Iframe**: The file is displayed in an iframe, which is embedded in the page with the `inline` content disposition.

### 3. **PDF Specific Controls**
If you're serving PDFs, you can further lock down the viewing by using specialized PDF viewers that provide view-only access, or by applying digital rights management (DRM) controls to the PDF files themselves.

#### Example: Using a PDF Viewer Library

If using JavaScript, you can integrate a PDF viewer library that limits user interaction, such as disabling printing or downloading.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Secure PDF Viewer</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.10.377/pdf.min.js"></script>
    <style>
        #pdf-viewer {
            width: 100%;
            height: 600px;
            border: 1px solid #ccc;
        }
    </style>
</head>
<body>

<h1>Secure PDF Viewer</h1>
<canvas id="pdf-viewer"></canvas>

<script>
    const url = '/secure-file';
    const pdfjsLib = window['pdfjs-dist/build/pdf'];
    pdfjsLib.GlobalWorkerOptions.workerSrc = 'https://cdnjs.cloudflare.com/ajax/libs/pdf.js/2.10.377/pdf.worker.min.js';

    const loadingTask = pdfjsLib.getDocument(url);
    loadingTask.promise.then(function(pdf) {
        pdf.getPage(1).then(function(page) {
            const scale = 1.5;
            const viewport = page.getViewport({ scale: scale });

            const canvas = document.getElementById('pdf-viewer');
            const context = canvas.getContext('2d');
            canvas.height = viewport.height;
            canvas.width = viewport.width;

            const renderContext = {
                canvasContext: context,
                viewport: viewport
            };
            page.render(renderContext);
        });
    });
</script>

</body>
</html>
```

**Explanation:**
- **PDF.js Library**: This JavaScript library renders PDFs in a `<canvas>` element, providing more control over how the PDF is displayed and interacted with.
- **View-Only Mode**: The PDF is rendered within the canvas, and you can further customize the viewer to disable additional features like zooming, printing, etc.

### Summary
By combining server-side checks for authentication and authorization with client-side controls, you can effectively prevent unauthorized users from downloading files while still allowing them to view the content. The solution involves secure file serving through a backend, enforcing inline display, and using JavaScript and CSP to restrict user actions that could lead to file downloads.