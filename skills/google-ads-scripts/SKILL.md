---
name: google-ads-scripts
description: "When the user wants help with Google Ads Scripts, automating repetitive tasks in Google Ads via JavaScript, writing or debugging scripts for bulk changes, automated reporting, bid adjustments, budget monitoring, ad scheduling, or any Google Ads automation beyond the UI. Triggers on 'Google Ads script', 'ads script', 'automate Google Ads', 'bulk changes script', 'script for Google Ads', 'Google Ads automation', 'auto pause', 'script alert', 'script report', or 'scheduled script'. For API-level automation see the Google Ads API."
metadata:
  version: 1.0.0
---

# Google Ads — Scripts & Automation

You are a Google Ads Scripts specialist. Your goal is to write, debug, and deploy JavaScript scripts that automate repetitive tasks, protect accounts from waste, surface insights, and execute bulk changes at scale — without needing the Google Ads API.

## Before Starting

**Check for product marketing context first:**
If `.agents/product-marketing-context.md` exists, read it before asking questions.

Gather this context:

### 1. What to Automate
- What task needs automating? (Reporting, bid changes, pausing, alerts, budget management?)
- How often should it run? (Hourly, daily, weekly?)
- Should it make changes or only alert?

### 2. Account Structure
- Single account or MCC (manager account) script?
- How many campaigns, ad groups, keywords?
- Any scripts currently running?

### 3. Access & Data
- Access to Google Ads Scripts editor?
- Google Sheets for reporting output?
- Email address for alerts?

---

## Google Ads Scripts Fundamentals

### What Scripts Can Do
- Read account data (campaigns, ad groups, keywords, ads, performance)
- Make changes (pause/enable, bid changes, budget updates, ad copy edits)
- Write to Google Sheets
- Send email alerts
- Interact with external APIs (Slack, etc.)
- Run on a schedule (hourly, daily, weekly)

### What Scripts Cannot Do
- Create new campaigns from scratch (limited)
- Access data older than 36 months
- Run faster than ~1 million operations per execution

### Script Editor
- Google Ads → Tools & Settings → Bulk Actions → Scripts
- Test with Preview mode first — **never run untested scripts in live mode**
- Check execution logs after each run

### Execution Limits
| Limit | Value |
|-------|-------|
| Runtime per execution | 30 minutes |
| Fetch calls per execution | 20,000 |
| Spreadsheet reads/writes | 250 per execution |
| Email sends | 100 per execution |

---

## Script Architecture

### Basic Script Template
```javascript
function main() {
  // 1. Define parameters
  var config = {
    ALERT_EMAIL: 'you@company.com',
    SPREADSHEET_URL: 'https://docs.google.com/...',
    DATE_RANGE: 'LAST_7_DAYS'
  };

  // 2. Get data
  var campaignIterator = AdsApp.campaigns()
    .withCondition('Status = ENABLED')
    .forDateRange(config.DATE_RANGE)
    .orderBy('Impressions DESC')
    .get();

  // 3. Process data
  while (campaignIterator.hasNext()) {
    var campaign = campaignIterator.next();
    var stats = campaign.getStatsFor(config.DATE_RANGE);

    // 4. Take action or log
    Logger.log(campaign.getName() + ': ' + stats.getConversions());
  }
}
```

### Key Objects

| Object | Access Via | Contains |
|--------|-----------|---------|
| Campaigns | `AdsApp.campaigns()` | Campaign settings, stats |
| Ad Groups | `AdsApp.adGroups()` | Ad group settings, stats |
| Keywords | `AdsApp.keywords()` | Keyword text, match type, bid, stats |
| Ads | `AdsApp.ads()` | Ad copy, stats, approval status |
| Budget | `campaign.getBudget()` | Daily budget, delivery method |
| Bidding | `campaign.getBiddingStrategyType()` | Bid strategy name |

### Selectors & Filters
```javascript
// Filter campaigns by conditions
AdsApp.campaigns()
  .withCondition('Status = ENABLED')
  .withCondition('CampaignName CONTAINS "Brand"')
  .withCondition('Impressions > 100')
  .forDateRange('LAST_30_DAYS')
  .orderBy('Cost DESC')
  .withLimit(50)
  .get();
```

**Common conditions:**
- `Status = ENABLED / PAUSED / REMOVED`
- `CampaignName CONTAINS "string"`
- `Cost > 100` (in account currency)
- `Clicks > 0`
- `Conversions = 0`
- `Ctr < 0.01` (1%)
- `QualityScore < 5`

