# 🍼 Voice-Activated Bottle Feed Tracker

A hands-free baby bottle feeding tracker designed for sleep-deprived parents. Simply press your iPhone's Action Button, speak the feeding details, and the app automatically logs the amount, time, and date to a shared tracker. Built with iOS Shortcuts integration, a progressive web app interface, and Google Sheets backend for real-time data sharing between caregivers. All times are captured in Eastern Standard Time (EST) with a customizable 6am-6am daily tracking window to match real-world feeding schedules.

Features a beautiful soft pastel baby theme with floating clouds and automatic dark mode for nighttime feedings.

## Features

### Voice-Activated Logging
- **Action Button Integration**: Press your iPhone 15 Pro's Action Button to instantly start voice logging
- **Natural Language Parsing**: Say "120 milliliters at 3pm" or "90 ml yesterday at 9:30am" and the app automatically extracts amount, time, and date
- **Hands-Free Operation**: Perfect for middle-of-the-night feedings when you don't want to fumble with your phone
- **EST Timezone**: All times are automatically captured and stored in Eastern Standard Time

### Real-Time Stats Dashboard
- **Today's Feeds**: Count of all feedings in the current 24-hour window (6am-6am EST)
- **Today's Total**: Total milliliters consumed in the current tracking window
- **Last Feed Time**: When the most recent feeding occurred (displayed in 12-hour format)
- **Time Since Last Feed**: Live countdown showing hours and minutes since the last feeding
- **6am-6am Tracking Window**: Stats reset at 6am EST every day to match typical daily feeding cycles

### 6-Day History View
- **Historical Overview**: Table showing the last 6 days of feeding data
- **Feed Counts**: Number of feedings per day
- **Daily Totals**: Total milliliters consumed each day
- **Same Tracking Window**: Uses the same 6am-6am EST logic as current stats
- **Color-Coded Data**: Teal dates, lavender feed counts, coral totals

### Manual Entry Options
- **Custom Amount Entry**: Manually enter any amount with "Log Now" button
- **Advanced Editing**: Expandable section for custom date/time input
- **Quick Access**: Always available below the history table

### Beautiful Pastel Theme
- **Soft Colors**: Yellow, peach, and lavender gradient background
- **10 Floating Clouds**: Realistic animated clouds drift across the screen
- **Rounded Design**: Modern 15-25px rounded corners throughout
- **Smooth Animations**: Hover effects and transitions on all interactive elements
- **Gradient Accents**: Purple, pink, coral, and mint green highlights

### Automatic Dark Mode
- **Time-Based Activation**: Turns on at 8:00 PM EST, off at 6:00 AM EST
- **Eye Comfort**: Reduces strain during nighttime feedings
- **Muted Palette**: Deep blue-purple background with soft lavender and pink accents
- **Dimmed Clouds**: Clouds reduce to 15% opacity at night
- **Automatic Switching**: Checks every minute to ensure proper timing

### Shared Data Across Devices
- **Google Sheets Backend**: All feeding data is stored in a shared Google Sheet
- **Multi-User Support**: Both parents can log feeds and see the same data in real-time
- **Data Persistence**: Never lose your feeding history - it's all safely stored in the cloud
- **Export Ready**: Access raw data directly in Google Sheets for pediatrician visits or personal analysis

### Clean, Mobile-Optimized UI
- **Progressive Web App**: Add to home screen for a native app-like experience
- **Responsive Design**: Works perfectly on any iPhone or Android device
- **Collapsible Feed Log**: Click header to expand/collapse full feed history
- **Organized Layout**: Stats → History → Manual Entry → Feed Log
- **Visual Polish**: Gradient text effects, soft shadows, smooth transitions

## How It Works

### Architecture
This system uses a three-component architecture:

1. **iOS Shortcut**: Captures voice input via dictation and sends it to the web app via URL parameters
2. **GitHub Pages Web App**: Parses voice input, manages UI, communicates with Google Sheets backend, and handles EST timezone conversion
3. **Google Apps Script API**: Handles reading and writing feeding data to/from Google Sheets

