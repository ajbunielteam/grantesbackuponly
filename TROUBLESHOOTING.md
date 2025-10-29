# Troubleshooting Guide - Like, Comment, and Share Not Working

## Quick Test Steps

### 1. First, check if the database has the `shares` column

Open phpMyAdmin (http://localhost/phpmyadmin) and run:
```sql
DESCRIBE admin_posts;
```

Look for a `shares` column. If it's missing, run:
```sql
ALTER TABLE admin_posts ADD COLUMN shares INT DEFAULT 0 AFTER likes;
ALTER TABLE admin_posts ADD COLUMN comments_count INT DEFAULT 0 AFTER comments;
```

### 2. Test the API endpoints

Open in browser: http://localhost/grantes/test_diagnostics.html

This will help you test:
- Database connection
- Get posts API
- Update engagement (like, comment, share) API

### 3. Check Browser Console

Open your browser's Developer Tools (F12) and check:
- Console tab for JavaScript errors
- Network tab to see if API calls are being made
- Look for any CORS errors or 404 errors

### 4. Verify Posts Exist

1. Log in as Admin
2. Create a test post
3. Check in phpMyAdmin if the post exists in `admin_posts` table
4. Note the post ID (e.g., 1, 2, 3...)

### 5. Test Student Functions

1. Log in as a Student
2. Go to Announcements
3. Try to like a post
4. Check browser console (F12) for errors

## Common Issues and Solutions

### Issue: "Post not found" error

**Solution**: 
- Make sure posts exist in the database
- Check that the post ID in the database matches what's being sent
- Run: `SELECT id, content FROM admin_posts LIMIT 5;` in phpMyAdmin

### Issue: API calls return empty or error

**Solution**:
1. Check XAMPP services are running (Apache and MySQL)
2. Verify the API file exists: `c:\xampp\htdocs\grantes\api\update_post_engagement.php`
3. Check PHP error log: `c:\xampp\htdocs\grantes\php_errors.log`

### Issue: CORS errors in browser

**Solution**: 
- The API headers should handle CORS, but if issues persist, check `api/config.php` exists

### Issue: Comments don't display

**Solution**:
1. Make sure the comment section is expanded (click the comment button)
2. Check that comments are being saved in the database
3. Run in phpMyAdmin: `SELECT id, comments FROM admin_posts WHERE id = 1;`

### Issue: Like count doesn't update

**Solution**:
1. Check database: `SELECT id, likes FROM admin_posts WHERE id = 1;`
2. Click like button and refresh the page
3. Check if the count increased in the database

## Debugging Commands

### Check if posts exist:
```sql
SELECT id, content, likes, shares, comments 
FROM admin_posts 
LIMIT 5;
```

### Check specific post:
```sql
SELECT * FROM admin_posts WHERE id = 1;
```

### Manual test of like:
```sql
UPDATE admin_posts SET likes = likes + 1 WHERE id = 1;
```

### Clear all comments:
```sql
UPDATE admin_posts SET comments = '[]';
```

## Expected Behavior

### When Student Clicks Like:
1. Should see "Post liked!" toast notification
2. Like count should increase immediately
3. Database should show updated like count

### When Student Clicks Comment:
1. Comment section expands
2. Input field appears
3. Type comment and press Enter or click send button
4. Comment appears in the list below
5. Comment count increases

### When Student Clicks Share:
1. Link copied to clipboard
2. Toast notification: "Post link copied to clipboard!"
3. Share count increases in database

## Manual Testing Checklist

- [ ] Database has `shares` column
- [ ] At least one post exists in database
- [ ] XAMPP Apache is running
- [ ] XAMPP MySQL is running
- [ ] Can access http://localhost/grantes
- [ ] Can log in as Admin
- [ ] Can log in as Student
- [ ] Posts appear on student announcements page
- [ ] Like button works
- [ ] Comment button works
- [ ] Share button works
- [ ] Browser console shows no errors

## Need More Help?

If still having issues, please provide:
1. Browser console errors (screenshot)
2. Network tab (F12 → Network → look for failed requests)
3. PHP error log output
4. Database table structure (DESCRIBE admin_posts;)

