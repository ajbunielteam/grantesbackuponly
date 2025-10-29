# MySQL Engagement Setup

This guide will help you set up the engagement (likes, comments, shares) functionality in your MySQL database.

## Step 1: Run the Setup Script

Open your browser and navigate to:
```
http://localhost/grantes/api/setup_engagement_columns.php
```

This will automatically add the required columns to your `admin_posts` table:
- `likes` (INT, default 0)
- `shares` (INT, default 0)  
- `comments` (TEXT, default '[]')

## Step 2: Verify the Setup

You can check if the columns were added successfully by running this in phpMyAdmin or MySQL command line:

```sql
USE grantes_db;
DESCRIBE admin_posts;
```

You should see columns named `likes`, `shares`, and `comments`.

## Step 3: Test the Functionality

1. Login as Administrator
2. Create a new post
3. Click Like, Comment, or Share buttons
4. The engagement data will be saved to MySQL database

## Manual SQL Alternative

If you prefer to run the SQL manually, execute:

```sql
USE grantes_db;

ALTER TABLE admin_posts 
ADD COLUMN IF NOT EXISTS likes INT DEFAULT 0;

ALTER TABLE admin_posts 
ADD COLUMN IF NOT EXISTS shares INT DEFAULT 0;

ALTER TABLE admin_posts 
ADD COLUMN IF NOT EXISTS comments TEXT DEFAULT '[]';

UPDATE admin_posts 
SET likes = COALESCE(likes, 0),
    shares = COALESCE(shares, 0),
    comments = COALESCE(NULLIF(comments, ''), '[]')
WHERE likes IS NULL OR shares IS NULL OR comments IS NULL OR comments = '';
```

## Database Connection

The system uses these database credentials (from `config/database.php`):
- Host: localhost
- User: root
- Password: (empty)
- Database: grantes_db

Make sure your MySQL server is running in XAMPP!

## Troubleshooting

If you get errors about columns not existing:
1. Make sure you ran the setup script
2. Check that your database connection is working
3. Verify the table `admin_posts` exists in the `grantes_db` database

If likes/comments/shares are not updating:
1. Check browser console for JavaScript errors
2. Check the API response in Network tab
3. Verify the API endpoint is accessible at: `http://localhost/grantes/api/update_post_engagement.php`

