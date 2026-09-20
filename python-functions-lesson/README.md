# Python Functions: interactive lesson with live class voting

This folder has everything needed to host the Year 9 Python Functions lesson on GitHub Pages, with students voting live from their own devices.

| File | What it is |
|---|---|
| `index.html` | The lesson you present from the front |
| `vote.html` | The page students open on their phones or laptops |
| `firebase-config.js` | Your Firebase settings, which you paste in during step 4 |
| `database.rules.json` | Security rules to paste into Firebase in step 3 |

The lesson works without step 2 onwards. Until Firebase is set up, you can count hands by tapping the options, and the **Live voting** button explains what's missing.

---

## Setup (about 10 minutes, once)

### 1. Put the files on GitHub Pages
1. Create a new repository on GitHub, for example `python-functions`.
2. Upload all the files in this folder: **Add file → Upload files**.
3. Go to **Settings → Pages**. Under *Build and deployment*, choose **Deploy from a branch**, then branch **main** and folder **/ (root)**, then **Save**.
4. After a minute your lesson is live at `https://YOUR-USERNAME.github.io/python-functions/`.

### 2. Create a free Firebase project
1. Go to <https://console.firebase.google.com> and sign in with a Google account.
2. Click **Create a project**. Give it a name (for example *cs-class-votes*). You can turn off Google Analytics.
3. In the left menu, open **Build → Authentication → Get started**.
4. On the **Sign-in method** tab, choose **Anonymous**, switch it **on** and click **Save**.
   *This gives each device a random ID so it can only vote once per question. No names or emails are collected.*
5. On the **Settings** tab, open **Authorized domains → Add domain** and enter `YOUR-USERNAME.github.io`.

### 3. Create the database and add the security rules
1. In the left menu, open **Build → Realtime Database → Create Database**.
2. Choose the location **Singapore (asia-southeast1)**, which is closest to Korea, then **Start in locked mode**, then **Enable**.
3. Open the **Rules** tab. Delete what's there, paste in everything from `database.rules.json`, and click **Publish**.

The rules mean:
- Only the teacher who started a session can open or close questions, reveal answers or end the session.
- Students can only vote on the question that is currently open, and only once per device. They can change their answer until voting closes.
- Nobody can read anything without joining through the app.

### 4. Connect the lesson to Firebase
1. Click the ⚙️ gear icon → **Project settings**. Under *Your apps*, click the **</>** (Web) icon.
2. Give the app a nickname. Leave *Firebase Hosting* unticked, then click **Register app**.
3. Firebase shows a block of code with `const firebaseConfig = { … }`. Copy the values between the `{ }`.
4. On GitHub, open `firebase-config.js`, click the ✏️ pencil to edit it, replace the `PASTE…` values with your own and **Commit changes**.
   - Make sure there is a `databaseURL` line. If it's missing, copy the URL shown at the top of the **Realtime Database → Data** page, which looks like `https://….asia-southeast1.firebasedatabase.app`.

These settings are safe to publish. Firebase web settings are meant to be public, and the rules from step 3 protect the data.

### 5. Test it
1. Open your lesson link, then click **Live voting → Start a live session**.
2. On your phone, scan the QR code, or go to the link and type the 5-letter room code.
3. In the lesson, go to **Predict** and click **Open voting on devices**. Vote on your phone, and the bars fill in live.

---

## Using it in a lesson
- **Start:** click **Live voting → Start a live session** and put the room code and QR code on the board. In Present mode, the room code appears in the control bar at the bottom. Click it to show the QR code again.
- **Each vote question** (Predict and the four exit ticket questions) has **Open voting on devices** and **Close voting** buttons. Only one question is open at a time, and opening a new one closes the last.
- **Reveal answer** closes voting and shows each student whether they were right on their own device.
- **Students without a device:** tap the options to add hand counts. These are added to the device votes, and the small text shows the split, for example "12 + 3 hands".
- **Reset votes** clears both the hand counts and the device votes for that question.
- **End session** at the end of the lesson deletes the room and all its votes from Firebase. Students' screens say the session has ended.

## Good to know
- **Free plan limit:** the free Firebase plan allows 100 devices connected at the same time. That's plenty for one class, but if several teachers share one Firebase project at the same time, each department member may want their own project.
- **Tidying up:** if you close the tab without clicking **End session**, that room stays in the database. It's tiny, but you can delete old rooms in **Realtime Database → Data**.
- **School network:** the pages need `gstatic.com` (Firebase), `cdnjs.cloudflare.com` (QR code) and `cdn.jsdelivr.net` (the Python runner). If voting doesn't connect on school Wi-Fi, ask IT to allow these, plus `*.firebasedatabase.app`.
- **Privacy:** no names, emails or student data are collected. Each device gets a random anonymous ID that only lasts for that browser.

## Troubleshooting
| What you see | What to do |
|---|---|
| "Live voting isn't set up on this copy yet" | `firebase-config.js` still has the `PASTE` values, or is missing `databaseURL` (step 4). |
| "Couldn't start a session … admin-restricted-operation" | Anonymous sign-in isn't switched on (step 2.4). |
| "… PERMISSION_DENIED" | The rules weren't published (step 3.3). |
| Students see "Room not found" | Check the code. Codes never use the letters I, L or O, or the digits 0 or 1. |
| No QR code appears | The QR library was blocked. Students can type the room code instead. |
