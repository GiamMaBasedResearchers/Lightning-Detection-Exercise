Lightning Detection Exercise - pure training and code far from usability ;)

Obviously, there will be several fixes to be made, and the code isn't clean, and you will find entities that appear to be false positives but are not always false positives. I consider the exercise very interesting for various levels of understanding, knowledge and technicalities.

As always, you can use the plain HTML locally 👋👋👋

To preview GitHub HTML files, use these services by pasting the links:

https://raw.githack.com/ (Tested / working)

You can also use W3Schools Tryit Editor:

https://www.w3schools.com/html/tryit.asp?filename=tryhtml_default


1. Lightning Comparator Tool (file Image_Comparator_Tool.HTML)


<img width="1917" height="925" alt="image" src="https://github.com/user-attachments/assets/43e78158-246f-4afd-8b23-9c45608ef7ef" />


A client-side web tool designed to compare and correlate global lightning activity data between WWLLN (World Wide Lightning Location Network) and Blitzortung (via Limaps).

This tool runs entirely in the browser using HTML5 Canvas for image manipulation and Leaflet.js for visual overlay comparison.

Features
Automated Data Fetching: Downloads the latest global lightning maps (Real-time or 24-hour composite) from WWLLN and Limaps.
Local Image Processing: Uses the Canvas API to resize the WWLLN layer to match the Blitz reference dimensions and removes the black background (chroma key) for better transparency.
History Composite: A specific mode to fetch and stack 24 hours of Blitz frames into a single composite image using screen blending mode.
Visual Comparison: Integrates Leaflet.js with L.CRS.Simple to treat the images as a flat coordinate system, allowing users to toggle layers and adjust opacity for precise comparison.
CORS Handling: Includes a fallback mechanism to manually upload images if browser security policies block remote pixel manipulation ("Tainted Canvas" issue).


Usage
Open index.html in any modern web browser.
Select a mode (Realtime or Yesterday) and click Load Images.
Once loaded, click Process & Remove Background. This will align the WWLLN image to the Blitz dimensions and strip the black background.
Use the checkboxes and opacity sliders to visually compare the two datasets.


Technical Details
Map Engine: Leaflet.js configured with L.CRS.Simple for pixel-based mapping rather than geographical coordinates.
Pixel Manipulation: Background removal logic iterates through pixel arrays to turn near-black pixels transparent.
Security Note: Remote images loaded without CORS headers ("tainted canvases") cannot have their pixels read by JavaScript. If the automatic processing fails, please use the Upload buttons to load local copies of the maps.


Data Sources
WWLLN: wwlln.net
Blitzortung/Limaps: limaps.org



2. Universal Map Alignment Tool (file Map Projection - Alignment Panel_2.html)


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/42a7b489-7a8d-4342-8f92-7557fbb51a43" />

   
A client-side web utility designed for precise map projection alignment and image processing. Specifically engineered to overlay WWLLN lightning data onto Blitzortung background maps by correcting complex geometric distortions using non-linear transformations.

Features
Advanced Projection Correction (Tri-Axial Y-Stretch): Unlike simple scaling, this tool applies independent stretch factors for the North Pole, Equator, and South Pole. This solves the "bowing" or distortion effects caused by mismatched map projections (e.g., aligning Equirectangular data onto Mercator composites).
Geometric Controls:
Global X/Y Offset shifting for precise positioning.
Horizontal Width Scaling to match longitudinal spans.
Image Processing:
Chroma Keying: Adjustable threshold to remove black backgrounds from the overlay layer automatically.
Real-time Rendering: Uses HTML5 Canvas for instant visual feedback as you drag sliders.
Export: Generates a downloadable transparent PNG of the processed and aligned layer.


Usage
Upload Background: Select your reference map (e.g., a Blitzortung composite image).
Upload Overlay: Select the data layer you want to fit (e.g., a raw WWLLN global map).
Adjust Sliders:
Use Horizontal Offset and Horizontal Scale to align the width and longitude.
Use the North/Center/South Polar Stretch sliders to warp the vertical axis until the coastlines or grid lines match perfectly.
Adjust Chroma Key Alpha Threshold to clean up noise and black areas.
Export: Click "Export Aligned Transparent Layer" to save the corrected image.


