ReferralForm – LWR Experience Cloud Site
This document explains how to create an LWR-based Experience Cloud site and add a Lightning Web Component (LWC) to make a public referral submission form.
________________________________________
Overview
•	Site Type: Experience Cloud (LWR)
•	Purpose: Public referral submission form
•	Component: Custom LWC (referralForm)
•	Access: Public (Guest Users)
________________________________________
Prerequisites
•	Salesforce org with Experience Cloud enabled
•	LWC component already created
•	Apex classes (optional, if backend logic is required)

⚠️ Important:
Your LWC must not include lightningCommunity__Page in the meta.xml.
Otherwise, it will not appear in the LWR Experience Builder.
________________________________________
STEP 1: Enable Digital Experiences (One-Time Setup)
1.	Go to Setup
2.	Search for Digital Experiences
3.	Click Settings
4.	Enable Digital Experiences
5.	Set a Domain Name (example: mycompany)
6.	Click Save
⚠️ Domain setup is mandatory before creating any Experience Cloud site.
________________________________________
STEP 2: Create an LWR Experience Cloud Site
1.	Go to Setup
2.	Search for All Sites
3.	Click New
4.	Select the template:
o	✅ Build Your Own (LWR)
5.	Click Get Started
________________________________________
STEP 3: Configure Site Details
Fill in the following:
Field	Value
Site Name	ReferralForm
URL	/referralform
Description	Public referral submission form
Click Create.
⏳ Salesforce will provision the site (takes approximately 1–2 minutes).
________________________________________
STEP 4: Open Experience Builder
1.	Go to Setup → All Sites
2.	Find ReferralForm
3.	Click Builder
________________________________________
STEP 5: Make the Site Public (Guest Access)
1.	In Experience Builder, click ⚙ Settings
2.	Open the General tab
3.	Under Access, enable:
o	✅ Allow access to unauthenticated users
4.	Click Save
________________________________________
STEP 6: Add the LWC to the Site Page
1.	In Experience Builder, open the Home Page
2.	Click Components (🧩 icon)
3.	Scroll to Custom Components
4.	Drag referralForm onto the page
✔ The component should now render on the page.
________________________________________
STEP 7: Grant Guest User Permissions (Mandatory)
1.	In Experience Builder, go to:
o	⚙ Settings → Advanced
2.	Click Guest User Profile
3.	Grant the following permissions as needed:
o	Access to the LWC
o	Access to Apex classes (if used)
o	Object permissions (Create / Read, if creating records)
⚠️ Without these permissions, the public form will not work.
________________________________________
STEP 8: Publish the Site
1.	In Experience Builder (top-right corner)
2.	Click Publish
3.	Confirm the action
________________________________________
STEP 9: Test the Public URL
Open the site in an incognito/private browser window:
https://yourdomain.my.site.com/referralform
✔ The referral form should load without requiring login.