### Timezone Handling
- **All timestamps use EST**: The app converts the device's local time to Eastern Standard Time
- **Consistent across timezones**: Works correctly even if parents are traveling in different timezones
- **Calendar date at midnight**: Feed dates change at midnight EST (normal calendar day)
- **Stats reset at 6am EST**: Daily tracking window runs from 6am to 6am the next day

### Example: How 6am-6am Tracking Works
```
Feb 15, 11:30 PM → Logged as: Feb 15, 11:30 PM | Counts in: Feb 15 stats (before 6am reset)
Feb 16, 2:00 AM  → Logged as: Feb 16, 2:00 AM  | Counts in: Feb 15 stats (still before 6am)
Feb 16, 6:00 AM  → Stats reset                 | New tracking window begins
Feb 16, 7:00 AM  → Logged as: Feb 16, 7:00 AM  | Counts in: Feb 16 stats (after 6am reset)
```

### Voice Parsing Capabilities
The app intelligently parses natural language input:

- **Amounts**: "120 milliliters", "90 ml", "4 ounces" (auto-converts to ml)
- **Times**: "at 3pm", "at 9:30am", "15:00", "now"
- **Dates**: "today", "yesterday", "February 10th", "2/10"
- **Combined**: "185 ml at 9:30am yesterday" parses all three components

### Data Flow
1. User presses Action Button → iOS dictation starts
2. User speaks feeding details → Shortcut captures text
3. Shortcut opens tracker URL with voice data as parameter
4. Web app parses voice input and extracts amount/time/date (converts to EST)
5. Web app sends data to Google Apps Script via POST request
6. Apps Script saves to Google Sheets with EST timestamp
7. Web app refreshes and displays updated stats and history

## Setup Instructions

### Prerequisites
- iPhone 15 Pro or newer (for Action Button integration)
- Google account
- GitHub account

