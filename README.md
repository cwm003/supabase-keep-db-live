# 🔌 Supabase Keep Database Live

Automated GitHub Actions workflow to keep your Supabase database active and prevent it from being paused due to inactivity.

## 📖 Overview

Supabase pauses databases after periods of inactivity (especially on free-tier projects) to conserve resources. This project uses GitHub Actions to periodically "ping" your Supabase databases, keeping them active and preventing automatic pausing.

## ✨ Features

- 🤖 **Automated Pings**: Runs on a schedule (configurable)
- 🎯 **Manual Triggers**: Can be triggered manually from GitHub UI
- 🌐 **Multiple Databases**: Keep several Supabase databases alive with one workflow
- 📊 **Detailed Logging**: See exactly what's happening with each ping
- 🔒 **Secure**: Uses GitHub Secrets to store sensitive credentials
- ⚡ **No Table Required**: Uses Supabase's auth endpoint - works globally without needing specific tables
- 📈 **Summary Reports**: Get a clear summary of all pings with success/failure counts

## 🚀 Setup Instructions

### 1. Fork or Clone This Repository

```bash
git clone https://github.com/yourusername/supabase-keep-db-live.git
cd supabase-keep-db-live
```

### 2. Configure GitHub Secrets

You need to add your Supabase database configurations as a GitHub repository secret:

1. Go to your GitHub repository
2. Navigate to **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Create a secret named: **`SUPABASE_CONFIGS`**
5. Set the value as a **JSON array** of your database configurations

> 📖 **Quick Setup Guide**: See [SETUP-EXAMPLE.md](SETUP-EXAMPLE.md) for ready-to-use templates and detailed instructions!

#### Format for Single Database:
```json
[{"name":"Production DB","url":"https://xxxxx.supabase.co","key":"eyJhbGc..."}]
```

#### Format for Multiple Databases:
```json
[
  {
    "name": "Production DB",
    "url": "https://xxxxx.supabase.co",
    "key": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  },
  {
    "name": "Staging DB",
    "url": "https://yyyyy.supabase.co",
    "key": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  },
  {
    "name": "Development DB",
    "url": "https://zzzzz.supabase.co",
    "key": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
]
```

#### Where to Find Your Credentials:
- **URL**: Supabase Dashboard → Settings → API → Project URL
- **Key**: Supabase Dashboard → Settings → API → Service Role Key (or anon key)

> 💡 **Tip**: The `name` field is optional but recommended for easier identification in logs
> 
> ⚠️ **Important**: Use the **service role key** for full access, or the **anon key** if you prefer. The workflow uses the auth endpoint which doesn't require table-specific permissions.

### 3. Customize the Schedule (Optional)

By default, the workflow runs **every Monday and Thursday at 9:00 AM UTC**. To change this:

Edit `.github/workflows/ping-supabase.yml`:

```yaml
on:
  schedule:
    - cron: '0 9 * * 1,4'  # Modify this line
```

#### Common Cron Schedule Examples:

| Schedule | Cron Expression |
|----------|----------------|
| Every day at 9:00 AM UTC | `0 9 * * *` |
| Every 12 hours | `0 */12 * * *` |
| Every Monday at 8:00 AM UTC | `0 8 * * 1` |
| Twice a week (Mon, Thu) | `0 9 * * 1,4` |
| Every 6 hours | `0 */6 * * *` |

> 💡 Use [crontab.guru](https://crontab.guru/) to help create custom cron schedules.

### 4. Test the Workflow

You can manually test the workflow:

1. Go to your GitHub repository
2. Navigate to **Actions** tab
3. Select **Ping Supabase to Prevent Pausing**
4. Click **Run workflow** → **Run workflow**

Check the logs to ensure everything is working correctly!

## 📁 Project Structure

```
supabase-keep-db-live/
├── .github/
│   └── workflows/
│       └── ping-supabase.yml    # GitHub Actions workflow
├── env.example                   # Environment variables template
├── .gitignore                    # Git ignore rules
├── package.json                  # NPM dependencies
├── ping-supabase.js             # Local testing script
├── README.md                     # Main documentation
└── SETUP-EXAMPLE.md             # Quick setup guide with templates
```

## 🔧 Local Testing (Optional)

If you want to test the ping functionality locally:

1. Install dependencies:
```bash
npm install
```

2. Create a `.env` file (copy from `env.example`):
```bash
cp env.example .env
```

3. Edit `.env` and update `SUPABASE_CONFIGS` with your database configurations:
```bash
SUPABASE_CONFIGS='[{"name":"My DB","url":"https://xxxxx.supabase.co","key":"your-key"}]'
```

4. Run the test script:
```bash
npm run ping
# or directly:
node ping-supabase.js
```

You should see output like:
```
🚀 Starting to ping 1 database(s)...
⏰ Timestamp: 2025-10-11T12:00:00.000Z
════════════════════════════════════════════════════════════

📊 [1/1] Pinging: My DB
📍 URL: https://xxxxx.supabase.co
✅ Success! Response time: 245ms

════════════════════════════════════════════════════════════
📈 Summary:
   ✅ Successful: 1
   ❌ Failed: 0
   📊 Total: 1

🎉 All databases pinged successfully!
```

## 📊 Monitoring

To check if your workflow is running successfully:

1. Go to the **Actions** tab in your GitHub repository
2. Look for recent workflow runs
3. Click on any run to see detailed logs

## 🤝 Contributing

Feel free to open issues or submit pull requests if you have suggestions for improvements!

## 📄 License

MIT License - feel free to use this for your own projects!

## 🙏 Credits

Based on the article by [Jack Pritom Soren](https://github.com/jps27cse)

## ❓ FAQ

### Do I need to specify table names?

**No!** This workflow uses Supabase's auth endpoint (`auth.getSession()`) which exists in every Supabase project. You don't need to configure any specific tables. It's truly global and works with any Supabase database.

### Can I ping multiple databases at once?

**Yes!** Just add multiple database configurations to your `SUPABASE_CONFIGS` secret. The workflow will ping each one sequentially and provide a summary report.

### Why use service role key instead of anon key?

Either works! Since we're using the auth endpoint (not querying tables), you can use either the service role key or the anon key. The service role key is more reliable as it has full access.

### How often should I ping the database?

Twice a week is usually sufficient. More frequent pings may not be necessary and could consume your GitHub Actions minutes (though the free tier is usually more than enough for this use case).

### Will this work with other databases?

This specific implementation is for Supabase. For other databases, you'd need to modify the connection and query logic.

### Does this affect my Supabase usage limits?

Each ping counts as an API request, but since we're only checking the auth endpoint a couple of times per week, the impact is minimal and well within free-tier limits.

### What if one database fails but others succeed?

The workflow will continue pinging all databases even if one fails. At the end, you'll see a summary showing which succeeded and which failed. The workflow will exit with an error if any database fails, so you'll be notified.

## 📞 Support

If you encounter any issues, please open an issue on GitHub!

---

**Happy coding! 🚀**