---

## Essential Scripts Library

### 1. Budget Pacing Monitor
Alerts if campaigns are on pace to over/underspend monthly budget.

```javascript
function main() {
  var ALERT_EMAIL = 'you@company.com';
  var THRESHOLD_PCT = 0.20; // Alert if 20%+ over/under pace

  var today = new Date();
  var daysInMonth = new Date(today.getFullYear(), today.getMonth()+1, 0).getDate();
  var dayOfMonth = today.getDate();
  var monthProgress = dayOfMonth / daysInMonth;

  var campaignIterator = AdsApp.campaigns()
    .withCondition('Status = ENABLED')
    .forDateRange('THIS_MONTH')
    .get();

  var alerts = [];

  while (campaignIterator.hasNext()) {
    var campaign = campaignIterator.next();
    var budget = campaign.getBudget().getAmount() * daysInMonth;
    var spent = campaign.getStatsFor('THIS_MONTH').getCost();
    var paceRatio = spent / (budget * monthProgress);

    if (paceRatio > 1 + THRESHOLD_PCT) {
      alerts.push(campaign.getName() + ' is OVERPACING at ' + (paceRatio*100).toFixed(0) + '%');
    } else if (paceRatio < 1 - THRESHOLD_PCT) {
      alerts.push(campaign.getName() + ' is UNDERPACING at ' + (paceRatio*100).toFixed(0) + '%');
    }
  }

  if (alerts.length > 0) {
    MailApp.sendEmail(ALERT_EMAIL, 'Budget Pacing Alert', alerts.join('\n'));
  }
}
```

### 2. Zero Conversion Keyword Pauser
Pauses keywords that have spent more than N× target CPA with 0 conversions.

```javascript
function main() {
  var TARGET_CPA = 50; // Your target CPA
  var MULTIPLIER = 2;  // Pause at 2× target CPA with 0 conversions
  var PAUSE_THRESHOLD = TARGET_CPA * MULTIPLIER;
  var SPREADSHEET_URL = 'https://docs.google.com/...';

  var sheet = SpreadsheetApp.openByUrl(SPREADSHEET_URL)
    .getActiveSheet();
  sheet.appendRow(['Date', 'Campaign', 'Ad Group', 'Keyword', 'Cost', 'Action']);

  var keywordIterator = AdsApp.keywords()
    .withCondition('Status = ENABLED')
    .withCondition('Conversions = 0')
    .withCondition('Cost > ' + PAUSE_THRESHOLD)
    .forDateRange('LAST_30_DAYS')
    .get();

  while (keywordIterator.hasNext()) {
    var keyword = keywordIterator.next();
    var stats = keyword.getStatsFor('LAST_30_DAYS');
    var adGroup = keyword.getAdGroup();
    var campaign = adGroup.getCampaign();

    sheet.appendRow([
      new Date(),
      campaign.getName(),
      adGroup.getName(),
      keyword.getText(),
      stats.getCost(),
      'Paused'
    ]);

    keyword.pause();
  }
}
```

### 3. Performance Report to Google Sheets
Daily campaign performance summary written to a spreadsheet.

```javascript
function main() {
  var SPREADSHEET_URL = 'https://docs.google.com/...';
  var DATE_RANGE = 'YESTERDAY';

  var spreadsheet = SpreadsheetApp.openByUrl(SPREADSHEET_URL);
  var sheet = spreadsheet.getSheetByName('Daily Report') ||
    spreadsheet.insertSheet('Daily Report');

  // Add header if sheet is empty
  if (sheet.getLastRow() === 0) {
    sheet.appendRow([
      'Date', 'Campaign', 'Impressions', 'Clicks', 'CTR',
      'Cost', 'Conversions', 'CPA', 'Conv Rate'
    ]);
  }

  var campaignIterator = AdsApp.campaigns()
    .withCondition('Status = ENABLED')
    .forDateRange(DATE_RANGE)
    .orderBy('Cost DESC')
    .get();

  while (campaignIterator.hasNext()) {
    var campaign = campaignIterator.next();
    var stats = campaign.getStatsFor(DATE_RANGE);
    var conv = stats.getConversions();
    var cost = stats.getCost();

    sheet.appendRow([
      new Date(),
      campaign.getName(),
      stats.getImpressions(),
      stats.getClicks(),
      (stats.getCtr() * 100).toFixed(2) + '%',
      cost.toFixed(2),
      conv,
      conv > 0 ? (cost / conv).toFixed(2) : 'N/A',
      (stats.getConversionRate() * 100).toFixed(2) + '%'
    ]);
  }
}
```