Technical Details
The tool runs entirely in the browser using HTML5 Canvas. The alignment logic bypasses standard affine transformations by applying a dynamic exponent function to the vertical axis:

ytransformed​=sign(y)⋅∣y∣einterpolated​ Where einterpolated​ is calculated based on the pixel's distance from the equator, blending the user-defined North, Equatorial, and South exponents. This allows for high-precision warping of the lightning data to fit curved projection lines.


3.Lightning Georeferencing Tool (file GeoCode_Raster_Lightning_v3.1.html)


<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/3e113b35-1ba0-42c6-b677-71f9f89b66d5" />


A client-side web utility designed to transform 2D raster lightning maps (Limaps/Blitzortung and WWLLN) into interactive, georeferenced vector layers. This tool performs pixel-level scanning to extract lightning strikes and projects them onto a Leaflet.js map with precise lat/lon coordinates.

Features
Raster-to-Vector Conversion: Automatically scans static images (PNG/JPG) and converts colored pixels into interactive L.circleMarker objects.
Dual Data Source Support: Built-in configurations for Limaps (Blitzortung) and WWLLN networks, supporting both Realtime and Historical Composite modes.
Smart Auto-Cropping: Utilizes a "Safe Bounding Box" algorithm to detect the usable map area, automatically ignoring labels, scales, and borders to ensure accurate projection.
Calibration Controls: Includes manual X/Y offset sliders to fine-tune the georeferencing alignment if automatic detection is slightly off.
Time-Based Filtering: Extracts color-coded data to allow filtering by strike age (e.g., < 10m, 20m, 30m... 60m) directly on the map interface.
Advanced Noise Filtering: Includes specific algorithms for WWLLN data to remove grid lines and artifacts (Line Detection & Proximity Filter).
Performance Optimized: Uses asynchronous scanning with batch rendering to prevent UI freezing when processing high-resolution images.
History Composite: Automatically fetches and stacks 24 hours of historical Limaps frames into a single composite layer for "Yesterday" analysis.


Usage

Automatic Mode (Realtime/History)
Select the Data Source Mode (Realtime or Yesterday).
Adjust the Precision slider (Lower = Faster, Higher = More detailed points).
Click ⚡ Load & Process Images.
Wait for the tool to download, scan, and rasterize the data.

Manual Calibration
If the map alignment seems off (e.g., points appear in the ocean):
Use the X/Y Offset sliders in the Configuration section.
Observe the map shifting in real-time.
Re-process the images (or simply visually align if using vector outputs) to lock coordinates.

Manual Upload
Use the Manual Upload section to load local screenshots or downloaded maps.
The tool will attempt to auto-detect the bounds and georeference the uploaded image immediately.


Technical Details
Projection: Assumes Equirectangular projection for the source images. It maps pixel coordinates linearly to [-180, 180] Longitude and [90, -90] Latitude based on the detected bounding box.
Color Logic:
White: < 10 minutes (Most recent)
Yellow: ~20 minutes
Orange: ~30 minutes
Red/Orange-Red: ~40-50 minutes
Maroon: ~60 minutes (Oldest)
CORS Handling: Includes fallback logic (getCleanImage) to bypass common CORS/AdBlock issues by fetching blobs via fetch() API where possible.


Data Sources
WWLLN: wwlln.net
Limaps: limaps.org


4. Satellite Lightning Processor (GeoFixed) (file Geostationary_lightning_flsh_detected_V2.html)


<img width="1105" height="797" alt="image" src="https://github.com/user-attachments/assets/24fdaef0-483b-4597-ae27-1e4010c7df81" />


<img width="1464" height="633" alt="image" src="https://github.com/user-attachments/assets/b5c0e971-3004-4ae7-bf0f-a8988a0a3951" />


