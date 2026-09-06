# Stirling County RFC – Micro Rugby Contacts

A single-page contact log for Stirling County RFC's Micro Rugby section:
child's name, parent/guardian name, email, phone, whether new-member
details have been sent, whether they've signed up, kit size, kit order
date, and a free-text comments field. Data is stored in Firestore and only
accessible to signed-in coaches/managers.

## 1. Create a Firebase project

1. Go to [console.firebase.google.com](https://console.firebase.google.com) and create a new project.
2. In **Build → Firestore Database**, click **Create database** and start in **production mode**.
3. In **Build → Authentication → Sign-in method**, enable **Email/Password**.
4. In **Authentication → Users**, manually add an account for each coach or
   manager who needs access (email + password). There is no public sign-up
   screen in this app on purpose — access is admin-granted only, since the
   data includes children's details.
5. In **Project settings → General**, scroll to "Your apps", click the
   web icon (`</>`) to register a web app, and copy the `firebaseConfig`
   object it gives you.

## 2. Wire up the app

Open `index.html`, find the `firebaseConfig` object near the bottom of the
file (inside the `<script type="module">` block), and replace the
placeholder values with the config you copied above.

## 3. Publish the security rules

In **Firestore Database → Rules**, paste the contents of `firestore.rules`
from this repo and click **Publish**. This restricts all reads/writes to
signed-in users only — nobody can read or edit the list without a login you
created for them.

## 4. Deploy

Pushing to `main` triggers `.github/workflows/deploy-pages.yml`, which
publishes the repo to GitHub Pages automatically. Enable Pages once under
your repo's **Settings → Pages → Source → GitHub Actions** if it isn't
already active.

## Notes

- This app stores personal details about children and their parents/guardians.
  Only give logins to people who need access, and remove accounts (in
  Authentication → Users) when someone leaves a coaching/admin role.
- The Add/Edit Player form is split into sections: Player Details, Contact
  Details, Membership, Kit Order, and Comments. "New Member Details Sent",
  "Signed Up", "Social Media Permission", "Added To WhatsApp Group", and
  "Kit Ordered" are all yes/no toggles (pale green = yes, pale red = no).
- Tap the **?** button in the header any time for a how-to popup (it also
  shows automatically the first time someone signs in, tracked via
  `localStorage` in that browser).
- The **Export** button downloads every player's details as a `.xlsx`
  spreadsheet (via the [ExcelJS](https://github.com/exceljs/exceljs) library,
  loaded from cdnjs, with a scarlet header row), regardless of the current
  search/filter.
- The "Kit Not Ordered" filter matches the "Kit Ordered" toggle being off,
  not whether a kit size or order date has been entered.
- Colours (black/red/gold) and the crest are a stylised approximation of
  Stirling County RFC's branding based on screenshots of
  stirlingcounty-rfc.co.uk — this environment couldn't reach that domain
  directly to pull exact hex values or the official crest artwork. Swap in
  the club's real crest image and exact colours in `index.html` if you'd
  like a pixel-perfect match.