### 4. Ad Disapproval Monitor
Alerts when ads get disapproved.

```javascript
function main() {
  var ALERT_EMAIL = 'you@company.com';
  var disapprovedAds = [];

  var adIterator = AdsApp.ads()
    .withCondition('Status = ENABLED')
    .withCondition('PolicySummary.ReviewStatus != REVIEWED')
    .get();

  while (adIterator.hasNext()) {
    var ad = adIterator.next();
    var policySummary = ad.getPolicySummary();

    if (policySummary.isDisapproved()) {
      var adGroup = ad.getAdGroup();
      var campaign = adGroup.getCampaign();
      disapprovedAds.push([
        campaign.getName(),
        adGroup.getName(),
        ad.getHeadlinePart1()
      ].join(' | '));
    }
  }

  if (disapprovedAds.length > 0) {
    MailApp.sendEmail(
      ALERT_EMAIL,
      'Google Ads: Disapproved Ads Alert',
      'The following ads are disapproved:\n\n' + disapprovedAds.join('\n')
    );
  }
}
```

### 5. Quality Score Tracker
Logs Quality Score by keyword over time to a spreadsheet.

```javascript
function main() {
  var SPREADSHEET_URL = 'https://docs.google.com/...';
  var MIN_IMPRESSIONS = 100; // Only track keywords with enough data

  var sheet = SpreadsheetApp.openByUrl(SPREADSHEET_URL).getActiveSheet();
  var date = Utilities.formatDate(new Date(), 'America/New_York', 'yyyy-MM-dd');

  var keywordIterator = AdsApp.keywords()
    .withCondition('Status = ENABLED')
    .withCondition('Impressions > ' + MIN_IMPRESSIONS)
    .forDateRange('LAST_30_DAYS')
    .orderBy('Impressions DESC')
    .get();

  while (keywordIterator.hasNext()) {
    var keyword = keywordIterator.next();
    var qs = keyword.getQualityScore();

    if (qs !== null) {
      sheet.appendRow([
        date,
        keyword.getAdGroup().getCampaign().getName(),
        keyword.getAdGroup().getName(),
        keyword.getText(),
        keyword.getMatchType(),
        qs,
        keyword.getQualityScore() < 5 ? 'LOW - Review' : ''
      ]);
    }
  }
}
```

### 6. Automated Bid Adjustment by Hour
Adjust campaign bids based on time of day performance.

```javascript
function main() {
  // Bid multipliers by hour (0-23) — 1.0 = no change
  var HOURLY_MULTIPLIERS = [
    0.5,  // 12am
    0.4,  // 1am
    0.4,  // 2am
    0.4,  // 3am
    0.5,  // 4am
    0.7,  // 5am
    0.9,  // 6am
    1.0,  // 7am
    1.1,  // 8am
    1.2,  // 9am
    1.2,  // 10am
    1.1,  // 11am
    1.0,  // 12pm
    1.0,  // 1pm
    1.1,  // 2pm
    1.1,  // 3pm
    1.0,  // 4pm
    0.9,  // 5pm
    0.8,  // 6pm
    0.7,  // 7pm
    0.6,  // 8pm
    0.5,  // 9pm
    0.5,  // 10pm
    0.5   // 11pm
  ];

  var currentHour = new Date().getHours();
  var multiplier = HOURLY_MULTIPLIERS[currentHour];

  var campaignIterator = AdsApp.campaigns()
    .withCondition('Status = ENABLED')
    .withCondition('CampaignName CONTAINS "[Hourly Bid]"') // Tag campaigns to include
    .get();

  while (campaignIterator.hasNext()) {
    var campaign = campaignIterator.next();
    // Implementation depends on bid strategy
    // For manual CPC: adjust via ad schedule
    Logger.log('Hour ' + currentHour + ': multiplier ' + multiplier + ' for ' + campaign.getName());
  }
}
```

### 7. Search Term Mining — Auto-Add Negatives
Add search terms matching patterns as negatives automatically.

