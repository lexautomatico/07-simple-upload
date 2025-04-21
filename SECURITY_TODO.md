# Security To-Do for Simple Upload

## Environment Variables Setup

This file outlines the environment variables that need to be set up for secure deployment of the Simple Upload application.

### Required Environment Variables

The following environment variables should be configured:

- **REACT_APP_IPFS_PROJECT_ID**: Infura IPFS Project ID for authentication
- **REACT_APP_IPFS_PROJECT_SECRET**: Infura IPFS Project Secret for authentication

### Implementation Steps

1. Create a `.env` file in the project root with the following template:
   ```
   REACT_APP_IPFS_PROJECT_ID=your_ipfs_project_id_here
   REACT_APP_IPFS_PROJECT_SECRET=your_ipfs_project_secret_here
   ```

2. Update your code to use these environment variables:

   a. In `src/utils/ipfsClient.js`:
   ```javascript
   // Replace:
   const projectId = "2DBM8M1VMImQ5vl4IQxYQzI8uKg";
   const projectSecret = "f34bbedac4f01e7e79a9c99d04ba08cf";
   
   // With:
   const projectId = process.env.REACT_APP_IPFS_PROJECT_ID;
   const projectSecret = process.env.REACT_APP_IPFS_PROJECT_SECRET;
   ```

   b. In `api/ipfs-proxy.js`:
   ```javascript
   // Replace:
   const projectId = "2DBM8M1VMImQ5vl4IQxYQzI8uKg";
   const projectSecret = "f34bbedac4f01e7e79a9c99d04ba08cf";
   
   // With:
   const projectId = process.env.REACT_APP_IPFS_PROJECT_ID;
   const projectSecret = process.env.REACT_APP_IPFS_PROJECT_SECRET;
   ```

   c. In `server.js`:
   ```javascript
   // Replace:
   const projectId = "2DBM8M1VMImQ5vl4IQxYQzI8uKg";
   const projectSecret = "f34bbedac4f01e7e79a9c99d04ba08cf";
   
   // With:
   const projectId = process.env.REACT_APP_IPFS_PROJECT_ID || "";
   const projectSecret = process.env.REACT_APP_IPFS_PROJECT_SECRET || "";
   
   // Add validation:
   if (!projectId || !projectSecret) {
     console.error("IPFS credentials not configured. Please set environment variables.");
   }
   ```

   d. In `src/setupProxy.js`:
   ```javascript
   // Replace:
   const projectId = "2DBM8M1VMImQ5vl4IQxYQzI8uKg";
   const projectSecret = "f34bbedac4f01e7e79a9c99d04ba08cf";
   
   // With:
   const projectId = process.env.REACT_APP_IPFS_PROJECT_ID;
   const projectSecret = process.env.REACT_APP_IPFS_PROJECT_SECRET;
   ```

3. Add the `.env` file to `.gitignore` to prevent committing secrets:
   ```
   # .gitignore
   .env
   .env.local
   .env.development
   .env.production
   ```

4. Remove environment files from git tracking if they exist:
   ```bash
   git rm --cached .env .env.development .env.production
   ```

5. For production deployment, set these environment variables in your hosting platform (Vercel, etc.):
   - Go to your project in the Vercel dashboard
   - Navigate to Settings → Environment Variables
   - Add the variables REACT_APP_IPFS_PROJECT_ID and REACT_APP_IPFS_PROJECT_SECRET with their respective values
   - Redeploy your application

### Additional Steps for This Project

1. **Rotate IPFS Credentials Immediately**:
   - Log in to your Infura dashboard
   - Generate new IPFS project credentials to replace the exposed ones
   - Update your local .env file and Vercel environment variables with the new credentials
   - The existing keys should be considered compromised and deactivated

2. **Verify All Files**:
   - Check for any additional files that might contain hardcoded credentials
   - Some development tools may cache environment variables in build files

3. **Add Error Handling**:
   - Implement proper error handling for cases where IPFS credentials are missing
   - Add user-friendly error messages without exposing sensitive details

4. **Update Documentation**:
   - Update README.md to describe the environment variables needed
   - Provide clear setup instructions for new developers

### Security Best Practices

- Never commit sensitive credentials to version control
- Rotate API keys periodically and after suspected exposure
- Use different API keys for development and production
- Implement proper access controls for all services
- Consider using a secrets management service for production
- Add validation to ensure the application fails securely if credentials are missing