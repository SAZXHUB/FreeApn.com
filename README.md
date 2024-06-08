
<html>
<head></head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Roboto:wght@400;700&display=swap');

        html, body {
            width: 100%;
            height: 100%;
            margin: 0;
            font-family: 'Roboto', sans-serif;
            color: white;
            text-align: center;
            background: radial-gradient(circle, #000428, #000428, #000428, #000428, #004e92);
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            overflow: hidden;
            position: relative;
        }

        body::before {
            content: '';
            position: absolute;
            top: -10px;
            left: -10px;
            width: calc(100% + 20px);
            height: calc(100% + 20px);
            border: 10px solid;
            border-image: linear-gradient(45deg, #ff0047, #2c34c7, #00ff85, #ffeb00, #ff0047) 1;
            pointer-events: none;
            animation: galaxy-border 10s ease infinite;
            box-sizing: border-box;
        }

        h1 {
            font-size: 3em;
            margin: 0;
            z-index: 1;
            text-shadow: 0 0 10px rgba(0, 0, 0, 0.7);
        }

        .button {
            margin-top: 20px;
            padding: 15px 30px;
            background: linear-gradient(45deg, #ff0047, #2c34c7, #00ff85, #ffeb00, #ff0047);
            background-size: 600% 600%;
            color: white;
            border: 2px solid transparent;
            border-radius: 30px;
            cursor: pointer;
            font-size: 1.2em;
            font-weight: bold;
            animation: galaxy-button 10s ease infinite;
            transition: all 0.3s ease;
            z-index: 1;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        .button:hover {
            border-color: white;
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.7);
            transform: scale(1.05);
        }

        @keyframes galaxy-border {
            0% { border-image-source: linear-gradient(45deg, #ff0047, #2c34c7, #00ff85, #ffeb00, #ff0047); }
            50% { border-image-source: linear-gradient(225deg, #ff0047, #2c34c7, #00ff85, #ffeb00, #ff0047); }
            100% { border-image-source: linear-gradient(45deg, #ff0047, #2c34c7, #00ff85, #ffeb00, #ff0047); }
        }

        @keyframes galaxy-button {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }

        @keyframes shooting-stars {
            0% { transform: translateY(-1500px) translateX(-1500px); }
            100% { transform: translateY(1500px) translateX(1500px); }
        }

        .shooting-star {
            position: absolute;
            top: -10px;
            left: 50%;
            width: 3px;
            height: 1500px;
            background: linear-gradient(to bottom, rgba(255, 255, 255, 0), rgba(255, 255, 255, 1));
            opacity: 0.7;
            transform: translateY(-1500px) translateX(-1500px);
            animation: shooting-stars 5s linear infinite;
            box-shadow: 0 0 15px 5px rgba(255, 255, 255, 0.7);
        }

        .shooting-star:nth-child(2) {
            left: 25%;
            animation-delay: 1s;
        }

        .shooting-star:nth-child(3) {
            left: 75%;
            animation-delay: 2s;
        }

        .shooting-star:nth-child(4) {
            left: 40%;
            animation-delay: 3s;
        }

        .shooting-star:nth-child(5) {
            left: 60%;
            animation-delay: 4s;
        }

        .shooting-star::after {
            content: '';
            position: absolute;
            bottom: 0;
            left: 50%;
            width: 10px;
            height: 10px;
            background: radial-gradient(circle, rgba(255, 255, 255, 1) 0%, rgba(255, 255, 255, 0) 70%);
            border-radius: 50%;
            transform: translateX(-50%);
            box-shadow: 0 0 20px 10px rgba(255, 255, 255, 0.7);
        }

        select {
            margin-top: 20px;
            padding: 10px;
            border-radius: 10px;
            border: 2px solid white;
            background: rgba(0, 0, 0, 0.5);
            color: white;
            font-size: 1em;
            font-family: 'Roboto', sans-serif;
            z-index: 1;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }

        select:focus {
            outline: none;
            box-shadow: 0 0 20px rgba(255, 255, 255, 0.7);
        }

        label {
            margin-top: 20px;
            font-size: 1.2em;
            font-weight: bold;
            text-shadow: 0 0 10px rgba(0, 0, 0, 0.7);
        }

        #apnResult {
            margin-top: 20px;
            z-index: 1;
            background: rgba(0, 0, 0, 0.5);
            padding: 20px;
            border-radius: 10px;
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
        }
    </style>
</head>
<body>
    <div class="shooting-star"></div>
    <div class="shooting-star"></div>
    <div class="shooting-star"></div>
    <div class="shooting-star"></div>
    <div class="shooting-star"></div>
    <h1>APN Finder for Asia</h1>
    <label for="country">Select a country:</label>
    <select id="country" onchange="updateProviders()">
        <option value="">--Select Country--</option>
        <option value="Thailand">Thailand</option>
        <option value="Vietnam">Vietnam</option>
        <option value="Malaysia">Malaysia</option>
        <option value="Singapore">Singapore</option>
        <option value="Indonesia">Indonesia</option>
        <option value="China">China</option>
        <option value="Japan">Japan</option>
    </select>
    <br>
    <label for="provider">Select a provider:</label>
    <select id="provider">
        <option value="">--Select Provider--</option>
    </select>
    <br>
    <button class="button" onclick="findAPN()">Find APN</button>
    <div id="apnResult"></div>

    <script>
    const apnData = {
            "Thailand": {
                "AIS": {
                    "apn": "internet",
                    "proxy": "proxy.ais.co.th",
                    "port": "8080",
                    "username": "ais",
                    "password": "ais",
                    "server": "ais.co.th"
                },
                "DTAC": {
                    "apn": "www.dtac.co.th",
                    "proxy": "proxy.dtac.co.th",
                    "port": "8080",
                    "username": "dtac",
                    "password": "dtac",
                    "server": "dtac.co.th"
                },
                "TrueMove": {
                    "apn": "internet",
                    "proxy": "proxy.truemove.co.th",
                    "port": "8080",
                    "username": "true",
                    "password": "true",
                    "server": "truemove.co.th"
                }
            },
            "Vietnam": {
                "Viettel": {
                    "apn": "v-internet",
                    "proxy": "proxy.viettel.com.vn",
                    "port": "8080",
                    "username": "viettel",
                    "password": "viettel",
                    "server": "viettel.com.vn"
                },
                "Mobifone": {
                    "apn": "m-wap",
                    "proxy": "proxy.mobifone.vn",
                    "port": "8080",
                    "username": "mobifone",
                    "password": "mobifone",
                    "server": "mobifone.vn"
                },
                "Vinaphone": {
                    "apn": "m3-world",
                    "proxy": "proxy.vinaphone.vn",
                    "port": "8080",
                    "username": "vinaphone",
                    "password": "vinaphone",
                    "server": "vinaphone.vn"
                }
            },
            "Malaysia": {
                "Celcom": {
                    "apn": "celcom3g",
                    "proxy": "proxy.celcom.net.my",
                    "port": "8080",
                    "username": "celcom",
                    "password": "celcom",
                    "server": "celcom.net.my"
                },
                "Maxis": {
                    "apn": "unet",
                    "proxy": "proxy.maxis.com.my",
                    "port": "8080",
                    "username": "maxis",
                    "password": "maxis",
                    "server": "maxis.com.my"
                },
                "Digi": {
                    "apn": "digi",
                    "proxy": "proxy.digi.com.my",
                    "port": "8080",
                    "username": "digi",
                    "password": "digi",
                    "server": "digi.com.my"
                }
            },
            "Singapore": {
                "Singtel": {
                    "apn": "e-ideas",
                    "proxy": "proxy.singtel.com.sg",
                    "port": "8080",
                    "username": "singtel",
                    "password": "singtel",
                    "server": "singtel.com.sg"
                },
                "StarHub": {
                    "apn": "shwap",
                    "proxy": "proxy.starhub.com.sg",
                    "port": "8080",
                    "username": "starhub",
                    "password": "starhub",
                    "server": "starhub.com.sg"
                },
                "M1": {
                    "apn": "sunsurf",
                    "proxy": "proxy.m1.com.sg",
                    "port": "8080",
                    "username": "m1",
                    "password": "m1",
                    "server": "m1.com.sg"
                }
            },
            "Indonesia": {
                "Telkomsel": {
                    "apn": "internet",
                    "proxy": "proxy.telkomsel.co.id",
                    "port": "8080",
                    "username": "telkomsel",
                    "password": "telkomsel",
                    "server": "telkomsel.co.id"
                },
                "Indosat": {
                    "apn": "indosatgprs",
                    "proxy": "proxy.indosat.com",
                    "port": "8080",
                    "username": "indosat",
                    "password": "indosat",
                    "server": "indosat.com"
                },
                "XL": {
                    "apn": "xlunlimited",
                    "proxy": "proxy.xl.co.id",
                    "port": "8080",
                    "username": "xl",
                    "password": "xl",
                    "server": "xl.co.id"
                }
            },
            "China": {
                "China Mobile": {
                    "apn": "cmnet",
                    "proxy": "10.0.0.172",
                    "port": "80",
                    "username": "",
                    "password": "",
                    "server": ""
                },
                "China Unicom": {
                    "apn": "3gnet",
                    "proxy": "10.0.0.172",
                    "port": "80",
                    "username": "",
                    "password": "",
                    "server": ""
                },
                "China Telecom": {
                    "apn": "ctnet",
                    "proxy": "",
                    "port": "",
                    "username": "",
                    "password": "",
                    "server": ""
                }
            },
            "Japan": {
                "NTT DoCoMo": {
                    "apn": "mopera.net",
                    "proxy": "",
                    "port": "",
                    "username": "",
                    "password": "",
                    "server": ""
                },
                "SoftBank": {
                    "apn": "softbank",
                    "proxy": "",
                    "port": "",
                    "username": "",
                    "password": "",
                    "server": ""
                },
                "KDDI": {
                    "apn": "au.kddi.com",
                    "proxy": "",
                    "port": "",
                    "username": "",
                    "password": "",
                    "server": ""
                }
            }
        };
        
        function updateProviders() {
            const country = document.getElementById('country').value;
            const providerSelect = document.getElementById('provider');
            providerSelect.innerHTML = '<option value="">--Select Provider--</option>';

            if (country && apnData[country]) {
                for (const provider in apnData[country]) {
                    const option = document.createElement('option');
                    option.value = provider;
                    option.textContent = provider;
                    providerSelect.appendChild(option);
                }
            }
        }

        function findAPN() {
            const country = document.getElementById('country').value;
            const provider = document.getElementById('provider').value;
            const apnResult = document.getElementById('apnResult');

            if (country && provider && apnData[country] && apnData[country][provider]) {
                const apnInfo = apnData[country][provider];
                apnResult.innerHTML = `
                    <p>APN for ${provider}, ${country}:</p>
                    <ul>
                        <li>APN: ${apnInfo.apn}</li>
                        <li>Proxy: ${apnInfo.proxy}</li>
                        <li>Port: ${apnInfo.port}</li>
                        <li>Username: ${apnInfo.username}</li>
                        <li>Password: ${apnInfo.password}</li>
                        <li>Server: ${apnInfo.server}</li>
                    </ul>
                `;
            } else {
                apnResult.innerHTML = "APN not found. Please select both a country and a provider.";
            }
        }

        // Function to create multiple shooting stars
        function createShootingStars() {
            for (let i = 0; i < 5; i++) {
                const star = document.createElement('div');
                star.className = 'shooting-star';
                star.style.left = `${Math.random() * 100}vw`;
                star.style.animationDelay = `${Math.random() * 3}s`;
                document.body.appendChild(star);
            }
        }

        // Create shooting stars on page load
        window.onload = createShootingStars;
    </script>
</body>
</html>