```javascript
function main() {
  var NEGATIVE_PATTERNS = [
    /\bfree\b/i,
    /\bjobs?\b/i,
    /\bcareers?\b/i,
    /\bwikipedia\b/i,
    /\bhow to\b/i,
    /\btutorial\b/i
  ];

  var sharedNegativeList = 'Auto-Negatives'; // Name of your shared negative list

  // Get or create shared negative keyword list
  var negListIterator = AdsApp.negativeKeywordLists()
    .withCondition('Name = "' + sharedNegativeList + '"')
    .get();

  var negList = negListIterator.hasNext()
    ? negListIterator.next()
    : AdsApp.newNegativeKeywordListBuilder().withName(sharedNegativeList).build().getResult();

  var report = AdsApp.report(
    'SELECT Query, Impressions, Clicks, Cost, Conversions ' +
    'FROM SEARCH_QUERY_PERFORMANCE_REPORT ' +
    'WHERE Impressions > 10 AND Conversions = 0 ' +
    'DURING LAST_7_DAYS'
  );

  var rows = report.rows();
  var addedNegatives = [];

  while (rows.hasNext()) {
    var row = rows.next();
    var query = row['Query'];

    for (var i = 0; i < NEGATIVE_PATTERNS.length; i++) {
      if (NEGATIVE_PATTERNS[i].test(query)) {
        negList.addNegativeKeyword('[' + query + ']'); // Add as exact negative
        addedNegatives.push(query);
        break;
      }
    }
  }

  if (addedNegatives.length > 0) {
    Logger.log('Added ' + addedNegatives.length + ' negatives: ' + addedNegatives.join(', '));
  }
}
```

---

## MCC (Manager Account) Scripts

Run across all child accounts simultaneously.

```javascript
function main() {
  var accountIterator = MccApp.accounts()
    .withCondition('LabelNames CONTAINS "Managed"') // Filter specific accounts
    .get();

  MccApp.executeInParallel(
    accountIterator,
    'processAccount', // Function to run in each account
    JSON.stringify({ threshold: 100 }) // Pass params as JSON string
  );
}

function processAccount(params) {
  var config = JSON.parse(params);
  // Same logic as single-account script
  // AdsApp now refers to current child account
}
```

---

## Scheduling Scripts

In Google Ads Scripts editor → Select frequency:
| Frequency | Use For |
|-----------|---------|
| Hourly | Bid adjustments, budget monitoring |
| Daily | Performance reports, keyword pausing |
| Weekly | QS tracking, audit reports |
| Monthly | Budget planning, comprehensive audits |

**Best practice:** Run alerts hourly or daily. Run scripts that make changes only after manual review of output first.

---

## Debugging & Best Practices

### Preview Mode
**Always run new scripts in Preview mode first.**
- Shows what the script would do without making changes
- Check Logger output for errors or unexpected behavior

### Error Handling
```javascript
function main() {
  try {
    // Your script logic
  } catch (e) {
    Logger.log('Error: ' + e.message);
    MailApp.sendEmail('you@company.com', 'Script Error', e.message + '\n\n' + e.stack);
  }
}
```

### Rate Limiting
```javascript
// Add delays for large operations
Utilities.sleep(500); // 500ms delay between operations
```

### Logging
```javascript
Logger.log('Campaign: ' + campaign.getName() + ' | Cost: ' + stats.getCost());
// View logs: Scripts editor → Logs tab
```

---

## Optimization Checklist

### Setup
- [ ] All scripts tested in Preview mode before enabling
- [ ] Spreadsheet URLs updated in config
- [ ] Alert email addresses set
- [ ] Schedules configured appropriately
- [ ] Scripts documented with comments

### Ongoing
- [ ] Review script logs weekly for errors
- [ ] Verify scripts are running on schedule (check "Last run" in Scripts list)
- [ ] Update thresholds when campaign performance baselines change
- [ ] Archive and version-control scripts in a Google Doc or GitHub

---

## Common Mistakes

- Running untested scripts in live mode (use Preview first)
- No error handling (silent failures)
- Hardcoded account-specific values that break in MCC context
- Not checking logs after script runs
- Scripts that make irreversible changes without logging what was changed

---

## Related Skills

- **google-ads-bidding**: Understanding bid strategies scripts can automate
- **google-ads-keywords**: Search term and keyword management scripts automate
- **google-ads-conversion-tracking**: Conversion data scripts report on
- **analytics-tracking**: GA4 and Sheets integration for reporting scripts
