Home Area Checker for UK Postcodes

A simple tool that lets anyone enter a UK postcode or full address and get a quick profile of the local area. It pulls together trusted public datasets and shows them in one readable report.

This repository contains the public website only. The backend service and data files live in a separate private repository.

If you are not a programmer, do not worry. You can follow this like a recipe.

⸻

What this does

When you enter a postcode or address, the site produces:
	•	Location details (postcode, latitude, longitude, LSOA, borough, region)
	•	Index of Multiple Deprivation (IoD 2025 scores)
	•	Crime summary (latest month)
	•	Flood warnings
	•	Nearby schools (GIAS plus Ofsted)
	•	Nearby grocery shops (OpenStreetMap)
	•	Nearby rail or transport stops (OpenStreetMap)
	•	Air quality from nearest LondonAir site
	•	Census 2021 demographics by LSOA
	•	Notes and warnings from the backend if anything is missing or timed out

While it runs you will see rotating progress messages, because even computers on free plans sometimes need a moment and a word of encouragement.

⸻

Live links
	•	Public website (GitHub Pages)
https://ph0429.github.io/UK-london-postcode-checker/
	•	Backend API (Render)
https://home-check-backend.onrender.com
You can view the backend documentation here:
https://home-check-backend.onrender.com/docs

⸻

Repository structure (public)

You should see files like:
	•	index.html
The full website. It contains:
	•	The postcode input
	•	All layout and styling
	•	The progress messages and empty state messages
	•	The logic that calls the backend API and displays results

No other files are required for the public site.

⸻

How the system is split

To keep costs at zero and avoid exposing data files publicly, the project is split into two parts.

Public repo (this one)
	•	Hosts the website using GitHub Pages for free.
	•	Only needs index.html.
	•	Calls the backend API over the internet.

Private backend repo
	•	Hosts the Python backend on Render.
	•	Contains:
	•	api.py which defines the web service and the /analyse endpoint.
	•	home_check.py which does the actual analysis.
	•	a /data folder with CSV and Excel files that are required for IMD, schools and census.
	•	This repo is private because it contains large data files and backend logic.

⸻

How to recreate this project from scratch

Step 1. Create the public GitHub repository
	1.	Go to GitHub and log in.
	2.	Click New repository.
	3.	Name it something like UK-london-postcode-checker.
	4.	Set it to Public.
	5.	Click Create repository.

Step 2. Add the website file
	1.	Open VS Code.
	2.	Create a new folder on your computer for the project.
	3.	Inside that folder, create a file called index.html.
	4.	Copy in your website content.
You do not need to write new code if you are recreating this project.
The key thing is that index.html points to your backend URL.

Your index.html should reference the backend like this in plain text:
	•	Backend base URL: https://home-check-backend.onrender.com
	•	Endpoint used by the site: /analyse

Step 3. Push the public repo to GitHub
	1.	In VS Code, open the left hand Source Control panel.
	2.	Click Initialise Repository if prompted.
	3.	Stage the index.html file.
	4.	Write a commit message, for example: “Add Home Area Checker site”.
	5.	Click Commit.
	6.	Click Publish Branch or Push.
	7.	Choose the public GitHub repo you created in Step 1.

Step 4. Turn on GitHub Pages
	1.	Go to your public repo on GitHub in a browser.
	2.	Click Settings.
	3.	In the left menu, click Pages.
	4.	Under Source, choose Deploy from a branch.
	5.	Under Branch, pick main and the root folder.
	6.	Click Save.
	7.	GitHub will show you the live URL after a minute or two.

Step 5. Create the private backend repository
	1.	Go to GitHub and click New repository again.
	2.	Name it something like home_check_backend.
	3.	Set it to Private.
	4.	Click Create repository.

Step 6. Add backend scripts
	1.	Open a fresh folder in VS Code for the backend.
	2.	Add these files:
	•	api.py
	•	home_check.py
	•	requirements.txt
	•	.gitignore
	3.	The backend should read its local data files using paths like:
“project folder / data / filename”.
This matters on Render.

Step 7. Add the data folder
	1.	In the backend folder, create /data.
	2.	Put your required files inside. Examples:
	•	IoD 2025 Excel file
	•	GIAS schools CSV
	•	Ofsted schools CSV
	•	Census 2021 CSVs
	•	LondonAir sites JSON

These files must be committed in the private repo so Render can read them.

Step 8. Push private backend repo to GitHub

Same as Step 3, but to your private repo.

Step 9. Deploy backend to Render for free
	1.	Go to https://render.com and log in.
	2.	Click New and choose Web Service.
	3.	Connect your GitHub account when asked.
	4.	Select your private backend repository.
	5.	Render settings:
	•	Environment: Python
	•	Build command: Render will detect from requirements.txt.
	•	Start command should be:
uvicorn api:app --host 0.0.0.0 --port 8000
	6.	Click Deploy.

Render free services can “sleep”. The first request after a while may take longer. That is normal and not a sign of doom.

Step 10. Confirm backend works
	1.	After deploy, open your Render service URL.
	2.	Add /docs to the end.
	3.	If you see the FastAPI documentation page, you are live.

Step 11. Connect public site to backend
	1.	In your public index.html, find the backend base URL line.
	2.	Set it to your Render URL.
	3.	Commit and push again to the public repo.

⸻

Troubleshooting guide

The site says it is processing but nothing appears
	•	Render free tier may be waking up. Wait a bit and try again.
	•	Check Render logs. If you see data file missing warnings, your backend data folder is not in the right place or not committed.

A section shows “no data” with a funny message

That is expected sometimes. There are three possible reasons, and the site tells you which one:
	1.	API worked but nothing was found
Example: there may genuinely be no flood warnings nearby.
	2.	API timed out or could not be reached
Example: OpenStreetMap Overpass occasionally times out.
	3.	A backend data file is missing
Example: census files not committed or in the wrong folder.

Fix the specific problem, redeploy backend, then rerun.

⸻

Data sources

This project uses public sources including:
	•	Postcodes.io for geocoding
	•	UK Police data for crime
	•	Environment Agency for flood warnings
	•	OpenStreetMap Overpass for shops and transport
	•	LondonAir for air quality in London
	•	Census 2021 by LSOA
	•	Index of Multiple Deprivation 2025

Results are indicative, not legal advice or a substitute for visiting the area yourself. The tool is a helpful scout, not a clairvoyant.

⸻

Notes on costs
	•	GitHub Pages is free for public repositories.
	•	Render has a free tier for the backend.
	•	The backend may sleep when idle, which causes slower first runs.
	•	No custom domain is needed to run this setup for free.

⸻

Acknowledgements

Built by Patrick as a practical postcode based “should I move here” assistant. If a section returns nothing, either the area is perfect or the internet is being dramatic. Either way, try again.
