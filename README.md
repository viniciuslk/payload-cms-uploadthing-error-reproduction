# Payload CMS UploadThing Error Reproduction

This repository is created to reproduce and document an issue with the UploadThing storage plugin in Payload CMS. The error occurs during media file creation and transformation.

## Issue Description

When attempting to create new media files using the UploadThing storage plugin, the system fails with an "unable to extract key from doc" error. This occurs during the initial file upload and creation process in the media collection.

### Error Details

```
[ERROR] Error deleting file: aspect-ratio-9-16-2.jpg - unable to extract key from doc
[ERROR] Error deleting file: aspect-ratio-9-16-2-300x452.jpg - unable to extract key from doc
[ERROR] Error deleting file: aspect-ratio-9-16-2-500x500.jpg - unable to extract key from doc
[ERROR] Error deleting file: aspect-ratio-9-16-2-600x904.jpg - unable to extract key from doc
[ERROR] Error deleting file: aspect-ratio-9-16-2-1200x630.jpg - unable to extract key from doc
[ERROR] There was an error while uploading files corresponding to the collection media with filename aspect-ratio-9-16-3.jpg
```

## Detailed Reproduction Steps

1. **Project Setup**

   ```bash
   git clone https://github.com/yourusername/payload-cms-uploadthing.git
   cd payload-cms-uploadthing
   pnpm install
   ```

2. **Environment Configuration**
   Create a `.env` file with:

   ```env
   PAYLOAD_SECRET=your_payload_secret
   DATABASE_URI=file:./payload-cms-uploadthing.db
   UPLOADTHING_TOKEN=your_uploadthing_token
   ```

3. **Start Development Server**

   ```bash
   pnpm dev
   ```

4. **Admin Panel Setup**

   - Open http://localhost:3000/admin
   - Create your first admin user if prompted
   - Log in to the admin panel

5. **Media Upload Test**

   - Navigate to the Media collection in the admin panel
   - Click "Create New"
   - Select an image file (e.g., a JPG or PNG)
   - Fill in the required fields (title, alt text, etc.)
   - Click "Save"

6. **Error Observation**

   - Watch the terminal/console output
   - The error will appear when the system attempts to process the uploaded file
   - The error occurs during the file transformation process
   - The file upload will fail to complete

7. **Error Verification**
   - Check the browser's network tab for failed requests
   - Verify the error messages in the console match the expected error pattern
   - Confirm that the file was not properly stored in UploadThing

## Configuration

The error occurs with the following UploadThing storage plugin configuration:

```typescript
uploadthingStorage({
  collections: {
    media: true,
  },
  options: {
    token: process.env.UPLOADTHING_TOKEN,
    acl: 'public-read',
  },
})
```

## Expected Behavior

The media file should be successfully uploaded and created in the UploadThing storage, with all necessary file transformations (different sizes) being processed correctly.

## Actual Behavior

The system fails to properly handle the file key extraction during the creation process, resulting in errors when trying to process file transformations.

---

This repository is created for debugging purposes to help resolve the UploadThing storage plugin issue in Payload CMS.