A client-side web utility designed to extract, rasterize, and georeference lightning flashes from geostationary satellite video feeds. This tool transforms transient video events (flashes) into a persistent, interactive map layer using Leaflet.js.

Features
Video-Based Detection: Analyzes MP4/WebM video streams frame-by-frame using pixel differencing to identify transient lightning flashes.
Color-Selective Analysis: Supports dual-channel detection logic:
Blue/Cyan: Detects lightning in GOES and Himawari ABI feeds.
Red: Detects lightning in MSG and FY-4B feeds.
Accurate Georeferencing: "GeoFixed" logic that calculates precise Leaflet bounds based on the specific sub-satellite longitude (e.g., GOES-18 West vs. GOES-19 East) and the detected Earth disk radius.
Rasterization: Converts detected video pixels into a persistent PNG overlay, allowing for historical accumulation of strikes over the video duration.
Auto-Calibration: Automatically detects the Earth's bounding box within the video frame to handle different crop margins and aspect ratios.
Preset & Local Support: Includes presets for major satellites (GOES-18/19, Himawari-8, MSG, FY-4B) and supports local video file uploads.
Supported Satellites

The tool comes pre-configured with accurate orbital longitudes for:

GOES-18 (West): ~137.2° W
GOES-19 (East): ~75.0° W
Himawari-8: ~140.7° E
MSG-0 (Prime): 0.0°
MSG-41 (IODC): 41.5° E
FY-4B: ~104.7° E

How It Works
Loading: Loads a video of the Earth (Full Disk usually).
Detection: Plays the video on a hidden canvas. For every frame, it compares the current frame to the previous one. If a pixel changes significantly (Diff) and matches the target color (Blue/Red), it is flagged as a lightning strike.
Mapping: The tool calculates the satellite's field of view (approx. +/- 81.3 degrees from the sub-satellite point). It maps the video coordinates (X/Y) to geographic coordinates (Lat/Lon) to ensure the overlay aligns perfectly with the underlying map.
Rendering: Draws all detected flashes onto a persistent canvas and overlays it on the map.

Usage
Select a Satellite Preset from the dropdown (or upload a local video).
Adjust Detection Sensitivity if the video is noisy.
Click ▶ Start Video Analysis.
Wait for the video to process. The tool will display a progress bar and count detected pixels.
Once finished, the map will automatically pan to the correct region and display the rasterized lightning layer.
Technical Note on Georeferencing
Unlike standard Equirectangular projections, geostationary feeds show a curved Earth. This tool approximates the geodetic projection by mapping the detected "Earth Disk" diameter to the satellite's actual ground coverage swath (~17.4 degrees view angle = ~162.6 km ground span at surface level). This ensures that pixels at the edge of the disk are mapped to the correct latitude/longitude, reducing distortion at the limbs.

Requirements
Modern web browser with Canvas and Video support.
No backend server required (works offline once loaded).


⚠️ Issue, Bug & misunderstandings of the code implementation:

- This code is highly imprecise and not very usable for effective analysis
- 99% of Proxy, CORS and similar redirects we try to eliminate for local use
- Many of the processes we hoped to automate must be entered by hand, so you need to download images and videos from the following links:

  a. Blitzortung
  www.blitzortung.org
  https://www.limaps.org/Maps/Maps/2026/
  https://www.limaps.org/Current/image_b_earth.png
  https://www.limaps.org/Maps/History/Backgrounds/image_b_earth.png

  b. WWLLN
  https://wwlln.net/
  https://wwlln.net/global_stroke_map.png
  https://wwlln.net/WWLLN.png
  https://wwlln.net/JBB/satellite/



⚠️ Disclaimer

Disclaimer: The code is for educational, training, analysis, & security purposes, & is intended for understanding & knowledge. The authors assume no liability for the misuse of this tool or the data it visualizes. Always ensure you have permission to access and visualize telemetry data.

Credits

This code provide from: "GiamMa-based researchers SDR R&D IoT" | @GiammaIoT2 License

This project is provided for research and educational purposes.
