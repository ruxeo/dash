Preventing users from downloading videos from your website or platform while allowing them to view them is a challenging task due to the nature of web technologies. However, you can implement various techniques and strategies to make downloading more difficult. Here’s a comprehensive approach to achieve this:

### 1. **Use Secure Streaming Protocols**

**HTTP Live Streaming (HLS) and Dynamic Adaptive Streaming over HTTP (DASH):**
- **HLS and DASH**: These protocols split videos into small segments and stream them over HTTP. They include support for encryption, which can prevent unauthorized users from downloading the entire video file directly.
- **Encryption**: Use AES encryption to secure video segments. The decryption keys are usually provided only to authorized users.

**Implementation:**
- **HLS**: Set up an HLS streaming server and configure it to use encrypted segments.
- **DASH**: Similarly, configure DASH streaming with encryption.

### 2. **Token-Based Authentication**

**Generate Expiring Tokens:**
- **Unique Tokens**: Issue a unique, time-limited token for each video access request. This token is included in the video URL or request and must be validated by the server.
- **Validation**: Ensure the server validates the token before allowing access to the video stream.

**Implementation:**
- Generate tokens with expiration times and include them in video URLs or API requests.
- Validate tokens on the server side before serving video content.

### 3. **Use DRM (Digital Rights Management)**

**Implement DRM Solutions:**
- **DRM Services**: Integrate with DRM providers like Google Widevine, Microsoft PlayReady, or Apple FairPlay. These services offer content encryption, secure playback, and usage restrictions.
- **Encrypted Media Extensions (EME)**: Use EME in combination with DRM to manage media playback and enforce DRM policies directly in the browser.

**Implementation:**
- Configure DRM settings and integrate the DRM provider’s SDK with your video player.
- Ensure that the video content is encrypted and that playback is controlled by the DRM system.

### 4. **Embed Videos in a Custom Player**

**Use a Custom Video Player:**
- **Custom Players**: Develop or use a custom video player that supports secure streaming protocols and DRM. This player can be designed to prevent right-clicks, disable the download option, and restrict the ability to save video files.

**Implementation:**
- Customize or integrate a video player that adheres to security requirements.
- Disable built-in browser features that allow saving or downloading content.

### 5. **Restrict Access via CORS and Referrer Headers**

**Configure CORS (Cross-Origin Resource Sharing):**
- **CORS Policies**: Set up CORS policies to restrict where your video content can be accessed from. This can help prevent unauthorized domains from accessing your content.

**Referrer Header Checks:**
- **Referrer Validation**: Check the `Referrer` header to ensure that the request for video content comes from your website or an authorized source.

**Implementation:**
- Configure CORS settings on your server to allow access only from specific origins.
- Validate the `Referrer` header in your server-side logic before serving video content.

### 6. **Watermarking**

**Embed Watermarks:**
- **Visible and Invisible Watermarks**: Use visible or invisible watermarks to identify and track video content. This can help deter unauthorized sharing and track the source of leaks.

**Implementation:**
- Embed watermarks during video processing or streaming.
- Track and monitor watermarked content to identify potential leaks.

### 7. **Monitoring and Analytics**

**Track Video Access and Usage:**
- **Analytics**: Implement analytics to monitor how videos are accessed and used. Look for unusual patterns that may indicate attempts to bypass security measures.

**Implementation:**
- Use logging and monitoring tools to track user interactions with video content.
- Analyze access patterns to detect and address potential security issues.

### 8. **Content Delivery Network (CDN) Features**

**Leverage CDN Security Features:**
- **Secure Tokens**: Many CDNs offer secure token-based access to video content. Use these features to control and protect access to your videos.
- **Geo-Blocking**: Restrict access based on geographic location if necessary.

**Implementation:**
- Configure your CDN to use secure tokens for video delivery.
- Set up geo-blocking and other security features as needed.

### Example Implementation Using HLS and Token Authentication

**Server-Side Token Generation and Validation:**

```java
// Generate Token
public String generateToken(String userId, String videoId, long expirationTime) {
    String tokenData = userId + ":" + videoId + ":" + expirationTime;
    String tokenSignature = HMACSHA256(tokenData, SECRET_KEY);
    return Base64.getEncoder().encodeToString((tokenData + ":" + tokenSignature).getBytes());
}

// Validate Token
public boolean validateToken(String token) {
    try {
        String decodedToken = new String(Base64.getDecoder().decode(token));
        String[] parts = decodedToken.split(":");
        String tokenData = parts[0];
        String tokenSignature = parts[1];
        
        String recalculatedSignature = HMACSHA256(tokenData, SECRET_KEY);
        if (!tokenSignature.equals(recalculatedSignature)) {
            return false;
        }
        
        String[] dataParts = tokenData.split(":");
        long expirationTime = Long.parseLong(dataParts[2]);
        if (System.currentTimeMillis() > expirationTime) {
            return false;
        }
        
        return true;
    } catch (Exception e) {
        return false;
    }
}
```

**Client-Side Implementation with Custom Video Player:**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Secure Video Player</title>
    <style>
        /* Custom player styles */
        video {
            width: 100%;
            height: auto;
        }
    </style>
</head>
<body>

<video id="videoPlayer" controls>
    <source src="https://cdn.example.com/video.mp4?token=YOUR_SECURE_TOKEN" type="video/mp4">
    Your browser does not support the video tag.
</video>

<script>
    // Custom player logic if needed
</script>

</body>
</html>
```

### Summary

To prevent downloading videos while allowing viewing, you should use a combination of secure streaming protocols, token-based authentication, DRM solutions, custom video players, and access controls. While these measures can significantly reduce the likelihood of unauthorized downloads, remember that no solution is completely foolproof. Regular monitoring and updates are essential to maintaining security.