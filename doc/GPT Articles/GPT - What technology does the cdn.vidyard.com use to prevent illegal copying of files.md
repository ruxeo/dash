Vidyard, a video hosting and analytics platform, uses several advanced technologies and methods to prevent illegal copying and unauthorized distribution of files. While specific details about Vidyard’s proprietary technology may not be publicly disclosed, typical strategies employed by platforms like Vidyard include:

### 1. **Token-Based Authentication**

Vidyard likely uses token-based authentication for video access. This involves generating a unique token or key that is required to access a video. This token is often time-limited and tied to a specific user or session, making it difficult for unauthorized users to access the content.

- **Implementation**: Tokens are embedded in URLs or included in API requests. They are validated on the server side before granting access to the video.

### 2. **Content Encryption**

Videos are often encrypted using industry-standard encryption methods. This ensures that even if a video file is downloaded, it cannot be played without the appropriate decryption key.

- **Implementation**: Content may be encrypted using AES (Advanced Encryption Standard) or other secure encryption algorithms. The decryption key is only provided to authorized viewers.

### 3. **Secure Streaming Protocols**

Vidyard likely uses secure streaming protocols such as HTTPS (Hypertext Transfer Protocol Secure) and HLS (HTTP Live Streaming) with encryption to deliver videos. This helps prevent man-in-the-middle attacks and unauthorized access during streaming.

- **Implementation**: HTTPS ensures that data transmitted between the server and client is encrypted. HLS can include encryption of video segments and token-based access control.

### 4. **Access Control and Permissions**

Vidyard probably uses detailed access control mechanisms to restrict who can view or download videos. This includes user authentication and authorization checks to ensure that only permitted users can access certain content.

- **Implementation**: Access controls are enforced through user authentication systems (e.g., OAuth) and permission settings that define who can view or interact with content.

### 5. **Watermarking**

Dynamic watermarking is a technique used to embed identifying information into videos. This can help trace the source of leaks or unauthorized copies.

- **Implementation**: Watermarks can be visible or invisible and may include user-specific information to identify the source of a leak.

### 6. **Digital Rights Management (DRM)**

Some platforms integrate DRM technologies to provide additional protection. DRM systems control how content is used, including viewing, copying, and distribution.

- **Implementation**: DRM solutions may use secure containers and licenses to enforce content protection policies.

### 7. **Monitor and Analytics**

Vidyard likely employs analytics to monitor how content is accessed and used. This can help detect unusual behavior that might indicate unauthorized access or copying.

- **Implementation**: Analytics tools track user interactions, including access patterns and download attempts.

### 8. **Tokenized URLs and Referrer Checks**

URLs for video content may be tokenized or include referrer checks to ensure that they are only accessed from authorized sources.

- **Implementation**: URLs may contain tokens that expire after a certain period or are validated against referrer headers to prevent misuse.

### Summary

Vidyard employs a combination of technologies to prevent illegal copying of files, including token-based authentication, content encryption, secure streaming protocols, access controls, watermarking, DRM integration, monitoring, and analytics. These measures work together to ensure that content is secure and that unauthorized access or distribution is minimized.