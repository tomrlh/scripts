# Auto-Accept Google Calendar Meetings

Automatically accept all incoming meeting invitations in your Google Calendar using Google Apps Script.

## Why Use This?

- Never miss a meeting because you forgot to click "Accept"
- Save time on repetitive clicking
- Ensure your calendar always reflects your actual availability

## Setup Instructions

### Step 1: Open Google Apps Script

1. Go to [script.google.com](https://script.google.com)
2. Click **"New project"**

### Step 2: Add the Script

Delete any existing code and paste the following:

```javascript
function autoAcceptMeetings() {
  const calendar = CalendarApp.getDefaultCalendar();
  const now = new Date();
  const endDate = new Date(now.getTime() + 7 * 24 * 60 * 60 * 1000); // next 7 days
  
  const events = calendar.getEvents(now, endDate);
  let accepted = 0;
  
  events.forEach(event => {
    const status = event.getMyStatus();
    if (status === CalendarApp.GuestStatus.INVITED || 
        status === CalendarApp.GuestStatus.MAYBE) {
      event.setMyStatus(CalendarApp.GuestStatus.YES);
      accepted++;
      Logger.log('Accepted: ' + event.getTitle() + ' at ' + event.getStartTime());
    }
  });
  
  Logger.log('Total accepted meetings: ' + accepted);
}
```

### Step 3: Save the Project

1. Click the **Save** icon (💾) or press `Ctrl+S` / `Cmd+S`
2. Name your project (e.g., "Auto Accept Meetings")

### Step 4: Grant Permissions

1. Click **Run** (▶️ button)
2. Click **"Review permissions"**
3. Select your Google account
4. Click **"Advanced"** → **"Go to Auto Accept Meetings (unsafe)"**
5. Click **"Allow"**

> **Note:** Google shows the "unsafe" warning because this is a personal script, not a verified app. The script only accesses your own calendar.

### Step 5: Set Up Automatic Trigger

1. In the left sidebar, click **Triggers** (⏰ clock icon)
2. Click **"+ Add Trigger"**
3. Configure the trigger:
   - **Function to run:** `autoAcceptMeetings`
   - **Deployment:** `Head`
   - **Event source:** `Time-driven`
   - **Type of time-based trigger:** `Hour timer`
   - **Hour interval:** `Every hour`
4. Click **"Save"**

## Configuration Options

### Change the Time Window

To scan more or fewer days ahead, modify the `7` in this line:

```javascript
const endDate = new Date(now.getTime() + 7 * 24 * 60 * 60 * 1000);
```

| Days | Value |
|------|-------|
| 3 days | `3 * 24 * 60 * 60 * 1000` |
| 7 days | `7 * 24 * 60 * 60 * 1000` |
| 14 days | `14 * 24 * 60 * 60 * 1000` |
| 30 days | `30 * 24 * 60 * 60 * 1000` |

### Change Trigger Frequency

For faster response to new invites, set the trigger to run every 15 minutes instead of every hour.

## Viewing Logs

To see which meetings were accepted:

1. Go to [script.google.com](https://script.google.com)
2. Open your project
3. Click **Executions** in the left sidebar
4. Click on any execution to view the logs

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Script doesn't run | Check that the trigger is enabled in the Triggers panel |
| Meetings not being accepted | Verify you granted calendar permissions |
| Permission errors | Re-run the script manually and re-authorize |

## Uninstalling

1. Go to [script.google.com](https://script.google.com)
2. Open the project
3. Click **Triggers** → Delete all triggers
4. Optionally, delete the entire project

## License

MIT — Use freely.
