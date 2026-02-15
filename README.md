# 🍼 Voice-Activated Bottle Feed Tracker

A hands-free baby bottle feeding tracker designed for sleep-deprived parents. Simply press your iPhone's Action Button, speak the feeding details, and the app automatically logs the amount, time, and date to a shared tracker. Built with iOS Shortcuts integration, a progressive web app interface, and Google Sheets backend for real-time data sharing between caregivers.

## Features

### Voice-Activated Logging
- **Action Button Integration**: Press your iPhone 15 Pro's Action Button to instantly start voice logging
- **Natural Language Parsing**: Say "120 milliliters at 3pm" or "90 ml yesterday at 9:30am" and the app automatically extracts amount, time, and date
- **Hands-Free Operation**: Perfect for middle-of-the-night feedings when you don't want to fumble with your phone

### Real-Time Stats Dashboard
- **Today's Feeds**: Count of all feedings logged today
- **Today's Total**: Total milliliters consumed today
- **Last Feed Time**: When the most recent feeding occurred
- **Time Since Last Feed**: Live countdown showing hours and minutes since the last feeding

### Quick Manual Entry
- **One-Tap Buttons**: Pre-configured buttons for common amounts (45ml, 70ml, 130ml, 180ml)
- **Custom Amount Entry**: Manually enter any amount with optional date/time editing
- **Advanced Editing**: Tap any feed to edit the date and time if needed

### Shared Data Across Devices
- **Google Sheets Backend**: All feeding data is stored in a shared Google Sheet
- **Multi-User Support**: Both parents can log feeds and see the same data in real-time
- **Data Persistence**: Never lose your feeding history - it's all safely stored in the cloud
- **Export Ready**: Access raw data directly in Google Sheets for pediatrician visits or personal analysis

### Clean, Mobile-Optimized UI
- **Progressive Web App**: Add to home screen for a native app-like experience
- **Responsive Design**: Works perfectly on any iPhone or Android device
- **Feed History Log**: Scrollable list of all past feedings with delete functionality
- **Clear All Option**: Reset all data when starting with a new baby or new tracking period

## How It Works

### Architecture
This system uses a three-component architecture:

1. **iOS Shortcut**: Captures voice input via dictation and sends it to the web app via URL parameters
2. **GitHub Pages Web App**: Parses voice input, manages UI, and communicates with the Google Sheets backend
3. **Google Apps Script API**: Handles reading and writing feeding data to/from Google Sheets

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
4. Web app parses voice input and extracts amount/time/date
5. Web app sends data to Google Apps Script via POST request
6. Apps Script saves to Google Sheets
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
2. Delete default code and paste the code from `google-apps-script.js`
3. Click **Save** and name it "Bottle Feed API"
4. Click **Deploy → New deployment**
5. Click gear icon → Select **Web app**
6. Configure:
   - Execute as: **Me**
   - Who has access: **Anyone**
7. Click **Deploy** and authorize the script
8. Copy the Web App URL (you'll need this for the next step)

### Step 3: Deploy Web App to GitHub Pages
1. Fork or create a new repository on GitHub
2. Upload `index.html` (the tracker web app file)
3. Edit the HTML file and replace `GOOGLE_SCRIPT_URL` with your Apps Script Web App URL
4. Go to repository **Settings → Pages**
5. Set source to deploy from branch **main**
6. Wait 2-3 minutes for deployment
7. Your tracker will be live at: `https://[username].github.io/[repo-name]/`

### Step 4: Create iOS Shortcut
1. Open **Shortcuts** app on iPhone
2. Create new shortcut named "Log Bottle Feed"
3. Add three actions:
   - **Dictate Text** (Stop Listening: After Pause)
   - **Text**: `https://[your-github-pages-url]/?trigger=` + [Dictated Text variable]
   - **URL**: Combine text to URL
   - **Open in Chrome** (or Safari): Open the URL
4. Save the shortcut

### Step 5: Assign to Action Button
1. Go to **Settings → Action Button**
2. Select **Shortcut**
3. Choose **Log Bottle Feed**

### Step 6: Set Up for Second Parent
1. Share your GitHub Pages URL with your partner
2. They can add it to their home screen: Share button → Add to Home Screen
3. They create the same iOS shortcut on their device
4. Both of you will see the same shared data from Google Sheets

## Usage

### Voice Logging
1. Press your Action Button
2. Wait for dictation to start
3. Say the feeding details (e.g., "120 milliliters at 3pm")
4. The app opens and logs the feed automatically

### Manual Logging
1. Open the tracker app
2. Tap a quick-log button (45ml, 70ml, 130ml, 180ml) OR
3. Enter a custom amount and tap "Log Feed"
4. Optionally edit date/time before logging

### Viewing History
- Scroll through the Feed Log section to see all past feedings
- Each entry shows date, time, and amount
- Tap "Delete" to remove an entry
- Tap "Clear All" to delete all feeding data

## Troubleshooting

### Voice input not parsing correctly
- Make sure you say "milliliters" or "ml" (required for parsing)
- Specify time with "at" (e.g., "at 3pm" not just "3pm")
- Check the green status message to see what was parsed

### Stats showing 0
- Check Google Sheets to confirm data is being saved
- Clear browser cache and force reload
- Check developer console for date comparison errors

### Time showing incorrectly
- Verify Google Sheet column B is formatted as plain text (not time format)
- Update Google Apps Script to latest version
- Check that times in Google Sheets don't have leading apostrophes

### Shortcut not triggering
- Verify the URL includes `?trigger=` parameter
- Make sure "Combine text to URL" step is included
- Check that Chrome (or Safari) is set to open the URL

## Technology Stack

- **Frontend**: Vanilla JavaScript, HTML5, CSS3
- **Hosting**: GitHub Pages (static site hosting)
- **Backend**: Google Apps Script (serverless API)
- **Database**: Google Sheets
- **Mobile Integration**: iOS Shortcuts with Action Button
- **Voice Input**: iOS native dictation API

## File Structure

```
bottle-feed-tracker/
├── index.html                      # Main web app (deployed to GitHub Pages)
├── google-apps-script.js          # Backend API code (deployed to Google Apps Script)
├── README.md                      # This file
└── GOOGLE-SHEETS-SETUP-GUIDE.md  # Detailed setup walkthrough
```

## Privacy & Data

- All feeding data is stored in your personal Google Sheet
- Only you and people with access to your Google Sheet can see the data
- The web app runs entirely in the browser (no data sent to third parties)
- Google Apps Script API is deployed under your Google account
- No analytics, tracking, or data collection

## Future Enhancement Ideas

- 📊 Graphs and trends showing feeding patterns over time
- ⏰ Push notifications for scheduled feedings
- 👶 Multi-baby support for twins or multiple children
- 💊 Medicine and vitamin tracking
- 🍼 Diaper change logging
- 📧 Automated daily/weekly email summaries
- 📱 Android Action Button alternative (Quick Settings Tile)
- 🌙 Dark mode for nighttime use
- 📤 Export to PDF for pediatrician appointments

## Contributing

This is a personal project, but feel free to fork and customize for your own needs! If you add cool features, consider sharing them.

## License

Free to use and modify for personal use.

## Acknowledgments

Built with love (and severe sleep deprivation) for new parents everywhere. May your nights be short and your baby's feeds be logged! 🌙👶

---

**Version**: 1.0  
**Last Updated**: February 2026  
**Tested On**: iPhone 15 Pro, iOS 18+
