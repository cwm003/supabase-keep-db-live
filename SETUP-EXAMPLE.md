# Quick Setup Examples

## GitHub Secret Configuration

Copy one of these templates and customize it with your Supabase credentials.

### Template: Single Database

```json
[
  {
    "name": "Production DB",
    "url": "https://xxxxxxxxxxxxx.supabase.co",
    "key": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.your-key-here"
  }
]
```

### Template: Multiple Databases

```json
[
  {
    "name": "Production DB",
    "url": "https://xxxxxxxxxxxxx.supabase.co",
    "key": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.your-prod-key"
  },
  {
    "name": "Staging DB",
    "url": "https://yyyyyyyyyyy.supabase.co",
    "key": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.your-staging-key"
  },
  {
    "name": "Development DB",
    "url": "https://zzzzzzzzzzz.supabase.co",
    "key": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.your-dev-key"
  }
]
```

### Template: Minimal (Without Names)

```json
[
  {
    "url": "https://xxxxxxxxxxxxx.supabase.co",
    "key": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.your-key-here"
  }
]
```

---

## How to Add This to GitHub

1. Go to your repository on GitHub
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Name: `SUPABASE_CONFIGS`
5. Value: Paste one of the JSON templates above (with your actual credentials)
6. Click **Add secret**

---

## Where to Find Your Credentials

### Supabase Project URL
1. Go to your Supabase project dashboard
2. Click **Settings** (gear icon)
3. Click **API**
4. Copy the **Project URL**
   - Example: `https://abcdefghijk.supabase.co`

### Supabase API Key
1. In the same **API** settings page
2. Copy either:
   - **anon/public key** - For basic access (recommended for most cases)
   - **service_role key** - For full access (bypasses RLS)

⚠️ **Never commit these keys to your repository!** Always use GitHub Secrets.

---

## Testing Your Configuration

After setting up the secret, test it manually:

1. Go to **Actions** tab in your GitHub repository
2. Select **Ping Supabase to Prevent Pausing** workflow
3. Click **Run workflow** → **Run workflow**
4. Wait a few seconds and check the logs

You should see:
```
🚀 Starting to ping X database(s)...
📊 [1/X] Pinging: Production DB
✅ Success! Response time: XXXms
...
🎉 All databases pinged successfully!
```

---

## Common Issues

### ❌ Error: "Missing SUPABASE_CONFIGS secret"
- Make sure you created the secret with the exact name: `SUPABASE_CONFIGS`
- Check that the secret is in the correct repository

### ❌ Error: "JSON parse error"
- Verify your JSON is valid (use a JSON validator)
- Make sure to use double quotes `"` not single quotes `'`
- Ensure all brackets are properly closed

### ❌ Error: "Missing url or key"
- Check that each database object has both `url` and `key` fields
- Verify there are no typos in the field names

---

## Pro Tips

💡 **Tip 1**: Use descriptive names to identify databases easily in logs
```json
{"name": "MyApp Production", "url": "...", "key": "..."}
```

💡 **Tip 2**: Start with one database, test it, then add more
```json
[{"name": "Test DB", "url": "...", "key": "..."}]
```

💡 **Tip 3**: Keep a copy of this configuration somewhere safe (encrypted)
- Consider using a password manager
- Don't store it in your code or plain text files

---

**Ready to go? Set up your secret and run the workflow!** 🚀