### Step 1: Create Google Sheet
1. Go to [sheets.google.com](https://sheets.google.com)
2. Create a new blank spreadsheet named "Bottle Feed Log"
3. Add column headers: `Date` | `Time` | `Amount` | `Timestamp`

### Step 2: Deploy Google Apps Script
1. In your Google Sheet, click **Extensions → Apps Script**
2. Delete default code and paste the code from `google-apps-script-STATS-FIXED.js`
3. Click **Save** and name it "Bottle Feed API"
4. Click **Deploy → New deployment**
5. Click gear icon → Select **Web app**
6. Configure:
   - Execute as: **Me**
   - Who has access: **Anyone**
7. Click **Deploy** and authorize the script
8. Copy the Web App URL (you'll need this for the next step)

**Important**: The Apps Script includes date/time formatting to ensure compatibility with EST timezone tracking and proper data structure for the 6am-6am window calculations.

### Step 3: Deploy Web App to GitHub Pages
1. Fork or create a new repository on GitHub
2. Upload `bottle-tracker-EST-FINAL-FIX.html` as `index.html`
3. Edit the HTML file and replace `GOOGLE_SCRIPT_URL` with your Apps Script Web App URL from Step 2
4. Go to repository **Settings → Pages**
5. Set source to deploy from branch **main**
6. Wait 2-3 minutes for deployment
7. Your tracker will be live at: `https://[username].github.io/[repo-name]/`

### Step 4: Create iOS Shortcut
1. Open **Shortcuts** app on iPhone
2. Create new shortcut named "Log Bottle Feed"
3. Add these actions in order:
   - **Dictate Text** (Stop Listening: After Pause)
   - **Text**: `https://[your-github-pages-url]/?trigger=` + [Dictated Text variable]
   - **URL** (Combine text to URL)
   - **Open in Chrome** (or Safari): Open the URL
4. Save the shortcut

**Note**: The URL parameter method ensures voice data is passed correctly to the web app for EST conversion and parsing.

### Step 5: Assign to Action Button
1. Go to **Settings → Action Button**
2. Select **Shortcut**
3. Choose **Log Bottle Feed**

### Step 6: Set Up for Second Parent
1. Share your GitHub Pages URL with your partner
2. They can add it to their home screen: Share button → Add to Home Screen
3. They create the same iOS shortcut on their device
4. Both of you will see the same shared data from Google Sheets

**Important**: Both parents will see the same 6am-6am tracking window regardless of their device's timezone, since all calculations use EST.

## Usage

### Voice Logging
1. Press your Action Button
2. Wait for dictation to start
3. Say the feeding details (e.g., "120 milliliters at 3pm")
4. The app opens and logs the feed automatically in EST

### Manual Logging
1. Open the tracker app
2. Tap a quick-log button (45ml, 70ml, 130ml, 180ml) OR
3. Enter a custom amount and tap "Log Feed"
4. Feed is logged with current EST timestamp
5. Optionally edit date/time before logging

### Viewing History
- Scroll through the Feed Log section to see all past feedings
- Each entry shows date (EST calendar date), time (12-hour format), and amount
- Tap "Delete" to remove an entry
- Tap "Clear All" to delete all feeding data (affects both parents!)

### Understanding the Stats
- **Today's Feeds**: Counts feeds from 6am EST today until 6am EST tomorrow
- **Today's Total**: Sum of milliliters in the current 6am-6am window
- **Last Feed**: Time of most recent feed (updates in real-time)
- **Time Since Last**: Live countdown (updates as time passes)

## Troubleshooting

### Voice input not parsing correctly
- Make sure you say "milliliters" or "ml" (required for parsing)
- Specify time with "at" (e.g., "at 3pm" not just "3pm")
- Check the green status message to see what was parsed
- All times are converted to EST automatically

### Stats showing 0 or incorrect counts
- Verify you're looking at the app after 6am EST (stats reset then)
- Check Google Sheets to confirm data is being saved
- Remember: feeds before 6am EST count toward yesterday's stats
- Clear browser cache and force reload

### Time showing incorrectly or "NaNm"
- Check that Google Sheet column B contains time in HH:MM format (e.g., "09:30", "13:00")
- Verify times don't have leading apostrophes in Google Sheets
- Update to latest version of the web app (includes EST timezone fixes)

### Date showing wrong day (e.g., tomorrow's date)
- Ensure you're using the EST-FINAL-FIX version of the HTML file
- The app should show EST calendar date, not your device's local date
- A feed at 11pm EST should show today's date, not tomorrow's

### Shortcut not triggering
- Verify the URL includes `?trigger=` parameter
- Make sure "URL" (combine to URL) step is included in shortcut
- Check that Chrome (or Safari) is set to open the URL
- Test by running the shortcut manually from Shortcuts app first

## Technology Stack

- **Frontend**: Vanilla JavaScript, HTML5, CSS3
- **Hosting**: GitHub Pages (static site hosting)
- **Backend**: Google Apps Script (serverless API)
- **Database**: Google Sheets
- **Mobile Integration**: iOS Shortcuts with Action Button
- **Voice Input**: iOS native dictation API
- **Timezone**: Intl.DateTimeFormat API for EST conversion

## File Structure

```
bottle-feed-tracker/
├── index.html                           # Main web app (use PASTEL-WITH-AUTO-DARK-MODE version)
├── google-apps-script-STATS-FIXED.js   # Backend API with date/time formatting
├── README.md                            # This file
└── GOOGLE-SHEETS-SETUP-GUIDE.md        # Detailed setup walkthrough
```

## Privacy & Data

- All feeding data is stored in your personal Google Sheet
- Only you and people with access to your Google Sheet can see the data
- The web app runs entirely in the browser (no data sent to third parties)
- Google Apps Script API is deployed under your Google account
- No analytics, tracking, or data collection
- Timezone conversion happens client-side (in the browser)

## Key Features Explained

### Why EST Timezone?
- Ensures consistency when parents are in different timezones
- Matches typical US East Coast usage
- Can be modified in code to use different timezone (e.g., PST, CST)

### Why 6am-6am Tracking Window?
- Aligns with typical daily feeding cycles (morning starts at 6am, not midnight)
- A 2am feeding counts toward the previous "day" as parents still consider it "tonight"
- Matches how pediatricians often track daily intake
- Stats reset at a practical time when baby is likely sleeping

### Why 6-Day History Instead of Quick Buttons?
- Provides better overview of feeding patterns over time
- Shows trends and helps identify changes in routine
- More useful than quick-log buttons for established feeding amounts
- Manual entry still available for custom amounts

### Why Automatic Dark Mode?
- Reduces eye strain during nighttime feedings (8pm-6am EST)
- Softer colors won't wake baby or disrupt sleep
- Automatic switching means one less thing to think about
- Dark mode optimized for dim lighting conditions

### Why Pastel Theme with Clouds?
- Soft, nursery-appropriate aesthetic
- Gentle colors reduce stress during feeding sessions
- Floating clouds add whimsy and visual interest
- Professional look suitable for daily use

### Customization Options
Both timezone and daily window can be customized by editing these values in the code:
- **Timezone**: Change `'America/New_York'` to your desired timezone
- **Daily Reset Time**: Change `6` in the stats calculation to your preferred hour (0-23)
- **Dark Mode Hours**: Modify the `checkDarkMode()` function to change activation times
- **Cloud Count**: Add or remove cloud divs in HTML and corresponding CSS
- **Theme Colors**: Update gradient values in CSS to match your preference

## Future Enhancement Ideas

### Multi-Child Dashboard
- 👶 Support for tracking multiple children
- 🎨 Color-coded sections per child
- 📊 Consolidated family view

### Additional Tracking
- 💊 Medicine and vitamin logging
- 💤 Sleep tracking (naps and nighttime)
- 🍽️ Solid food introduction tracker
- 🚼 Diaper change logs
- 🌡️ Temperature and illness tracking
- ⚖️ Weight and height milestones
- 🦷 Teething tracker

### Analytics & Insights
- 📈 Graphs showing feeding patterns over time
- 📊 Weekly/monthly summary reports
- 📉 Growth charts
- 🔔 Smart predictions for next feeding time
- ⚠️ Alerts if baby hasn't fed in X hours

### Family Features
- 📧 Automated daily/weekly email summaries
- 📤 Export to PDF for pediatrician appointments
- 📅 Appointment and vaccination reminders
- 👨‍👩‍👧 Shared notes and caregiver handoff
- 📷 Photo and milestone gallery

### UI Enhancements
- 🌙 Manual dark mode toggle option
- 🌍 Automatic timezone detection
- 📱 Android Quick Settings Tile integration
- ⚙️ Customizable daily reset time in settings
- 🎨 Multiple theme options

## Contributing

This is a personal project, but feel free to fork and customize for your own needs! If you add cool features, consider sharing them.

## License

Free to use and modify for personal use.

## Acknowledgments

Built with love (and severe sleep deprivation) for new parents everywhere. Special thanks to Google Sheets for being a reliable free database and to Apple for the Action Button feature. May your nights be short and your baby's feeds be logged! 🌙👶

---

**Version**: 3.0 (Pastel Theme + Auto Dark Mode)  
**Last Updated**: February 2026  
**Tested On**: iPhone 15 Pro, iOS 18+, Chrome & Safari  
**Timezone**: Eastern Standard Time (EST/EDT)  
**Daily Window**: 6:00 AM - 6:00 AM EST  
**Theme**: Soft Pastel Baby with 10 Floating Clouds  
**Dark Mode**: Auto-activates 8:00 PM - 6:00 AM EST
