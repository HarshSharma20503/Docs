## Installation
```bash
npm install puppeteer
```
## Launch a browser

1. Launching puppeteer browser
```javascript
     const browser = await puppeteer.launch({
       headless: false,
       defaultViewport: null,
       args: ["--start-maximized"],
     });
```

2. Launching your own browser
```javascript
    const browser = await puppeteer.launch({
      executablePath:
        "/Applications/Google Chrome.app/Contents/MacOS/Google Chrome",
      headless: false,
      defaultViewport: null, // This allows the browser to open in full screen
      args: ["--start-fullscreen"], // Full screen argument
    });
```

## Opening new page

```javascript
const page = await browser.newPage();
await page.goto("https://www.geeksforgeeks.org/problem-of-the-day");
```
## Click On link and get new page

```javascript
// Listen for new target (tab) creation
    const [newPage] = await Promise.all([
      new Promise((resolve) =>
        browser.once("targetcreated", (target) => resolve(target.page()))
      ),
      page.click(".ui.button.problemOfTheDay_POTDCntBtn__SSQfX"),
    ]);
```

## Type on a input box
```javascript
await newPage.waitForSelector(".next_input");
const inputbox = await newPage.$$(".next_input");
await inputbox[0].type("harshsharma20503");
await inputbox[1].type("H@rsh123sharma");
await newPage.click(".notSocialLoginBtnText");
```

## Get all links

```javascript
  const hrefs = await page.evaluate(() => {
    // Select all <a> tags on the page
    const anchorTags = document.querySelectorAll("a");

    // Extract and return the href attributes from all <a> tags
    return Array.from(anchorTags)
      .map((a) => a.href)
      .filter((href) => href)
      .filter((href) => href.includes("envType=daily-question"));
  });
```

## Snippet

```javascript
import puppeteer from "puppeteer";

(async () => {
  try {
    const page = await browser.newPage();
    await page.goto("https://www.geeksforgeeks.org/problem-of-the-day");

    // Listen for new target (tab) creation
    const [newPage] = await Promise.all([
      new Promise((resolve) =>
        browser.once("targetcreated", (target) => resolve(target.page()))
      ),
      page.click(".ui.button.problemOfTheDay_POTDCntBtn__SSQfX"),
    ]);
    await newPage.waitForNavigation();
    //   console.log("Page content is ready: ", await newPage.content());
    await newPage.waitForSelector(
      ".signinButton.gfg_loginModalBtn.login-modal-btn"
    );
    await newPage.click(".signinButton.gfg_loginModalBtn.login-modal-btn");

    await newPage.waitForSelector(".next_input");

    const inputbox = await newPage.$$(".next_input");

    await inputbox[0].type("harshsharma20503");
    await inputbox[1].type("H@rsh123sharma");
    await newPage.click(".notSocialLoginBtnText");

    //   await newPage.waitForSelector(".mb15.next_input ");
    //   await newPage.type(".mb15.next_input ", "harshsharma20503@gmail.com");
    //   await newPage.type(".next_input", "H@rsh123sharma");
    //   await newPage.click(".notSocialLoginBtnText");

    // Perform further actions if needed
    await browser.close();
  } catch (err) {
    console.log("Error: ", err);
  }
})();

```
