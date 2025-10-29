# Fix Images Not Displaying

## Step 1: Run SQL in phpMyAdmin

1. Open phpMyAdmin at http://localhost/phpmyadmin
2. Select `grantes_db` database
3. Click the SQL tab
4. Copy and paste this SQL:

```sql
USE grantes_db;

-- Change images column from TEXT to MEDIUMTEXT (supports up to 16MB)
ALTER TABLE admin_posts MODIFY COLUMN images MEDIUMTEXT;
```

5. Click "Go"
6. You should see "Query OK"

## Step 2: Test

1. Refresh your GranTES page
2. Log in as Admin
3. Create a new post with an image
4. The image should now display!

## Why this fixes it:

- The `TEXT` field type can only hold up to 65,535 characters
- Base64-encoded images are usually larger than this
- `MEDIUMTEXT` can hold up to 16MB, which is enough for most images
- This allows full images to be stored and displayed

## If still not working:

Check the browser console (F12) for any errors. Look for "Post images:" in the console to see what's being retrieved from the database.

